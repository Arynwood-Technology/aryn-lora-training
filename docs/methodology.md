# Methodology

## Research question

Subject-identity LoRA fine-tunes for SDXL are trained against one specific
base checkpoint. Do the resulting weights transfer to *other* SDXL-family
checkpoints without retraining, and what — if anything — breaks when the
same LoRA is applied to a checkpoint of a different base architecture
(SD1.5)?

## Dataset

- 48 source images (self-portrait photography, mixed lighting/angle/quality —
  see project title *"Cloning an artist with 48 blurry screenshots"*).
- Images are not included in this repository; only the preparation pipeline
  and configuration are published.
- Preprocessing (`scripts/prepare_lora_dataset.py`): images are resized to a
  1024px long edge, padded/backgrounded, and auto-captioned with WD14
  (already vendored inside kohya_ss, tag-style output). WD14 was chosen over
  BLIP specifically because its comma-separated tag output composes cleanly
  with a single prepended trigger word (`trigger_word, tag1, tag2, ...`),
  which is the caption format kohya_ss LoRA training expects — BLIP's
  sentence-style captions do not.
- Trigger word: `Arynwood`. Kohya "n_repeats_trigger" folder convention:
  `10_Arynwood/`, i.e. each image is seen 10x per epoch.

## Training configuration

Run via `scripts/run_lora_training.py`, which wraps kohya_ss's
`sdxl_train_network.py` (SDXL-specific trainer — this pipeline does not
support SD1.5 targets; see Limitations).

| Parameter | Value |
|---|---|
| Base checkpoint | Juggernaut-XL-v9.safetensors |
| Network module | `networks.lora` |
| Network dim (rank) | 32 |
| Network alpha | 16 |
| Learning rate | 1e-4 |
| Resolution | 1024 |
| Train batch size | 1 |
| Max train epochs | 10 |
| Repeats per image | 10 |

Total run time: ~6h45m on the training host, producing one `.safetensors`
checkpoint per epoch plus a final combined output (`Arynwood_lora.safetensors`,
456 MB).

## Cross-checkpoint portability — technical basis

SDXL-family checkpoints (Juggernaut XL, DreamShaper XL Turbo, RealVisXL,
vanilla SDXL 1.0) share one fixed architecture: the same UNet layout, the
same dual text encoders (OpenCLIP-G + CLIP-L feeding 2048-dim cross-attention
conditioning), and the same VAE latent space. A LoRA's weight-delta tensors
are keyed to that shared architecture's layer names/shapes, not to any one
finetune's specific learned weights — so a LoRA trained against Juggernaut XL
loads without error on any other SDXL derivative.

SD1.5 is architecturally distinct: a single CLIP text encoder feeding
768-dim cross-attention, and a different UNet channel/block layout entirely.
None of an SDXL LoRA's keys resolve against an SD1.5 checkpoint's modules,
so the loader either silently skips the LoRA (no-op — output is
indistinguishable from not having selected it) or raises a load error,
depending on the inference backend's handling of unmatched keys. The trigger
word also carries no learned meaning in SD1.5's separate text-encoder
embedding space.

**Practical implication:** loading is guaranteed to work across SDXL
derivatives, but *fidelity* is not — the LoRA's deltas were computed against
Juggernaut XL's specific weights, so applying them to a distilled/Turbo
checkpoint (different sampling schedule, fewer inference steps) is expected
to produce a measurably weaker or drifted likeness than a checkpoint-specific
retrain would. This repo currently documents the mechanism; a quantitative
CLIP-similarity comparison across checkpoints is future work (see below).

## Limitations

- **Output-path collision.** The training pipeline keys its output directory
  and destination filename by trigger word + project id, not by base
  checkpoint. Retraining the same project against a different base
  checkpoint silently overwrites the previous checkpoint's output at the
  same path — there is currently no way to keep a Juggernaut-trained and a
  DreamShaper-Turbo-trained artifact side by side under one project. This is
  a pipeline gap, not an architectural one, and is fixable by suffixing the
  output filename with the base checkpoint key.
- No quantitative fidelity metric has been computed yet — the portability
  claims above are architectural (based on tensor key/shape matching), not
  yet empirically scored.

## Future work

1. Quantitative cross-checkpoint fidelity comparison (CLIP image-similarity
   between generations and reference photos) across Juggernaut XL,
   DreamShaper XL Turbo, and RealVisXL, same prompt/seed.
2. Fix the output-collision gap so per-checkpoint artifacts persist
   independently.
3. Add an SD1.5-targeted training path (kohya's plain `train_network.py` at
   512–768px) to test whether a *separately* trained SD1.5 LoRA reaches
   comparable fidelity to the SDXL version, as a control.

# Cross-Checkpoint Portability of Subject-Identity LoRA Fine-Tunes in SDXL

[![DOI](https://zenodo.org/badge/DOI/PLACEHOLDER.svg)](#) <!-- TODO: replace with the real Zenodo DOI badge after your first release -->

A small technical study of whether a subject-identity LoRA trained against
one SDXL-derivative checkpoint (Juggernaut XL) transfers to other SDXL
derivatives (e.g. DreamShaper XL Turbo) without retraining, and why it
architecturally cannot transfer to SD1.5.

Full write-up: [docs/methodology.md](docs/methodology.md)

## Contents

- `scripts/prepare_lora_dataset.py` — dataset preprocessing + WD14 auto-captioning
- `scripts/run_lora_training.py` — kohya_ss `sdxl_train_network.py` wrapper used for the training run
- `docs/methodology.md` — dataset, training config, and the cross-checkpoint portability analysis
- `weights/` — pointer to the trained `.safetensors` file (hosted on Zenodo, not in git — see below)

Training images are not included (personal photo dataset).

## Citing this work

See [CITATION.cff](CITATION.cff). Licensed under [CC-BY-4.0](LICENSE).

**Before your first publish:** `CITATION.cff` and `.zenodo.json` currently
list the author without an ORCID iD. Once you've registered one at
[orcid.org](https://orcid.org), add it to both files (uncomment the `orcid:`
line in `CITATION.cff`, add an `"orcid"` field to the `creators` entry in
`.zenodo.json`) — that's what actually links a published DOI back to your
ORCID profile as a "work."

## Publishing a new version

This repo is wired for Zenodo's GitHub integration, which is release-triggered,
not push-triggered — nothing publishes automatically on `git push`. A DOI is
only minted when *you* cut a GitHub Release.

**One-time setup (do this yourself — needs your own login):**

1. Push this repo to a public GitHub repo (see below).
2. Log into [zenodo.org](https://zenodo.org) with your GitHub account.
3. Go to your Zenodo GitHub settings (Account → GitHub), find this
   repository in the list, and flip its toggle on.

**Every time you want to publish a version:**

1. On GitHub, draft a new **Release** (tag it, e.g. `v0.1.0`) and click
   *Publish release*. Zenodo archives the repo at that tag and mints/updates
   a DOI automatically — this is the manual trigger, entirely under your
   control.
2. If this version should include the trained weights: go to the new Zenodo
   deposit (it starts as unpublished/editable right after step 1 fires),
   add `Arynwood_lora.safetensors` as an extra file via Zenodo's uploader,
   then publish the deposit. This is a separate manual drag-and-drop step
   by design — the weight file never touches git or GitHub.
3. Update `weights/README.md` with the resulting file's DOI/link, and the
   DOI badge at the top of this README, then commit that small update
   (does not require cutting another release).

## Repo scope note

This repository intentionally contains *only* the LoRA research artifact —
it does not include any code from the private commercial platform this
training pipeline was originally built inside.

# Cross-Checkpoint Portability of Subject-Identity LoRA Fine-Tunes in SDXL

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.21282393.svg)](https://doi.org/10.5281/zenodo.21282393)

A small technical study of whether a subject-identity LoRA trained against
one SDXL-derivative checkpoint (Juggernaut XL) transfers to other SDXL
derivatives (e.g. DreamShaper XL Turbo) without retraining, and why it
architecturally cannot transfer to SD1.5.

Full write-up: [docs/methodology.md](docs/methodology.md)

## Contents

- `scripts/prepare_lora_dataset.py` — dataset preprocessing + WD14 auto-captioning
- `scripts/run_lora_training.py` — kohya_ss `sdxl_train_network.py` wrapper used for the training run
- `docs/methodology.md` — dataset, training config, and the cross-checkpoint portability analysis
- `weights/` — pointer to the two trained `.safetensors` files (hosted on Zenodo, not in git — see [`weights/README.md`](weights/README.md))

Training images are not included (personal photo dataset).

## Citing this work

See [CITATION.cff](CITATION.cff). Licensed under [CC-BY-4.0](LICENSE).

Author ORCID iD: [0009-0007-8723-2857](https://orcid.org/0009-0007-8723-2857)
— set in both `CITATION.cff` and `.zenodo.json`, so a published DOI will
link back to the ORCID profile as a "work" automatically.

## Releases & DOIs

This repository is connected to Zenodo's GitHub integration, which is
release-triggered rather than push-triggered: a DOI is minted only when a
GitHub Release is published here, never automatically on a commit.

- **Concept DOI** (always resolves to the latest version, use this one for
  citing the project generally): [10.5281/zenodo.21282393](https://doi.org/10.5281/zenodo.21282393)
- **Current version DOI** (v0.2.0): [10.5281/zenodo.21282597](https://doi.org/10.5281/zenodo.21282597)

v0.1.1 was code/methodology only. v0.2.0 adds both trained LoRA checkpoints
(Juggernaut XL and DreamShaper XL Turbo) as direct uploads to the Zenodo
deposit — see [`weights/README.md`](weights/README.md) for file details and
checksums. As with the code, this was a manual, deliberate publish, not
something that happens automatically on push.

Publishing a further version: draft a GitHub Release (tagged, e.g.
`v0.3.0`) and publish it — Zenodo archives the repository at that tag and
adds a new version under the same concept DOI. Note that a GitHub Release
only archives tracked git content; any additional large files (like the
weights) still need a separate manual upload directly to that version's
Zenodo deposit.

## Scope

This repository contains the LoRA research artifact only — training
scripts, configuration, and methodology. It does not include the private
platform this pipeline was originally developed inside, nor the personal
photo dataset used for training.

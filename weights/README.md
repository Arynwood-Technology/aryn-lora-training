# Trained weights

Two distinct SDXL LoRA checkpoints came out of this project — trained in
separate runs against two different base checkpoints, using the same
trigger word (`Arynwood`). Both are published on Zenodo as of v0.2.0
([10.5281/zenodo.21282597](https://doi.org/10.5281/zenodo.21282597), under
the concept DOI [10.5281/zenodo.21282393](https://doi.org/10.5281/zenodo.21282393)):

- **`Arynwood_lora_juggernaut_XL.safetensors`** — 456 MB, trained against
  `Juggernaut-XL-v9.safetensors`. This is the run documented in
  [../docs/methodology.md](../docs/methodology.md) (network dim 32, alpha
  16, lr 1e-4, 10 epochs, 1024px). Provenance verified: matched against the
  training database's recorded base checkpoint and completion timestamp,
  and checksummed against the author's separately downloaded copy.
  MD5: `4375008c258dbdc245009f6e4b17f1e3`
- **`Arynwood_lora_dreamshaper_turbo.safetensors`** — 456 MB, from an
  earlier training run against DreamShaper XL Turbo. The local pipeline's
  output path is keyed by trigger word and project id only, not by base
  checkpoint (see *Limitations* in `docs/methodology.md`), so this run's
  output was silently overwritten on disk when the Juggernaut run reused
  the same project. The file published here was recovered from a
  deleted-files backup and renamed by hand — provenance is not
  independently verified the way the Juggernaut file's is, though its
  checksum confirms it is a genuinely distinct file, not a duplicate.
  MD5: `6e8cfbb535c1e76e3a4a7fb251147c67`

Neither file is stored in this git repository — both are too large for a
normal git push and don't belong in version control. They were uploaded
directly to the Zenodo deposit's file list, not through GitHub (GitHub
Release binary attachments don't flow into Zenodo automatically — only the
tracked git source at the release tag does).

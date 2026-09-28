# Repository operating rules

This repository publishes data from Energy Atlas projects that can be shared publicly.
Everything committed here is treated as public, even while the repository is private.

## What may be published

- Only data whose sources allow redistribution, checked source by source and recorded in
  the dataset's data card with the licence, the required attribution and the date checked.
  When a source's terms are unclear, leave that source out and record why.
- Never commit credentials, tokens, private URLs, personal data, raw provider downloads
  whose terms forbid redistribution, or project-internal notes.
- Keep derived data honest: state how it was produced, from which source versions, and
  what it does not establish. No fabricated or unverified values.

## Dataset structure

- One folder per dataset under `datasets/`, named in lower-case kebab-case.
- Each dataset has a `README.md` data card (contents, sources, licences, attribution,
  method, known limits, version history) and a `datapackage.json` (Frictionless Data
  Package) listing every file with its schema, format, byte size and SHA-256.
- Prefer open, typed, machine-friendly formats: Parquet (GeoParquet for located records)
  for large tables, CSV for small flat tables, JSON for metadata. Use archives only for
  convenience copies, never as the only form.
- Record provenance per record where the source varies within a dataset.

## Versions and size

- Version each dataset (dated or semantic) in its data card and `datapackage.json`;
  never rewrite a published version in place.
- Keep individual files under 50 MB. Larger files or frequent binary updates go to
  release assets, with their checksums recorded here.

## Git

- Use the normal configured human authorship; no agent or model co-authors or trailers.
- Commit messages: `fix(topic): concise description`.
- Use purpose-based branch names such as `data/...`, `docs/...` or `fix/...`.
- No force pushes, history rewrites or branch deletions without explicit owner approval.
- If a secret or non-redistributable file is found in history, stop and tell the owner;
  do not rewrite history on your own.

## Verification

- Before committing a dataset, check that every file in `datapackage.json` exists and
  matches its size and SHA-256, and that each schema matches its file.
- Report what was checked and any limits, in the data card and in the commit.

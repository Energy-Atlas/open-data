# Claude instructions

Read [AGENTS.md](AGENTS.md) before any change; its rules apply in full. In short:

- Everything here is public. Only publish data whose sources allow redistribution, with
  the licence, attribution and review recorded in the dataset's data card.
- Each dataset lives in `datasets/<dataset>/` with a `README.md` data card, a
  `datapackage.json` (files, schemas, sizes, SHA-256) and its data files in `data/`.
- Prefer Parquet, CSV and JSON over opaque archives.
- Commit as the configured human author, with no agent or model co-authors or trailers.

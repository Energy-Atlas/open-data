# Energy Atlas open data

Data from Energy Atlas projects that can be shared publicly: catalogs, reference tables
and derived datasets, published in open, machine-friendly formats with their provenance,
licences and checksums.

Nothing here is confidential. Each dataset states where its data came from, under which
terms it can be reused, and how it was produced, so it can be reused without the project
that made it.

## Layout

```text
datasets/<dataset>/     one folder per dataset
  README.md             the data card: contents, sources, licence, attribution, versions
  datapackage.json      machine-readable description: files, schemas, checksums
  data/                 the data files
schemas/                shared schemas referenced by several datasets
scripts/                build and validation scripts
docs/                   policies: what may be published, formats, versioning
```

## Using a dataset

Start with the dataset's `README.md` for what it contains and how it may be reused, and
its `datapackage.json` for the files, their columns and SHA-256 checksums. Cite the
original sources listed there, not only this repository.

## Contributing

Read [AGENTS.md](AGENTS.md) before adding or changing data. A dataset is added only after
its sources have been checked for redistribution, and every file is listed with a
checksum in its `datapackage.json`.

## Licence

There is no single licence for the repository: each dataset carries the terms of its
sources, stated in its data card. A licence for the scripts is still to be chosen.

# Publishing a dataset

1. **Check the sources.** For each source, record its licence or terms, whether
   redistribution is allowed, the attribution it requires, and the date checked. Leave out
   any source whose terms do not allow redistribution or are unclear.
2. **Choose formats.** Parquet (GeoParquet for located records) for large tables, CSV for
   small flat tables, JSON for metadata. Keep one row per record, typed columns, and a
   source column where records come from different sources.
3. **Describe it.** Write the data card (`README.md`) and `datapackage.json` with every
   file's schema, byte size and SHA-256.
4. **Verify.** Re-read each file and compare it with its schema and checksum.
5. **Version.** Record the version and what changed; never replace a published version.

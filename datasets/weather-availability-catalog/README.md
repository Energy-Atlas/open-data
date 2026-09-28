# Weather availability catalog

Which weather products are available where, and for which years: weather-station
records, published typical-year EPW files, reanalysis, solar and climate-projection
products. Every statement links to the evidence it rests on. The catalog holds
**metadata only**, with no weather data: station lists, coordinates, published file names and
links, years and hourly record counts, and licence fields.

It is the Stage 1 availability catalog of [openepw](https://github.com/Energy-Atlas/openepw),
exported with `openepw catalog export`. A hosted openepw builds its catalog from this
package (`openepw catalog import --from-package datapackage.json`, or on first start from
a pinned URL and SHA-256; see openepw's `docs/deploy/render.md`).

| | |
|---|---|
| Version | 2026.09.28 |
| Sources read | 2026-09-23 to 2026-09-24 (UTC), per row in `data/sources.csv` |
| Size | 44.1 MB in 29 files, the largest 12.2 MB |
| Format | CSV (UTF-8, comma, header row) and JSON, described in `datapackage.json` |

## Files

One folder per provider, so each source's terms apply to its own files.

| File | Rows | One row per |
|---|---:|---|
| `data/sources.csv` | 38 | evidence file or page read: URL, SHA-256, retrieval time, ETag |
| `data/reviews.csv` | 61 | owner-reviewed OneBuilding location decision |
| `data/noaa/products.csv` | 1 | product (NOAA ISD global-hourly) |
| `data/noaa/sites.csv` | 29,508 | ISD station |
| `data/noaa/entries.csv` | 16,513 | station with hourly records (years with data) |
| `data/noaa/month-counts.csv` | 154,841 | station-year: hourly records per month |
| `data/onebuilding/products.csv` | 21,653 | published EPW file (TMYx and others) |
| `data/onebuilding/sites.csv` | 21,653 | location of that file |
| `data/onebuilding/entries.csv` | 21,653 | typical-year reference period of that file |
| `data/oedi/{products,sites,entries}.csv` | 2 / 4,736 / 9,472 | Argonne downscaled future weather, by grid site and scenario |
| `data/cmip6/{products,entries}.csv` | 636 / 636 | CMIP6 model, scenario and member with its future window |
| `data/nsrdb/{products,entries}.csv` | 4 / 36 | NSRDB products and their years or typical-year labels |
| `data/cds`, `data/openmeteo`, `data/pvgis` | 1-2 each | gridded products and their periods |
| `data/footprints/nsrdb/.../mask.json` | | NSRDB typical-year coverage mask with its manifest |

Every CSV has a `sites.csv` and `entries.csv`, even when empty, so each provider looks the
same. Column meanings, types and keys are in `datapackage.json`.

**Reading the cells:** an empty cell means missing. Lists and mappings are JSON (for
example `["noaa-history"]`). Dates are `YYYY-MM-DD`, times ISO 8601 UTC. Numbers keep full
precision. `entries.csv` covers three kinds of scope, given in `scope_kind`:

- `actual`: years with records (`years`), or an available interval (`operating_start`,
  `operating_end`); per-month hourly counts are in `month-counts.csv`.
- `tmy_reference`: a typical-year file's period (`start_year`–`end_year`) and label, and
  the year chosen for each month where published.
- `future_window`: a projection's scenario, model, member, grid and period.

`evidence_ids` in any table refer to `id` in `data/sources.csv`.

## Sources, terms and attribution

Checked 2026-09-28. Cite the original sources, not only this repository.

| Provider | What is used here | Terms | Attribution |
|---|---|---|---|
| NOAA NCEI | ISD `isd-history.csv` (stations) and `isd-inventory.csv` (record counts) | US Government work; no copyright restrictions on the metadata. Some non-US ISD *observations* carry WMO Resolution 40 limits; no observations are included here. | NOAA National Centers for Environmental Information, Integrated Surface Database |
| OneBuilding | Published EPW catalog pages and the location spreadsheets on climate.onebuilding.org | **Unclear.** The site offers the files for download and asks for citation, but its footer reads all rights reserved and it grants no general redistribution licence. Published here by owner decision (2026-09-28); see Known limits. | climate.onebuilding.org (Lawrie and Crawley) |
| NLR NSRDB | Product list and years from the NSRDB data query API | Offered publicly without charge; no formal licence stated. | National Solar Radiation Database, National Laboratory of the Rockies (formerly NREL) |
| Copernicus CDS | ERA5 and ERA5-Land catalogue records | CC BY 4.0, as declared in the collection records | Contains modified Copernicus Climate Change Service information 2026 |
| Open-Meteo | Historical weather API documentation | API data under CC BY 4.0 | Weather data by Open-Meteo.com |
| PVGIS (EC JRC) | One typical-year probe (London) | Free for public use if the source is acknowledged | PVGIS © European Communities, 2001-2026 |
| CMIP6 (WCRP) | Pangeo CMIP6 catalog and the WCRP source-id licence registry | Per model: CC BY 4.0 (596 products) or CC0 1.0 (40), in `license_effective` | The modelling group of each `source_id`, and WCRP CMIP6 |
| OEDI 5974 (Argonne) | Site table and file directories of the downscaled future weather dataset | CC BY 4.0 | Zeng, Kim, Wang and Muehleisen, Argonne National Laboratory, doi:10.25984/2202668 |

The `datapackage.json` `licenses` list summarises these; the table above is authoritative.

## Method

openepw's Stage 1 importer read each source listed in `data/sources.csv`, kept its SHA-256
and HTTP validators, and recorded only what the source states: published coordinates and
names, listed years and record counts, published file names. OneBuilding locations that
could not be matched to a station were reviewed by the owner (`data/reviews.csv`). The
export is lossless: importing it rebuilds the same catalog record for record, which was
checked on this release (all 38 sources, 22,301 products, 55,897 sites, 48,315 entries and
61 reviews identical, footprints byte-identical).

## Known limits

- **Availability is evidence, not a guarantee.** Counts and years describe the source at
  the retrieval time; sources change afterwards.
- **Coordinates are as published.** Some ISD stations publish placeholder positions (for
  example 0, 0); `position_status` says how a position is known, and approximate
  OneBuilding localities are not exact station positions.
- **OneBuilding terms are unresolved.** Redistribution of this metadata was not granted
  explicitly. If OneBuilding asks, remove `data/onebuilding/` and `data/reviews.csv` in a
  new version. This repository's rule to leave out sources with unclear terms was
  overridden by the owner for this release.
- No weather observations or EPW files are included; download them from each provider
  under its own terms.

## Versions

| Version | Date | Changes |
|---|---|---|
| 2026.09.28 | 2026-09-28 | First release, from openepw catalog generation `0a09bde8`. |

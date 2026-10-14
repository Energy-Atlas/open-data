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
| Version | 2026.10.13.2 |
| Sources read | 2026-09-23 to 2026-10-13 (UTC), per row in `data/sources.csv` |
| Size | 88.2 MB in 39 files, the largest 31.2 MB |
| Format | CSV (UTF-8, comma, header row) and JSON, described in `datapackage.json` |

## Files

One folder per provider, so each source's terms apply to its own files.

| File | Rows | One row per |
|---|---:|---|
| `data/sources.csv` | 70 | evidence file or page read: URL, SHA-256, retrieval time, ETag |
| `data/reviews.csv` | 61 | owner-reviewed OneBuilding location decision |
| `data/noaa/products.csv` | 1 | product (NOAA ISD global-hourly) |
| `data/noaa/sites.csv` | 29,508 | ISD station |
| `data/noaa/entries.csv` | 16,513 | station with hourly records (years with data) |
| `data/noaa/month-counts.csv` | 154,841 | station-year: hourly records per month |
| `data/onebuilding/products.csv` | 72,747 | published EPW file (TMYx and others) |
| `data/onebuilding/sites.csv` | 72,747 | location of that file |
| `data/onebuilding/entries.csv` | 72,747 | typical-year reference period of that file |
| `data/oedi/{products,sites,entries}.csv` | 2 / 4,736 / 9,472 | Argonne downscaled future weather, by grid site and scenario |
| `data/cmip6/{products,entries}.csv` | 636 / 636 | CMIP6 model, scenario and member with its future window |
| `data/nsrdb/{products,entries}.csv` | 14 / 207 | NSRDB products and their years or typical-year labels, per probe and, where every probe agrees, documented for the whole mask |
| `data/cds`, `data/openmeteo`, `data/pvgis` | 1-4 each | gridded products and their periods; PVGIS has one entry per probe and one documented period |
| `data/footprints/nsrdb/.../mask.json` | | NSRDB coverage masks (0.25° grid-site cells) for GOES, Himawari, Meteosat and Meteosat IODC, each with its manifest |
| `data/footprints/cds/reanalysis-era5-land/present-t2m/mask.json` | | ERA5-Land mask: the 0.1° points where ERA5-Land has a value (row runs), with its manifest |
| `data/footprints/pvgis/tmy-v5_3/domain/mask.json` | | PVGIS 5.3 TMY domain (0.1° row runs: `runs` inside, `edge_runs` unknown, the rest outside) with its manifest |

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

Checked 2026-09-28; PVGIS rechecked 2026-10-13 on its data-download page. Cite the original sources, not only this repository.

| Provider | What is used here | Terms | Attribution |
|---|---|---|---|
| NOAA NCEI | ISD `isd-history.csv` (stations) and `isd-inventory.csv` (record counts) | US Government work; no copyright restrictions on the metadata. Some non-US ISD *observations* carry WMO Resolution 40 limits; no observations are included here. | NOAA National Centers for Environmental Information, Integrated Surface Database |
| OneBuilding | Published EPW catalog pages and the TMYx location spreadsheets of every WMO region on climate.onebuilding.org | **Unclear.** The site offers the files for download and asks for citation, but its footer reads all rights reserved and it grants no general redistribution licence. Published here by owner decision (2026-09-28, extended to all regions 2026-10-13); see Known limits. | climate.onebuilding.org (Lawrie and Crawley) |
| NLR NSRDB | Product list and years from the NSRDB data query API at 15 points, and the documented years of nine dataset pages | Offered publicly without charge; no formal licence stated. | National Solar Radiation Database, National Laboratory of the Rockies (formerly NREL) |
| Copernicus CDS | ERA5 and ERA5-Land catalogue records | CC BY 4.0, as declared in the collection records | Contains modified Copernicus Climate Change Service information 2026 |
| ECMWF / Copernicus (ERA5-Land) | The ERA5-Land land-sea mask (used for the PVGIS domain), and one ERA5-Land 2 m temperature field from CDS reduced to the points with values | CC BY 4.0 (CDS collection record) | Contains modified Copernicus Climate Change Service information 2026 |
| Open-Meteo | Historical weather API documentation | API data under CC BY 4.0 | Weather data by Open-Meteo.com |
| PVGIS (EC JRC) | Six typical-year requests: three answered (London, Ithaca, Lake Victoria) and three refused as sea (open Atlantic, North Sea, Svalbard); the PVGIS 5.3 dataset report (JRC139355) for the documented domain | Free for public use if the source is acknowledged | PVGIS © European Communities, 2001-2026 |
| CMIP6 (WCRP) | Pangeo CMIP6 catalog and the WCRP source-id licence registry | Per model: CC BY 4.0 (596 products) or CC0 1.0 (40), in `license_effective` | The modelling group of each `source_id`, and WCRP CMIP6 |
| OEDI 5974 (Argonne) | Site table and file directories of the downscaled future weather dataset | CC BY 4.0 | Zeng, Kim, Wang and Muehleisen, Argonne National Laboratory, doi:10.25984/2202668 |

The `datapackage.json` `licenses` list summarises these; the table above is authoritative.

## Method

openepw's Stage 1 importer read each source listed in `data/sources.csv`, kept its SHA-256
and HTTP validators, and recorded only what the source states: published coordinates and
names, listed years and record counts, published file names. OneBuilding locations that
could not be matched to a station were reviewed by the owner (`data/reviews.csv`).

Version 2026.10.13 adds:
- **OneBuilding:** every TMYx file listed in the ten regional location spreadsheets, with the
  coordinates published there (51,094 more files).
- **NSRDB:** three more data-query points, and coverage masks for Himawari, Meteosat and Meteosat IODC from each satellite's published grid table.
- **ERA5-Land:** a land mask from ECMWF's land-sea mask (land fraction ≥ 0.5, decoded from GRIB).

Version 2026.10.13.1 adds:
- **PVGIS 5.3 TMY:** one product whose radiation is SARAH3 or ERA5 by location, as PVGIS reports
  it. There is a documented 2005–2023 period and a domain mask built from the same ECMWF
  land-sea mask:
  - inside: land between 60° S and 75° N;
  - outside: open sea more than 50 km from any land or published station, and beyond those
    latitudes;
  - unknown: the coasts, lakes and small islands between.
- **Probe evidence:** two probes that returned a TMY (Ithaca with ERA5 radiation, Lake
  Victoria with SARAH3).

Version 2026.10.13.2 adds:
- **NSRDB:**
  - data-query probes at ten grid edges, from Honolulu to Almaty;
  - the documented years of each adapted dataset's NLR page;
  - where every probe agrees and the page lists nothing more, an entry for the whole coverage
    mask;
  - two upstream-only products, NSRDB Polar and Polar TMY.
- **ERA5-Land:** the mask is now the points where ERA5-Land has a value, which takes in lakes,
  from one CDS field. It replaces the land-fraction mask.

The requests, checks and limits are in openepw's
`docs/validation/availability-coverage-2026-10-13/README.md` and, for 2026.10.13.1,
`docs/validation/availability-gaps-2026-10-13/README.md`. The export is lossless: importing it
rebuilds the same catalog record for record. This was checked on this release: all 70
sources, 73,405 products, 106,991 sites, 99,583 entries and 61 reviews are identical, and the
12 footprint files are byte-identical.

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
- **The ERA5-Land mask comes from one field at one hour.** ERA5-Land's points with values are
  fixed by its model grid, so the hour does not matter. Open-Meteo's ERA5-Land is gated by the
  same mask.
- **NSRDB years are documented, not checked at every site.**
  - Fifteen probes, including ten at grid edges, list the same years per dataset.
  - Four earliest-year downloads at edges hold full data.
  - NLR's pages can lag the data query: the GOES TMY page omits the -2025 files every probe
    lists.
- **The PVGIS domain mask is documented and spot-checked, not a PVGIS raster.**
  - PVGIS uses its own coastline. JRC documents a 25 km sea zone, but a point about 19 km off
    the Dutch coast was refused.
  - So coastal points whose nearest 0.1° cell is mostly water stay unknown. This is 17% of
    published station positions inside 60° S–75° N, including Cape Town, Rio de Janeiro,
    Singapore and Honolulu.
  - Lakes stay unknown too, although Lake Victoria was served.
- No weather observations or EPW files are included; download them from each provider
  under its own terms.

## Versions

| Version | Date | Changes |
|---|---|---|
| 2026.09.28 | 2026-09-28 | First release, from openepw catalog generation `0a09bde8`. |
| 2026.10.13 | 2026-10-13 | OneBuilding TMYx files of every WMO region, NSRDB probes at Singapore, Nairobi and London with Himawari, Meteosat and IODC coverage masks, the ERA5-Land land mask. |
| 2026.10.13.1 | 2026-10-13 | PVGIS 5.3 TMY: SARAH3 or ERA5 radiation by location, a documented 2005–2023 period, the domain mask, and probes at Ithaca and Lake Victoria. |
| 2026.10.13.2 | 2026-10-13 | NSRDB: ten edge probes, documented years for each coverage mask, Polar products listed. ERA5-Land: the mask of points with values replaces the land-fraction mask. |

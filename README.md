# NZ retirement policy model

An R/Shiny scenario model for exploring selected changes to New Zealand Superannuation (NZ Super). It was commissioned by Te Ara Ahunga Ora Retirement Commission and developed by Heuser | Whittington.

The model supports policy exploration and discussion. It is not an official government forecast, costing, or policy position.

## Live tool and hosting

Live tool: https://retirement-commission.github.io/retirement-policy-model/

The live tool is hosted on GitHub Pages from this repository (branch `main`, folder `/docs`). The `docs/` folder contains a Shinylive export of the app: R runs in the user's own web browser, so no server is involved. The first load downloads R into the browser and can take up to a minute; later visits are faster.

## How to update the live tool

Every commit to `main` that changes `docs/` goes live, so test changes before committing.

1. Make and test changes in `app/` locally: `renv::restore()`, then `shiny::runApp("app")`, then `source("scripts/check-release.R")`.
2. Build the static site: `source("scripts/export-shinylive.R")`. This creates a `site/` folder.
3. Replace the contents of `docs/` with the contents of `site/`, **except** `docs/architecture.md` and `docs/public-source-manifest.csv`, which are documentation and must be kept.
4. Commit and push to `main`. Because the export contains hundreds of files, use Git or GitHub Desktop rather than GitHub's web upload (which is limited to 100 files at a time).
5. GitHub Pages republishes automatically within a few minutes. Check the live tool afterwards.

## Ownership and history

- Owned by Te Ara Ahunga Ora Retirement Commission. Business owner and technical owner: Te Ara Ahunga Ora Retirement Commission.
- Commissioned by Te Ara Ahunga Ora Retirement Commission and developed by Heuser | Whittington.
- Handover documents (methodology, maintenance instructions, end-user guidance) and the raw public-data bundle: location TBC.
- Contact: office@retirement.govt.nz

## What is included

- Fiscal impacts: aggregate NZ Super costs, annual saving decomposition, and net-present-value summaries.
- Cohort impacts: reform transition effects, eligibility-age deferral, and illustrative income-test incidence.
- Person impacts: illustrative working-life earnings, KiwiSaver accumulation, retirement-income composition, and contribution-rate sensitivity.
- An Excel export for the selected scenario.

## Run locally

From the repository root, restore the pinned R environment and start the model:

```r
renv::restore()
shiny::runApp("app")
```

Run the automated release check before accepting a code or prepared-data change:

```r
source("scripts/check-release.R")
```

The check runs the test suite, confirms that the Shiny entry point loads, and reports the approved numerical release fixtures.

## Build a static site

The model can be exported as a static Shinylive site for a hosting route chosen by the Retirement Commission:

```r
source("scripts/export-shinylive.R")
httpuv::runStaticServer("site")
```

`site/` is a disposable build output and is deliberately excluded from version control. The Retirement Commission publishes the live tool via GitHub Pages; see "Live tool and hosting" above.

## Repository structure

| Location | Purpose |
| --- | --- |
| `app/` | Shiny entry point, model/UI code, prepared data files, and static assets. |
| `app/R/model/` | Testable fiscal, cohort, person-impact, rate-path, and export logic. |
| `app/R/ui/` | Layout, theme, plotting, formatting, and display helpers. |
| `app/data/` | Compact prepared data files used by the released model. |
| `scripts/` | R scripts for validation, public-source preparation, and Shinylive export. |
| `tests/` | Unit, contract, and approved release-fixture tests. |
| `docs/` | Architecture note and public-source manifest. |

## Data refresh boundary

Normal model use requires only the prepared data files shipped in `app/data/`. Raw source files are deliberately outside Git and are not a deployment prerequisite.

A maintainer can rebuild prepared data files from the separately supplied public-data bundle:

```r
source("scripts/prepare-app-data.R")
prepare_app_data("C:/path/to/public-data-bundle")
```

The required source files, publisher links, data versions, refresh guidance, redistribution notes, checksums, and downstream prepared data files are listed in [`docs/public-source-manifest.csv`](docs/public-source-manifest.csv). To confirm a refresh exactly reproduces the shipped inputs, set `NZS_PUBLIC_SOURCE_DIR` and run `scripts/check-release.R`.

Detailed methodology, maintenance instructions, and end-user guidance are supplied as separate handover documents.

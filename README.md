# Kids Over Profits data

The datasets behind https://kidsoverprofits.org, in the layout the site reads
them: this repository is deployed as the theme's `js/data/` folder. Free to
reuse under [CC BY-SA 4.0](DATA-LICENSE.md); credit "Kids Over Profits
(https://kidsoverprofits.org)" and share what you build from it under the same
licence. Bulk downloads of the facility, inspection, lawsuit, legislation and
memorial records (which live in the site's database, not here) are at
https://kidsoverprofits.org/open-data/.

The code that reads these files is the theme, `Kids-Over-Profits`; the tools
that build them are in `kop-pipeline` (private). A folder's builder is named
below so a change can be traced.

## What is here

| Folder or file | What it is | Built by |
| --- | --- | --- |
| `network/` | The network map: the board export (`tti_nodes.csv`, `tti_edges.csv`), hand corrections (`network-overrides.json`), reviewed staff moves (`staff-movement.csv`, `staff-list.csv`, `sequel-staff.csv`), and the built `graph.json` + `layout.json` the map draws. `rename-candidates.json` and `years-candidates.json` are research results reviewed on the site. | `scripts/build-network-graph.js`, `build-network-layout.js`; a GitHub Actions run in kop-pipeline rebuilds them when the records change |
| `reporting/` | Where to report an abusive therapist or program, state by state: `national.json`, `states/<abbr>.json`, the built `directory.json`, CHILD USA's deadlines in `childusa-sol.json`. `README.md` there is the field-by-field schema. | `scripts/build-reporting-directory.js`, `pull-childusa-sol.js` |
| `reddit-wiki/` | The r/troubledteens wiki's program lists by state (`programs-<ST>.json`), their aggregate (`programs-array.json`, `search-index.json`, `index.json`), the pages' addresses (`page-urls.json`), the live-page comparison (`live-compare.json`), HEAL's archived addresses and the drafted wiki updates (`update-drafts.json`). | `scripts/aggregate-all-programs.js`, `reddit-wiki-live.py`, `wiki-drafts.py`, `build-wiki-page-urls.py` |
| `people/` | Name variants reviewed by hand (`name-variants-reviewed.json`) and nicknames (`nicknames.json`). | by hand |
| `proper-names.json` | Names always capitalised as written (NATSAP, MacLaren, LLC); the site keeps its own copy once edited there. | by hand |
| `tti-program-links.json` | Program names not yet filed under a state. | `scripts/extract-reddit-wiki-links.js` |
| `survivor-archives/` | Documents on survivor-run sites, filed under the program they concern, one folder per site; `privacy.json` holds only hashes of addresses. | `scripts/survivor-archives.py` |
| `unsilenced/` | Unsilenced's archive, listed per program for documents the site has no copy of. | `scripts/build-unsilenced-links.py` |
| `data-audit/`, `federal-funding/`, `featured/` | Proposals reviewed on the site: data corrections, federal money found for programs, nominations for essential reporting. | `scripts/data-audit.py`; by hand |
| `az_reports/`, `ut_checklists/`, `ccl_reports_batch_*.json`, `ct_`/`mn_`/`mt_`/`ut_`/`wa_reports.json` | Early inspection scrapes kept as the site's fallback when the database has no rows for a state. The current data is in the site's database, scraped by `kop-pipeline/scrapers/`. | the state scrapers |
| `facility-projects-export*.json`, `combined_index.json` | Older exports a few admin tools still read. | the site |

Two folders of the site's `js/data/` are not here because they are page
content edited on the site: `pages/` and `glossary/` stay in the theme.

## Deploying

A push to `main` runs `.github/workflows/deploy.yml`, which asks cPanel to pull
this repository and run `.cpanel.yml` (copies everything into the theme's
`js/data/`). The theme deploys the same way from its own repository; the two
never touch each other's files.

## Working on it locally

Clone this next to the theme and the pipeline:

```
source/repos/
  Kids-Over-Profits/   the theme
  kop-data/            this repo
  kop-pipeline/        builders, scrapers, research tools
```

The theme's offline tests expect these files at `js/data/`: run
`scripts/link-data.ps1` in the theme checkout once, and it makes a junction
per folder. The pipeline's scripts find this checkout by its default location
or `KOP_DATA_DIR`.

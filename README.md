# Trichy East Election Data Pipeline

A Databricks medallion-architecture portfolio project for Tamil Nadu Assembly
Constituency 141, Tiruchirappalli East, covering the 2016, 2021, and 2026
elections.

## Project Goal

Build an auditable pipeline from inconsistent election PDFs and CSV exports to
analysis-ready booth and candidate datasets. The project deliberately preserves
raw source evidence before applying candidate mapping, vote reconciliation, or
other election logic.

## Why This Project Is Interesting

The source documents vary by year:

- Form 20 layouts and candidate counts change.
- Some PDFs contain usable text while others require OCR.
- Candidate ordering is constituency-and-year specific.
- Candidate totals include postal votes, while polling-booth rows contain
  general votes.
- The 2026 candidate CSV is exported as one quoted field per row and must be
  preserved before repair.

These are realistic data-engineering problems: schema drift, mixed extraction
methods, source reconciliation, and traceable error handling.

## Bronze Design

Bronze is source-aligned. It records what arrived and what the extractor saw,
without calculating winners, vote share, postal votes, or normalized candidate
identities.

| Delta table | Grain |
|---|---|
| `bronze.trichy_east_boothwise_votes_raw` | One extracted Form 20 row, or one failure record per page |
| `bronze.trichy_east_candidate_totals_raw` | One extracted official-total row, including summary rows |
| `bronze.trichy_east_polling_booths_raw` | One extracted polling-station row |
| `bronze.trichy_east_candidate_list_raw` | One extracted candidate-list row |
| `bronze.trichy_east_ingestion_audit` | One source file per ingestion run |

NOTA is intentionally not a separate Bronze table. There is no independent NOTA
source file: booth NOTA is present in Form 20 and constituency NOTA is present in
official totals. A dedicated NOTA dataset belongs in Silver, where those raw
records can be reconciled without duplicating Bronze evidence.

## Notebook Order

1. `00_source_inventory` - register files and verify accessibility.
2. `01_bronze_boothwise_votes` - land Form 20 rows and page failures.
3. `02_bronze_candidate_totals` - land official candidate and summary totals.
4. `03_bronze_polling_booths` - land polling-station reference rows.
5. `04_bronze_candidate_list` - land candidate positions, names, parties, symbols.
6. `05_bronze_quality_checks` - profile coverage, failures, duplicates, and volume.

## Repository Layout

```text
config/      Source manifest and runtime configuration
docs/        Architecture decisions and source inventory
notebooks/   Databricks notebooks in execution order
src/         Shared extraction and validation helpers
tests/       Unit tests for stable parsing helpers
```

## Current Scope

- Constituency: `141 - Tiruchirappalli East`
- Election years: `2016`, `2021`, `2026`
- Platform: Databricks with Delta Lake and Unity Catalog Volumes
- Current phase: Bronze source ingestion

# Data Architecture

## Goal

Build an auditable Databricks pipeline for Tiruchirappalli East Assembly election data covering 2016, 2021, and 2026.

## Bronze Principle

Bronze preserves what arrived and what the extractor observed.

It does not:

- Calculate winners or vote share
- Standardize candidate identities
- Derive postal votes
- Join candidate names to Form 20 vote positions
- Apply election business logic

## Bronze Tables

### `bronze.trichy_east_boothwise_votes_raw`

Grain: one extracted Form 20 row, or one failure record for an unparseable page.

### `bronze.trichy_east_candidate_totals_raw`

Grain: one extracted candidate or summary row from the official result source.

### `bronze.trichy_east_polling_booths_raw`

Grain: one extracted polling-station entry.

### `bronze.trichy_east_candidate_list_raw`

Grain: one extracted candidate-list row.

### `bronze.trichy_east_ingestion_audit`

Grain: one source file per ingestion run.

## NOTA Design

NOTA is not a separate Bronze table because it is already present in:

- Form 20 booth-level records
- Official constituency result records

A dedicated NOTA dataset will be created later in Silver.

## Extraction Strategy

1. Attempt native PDF text extraction.
2. Use OCR when native extraction produces no usable content.
3. Write a failure record when extraction is unsuccessful.
4. Preserve raw text, JSON fields, extraction method and errors.

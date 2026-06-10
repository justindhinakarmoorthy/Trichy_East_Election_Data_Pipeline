# Source Inventory

Scope: Assembly Constituency 141, Tiruchirappalli East.

## 2016

| Dataset | Source file | Pages | Extraction |
|---|---|---:|---|
| Boothwise votes | `Ac141_2016.pdf` | 13 | Native PDF text |
| Candidate totals | `Candidatewise_votes.pdf` | 1 | Native PDF text |
| Candidate list | `Candidate_List_2016.pdf` | 2 | Native PDF text |
| Polling booths | `pollingstations_2016.pdf` | 76 | Native PDF text |

## 2021

| Dataset | Source file | Pages | Extraction |
|---|---|---:|---|
| Boothwise votes | `AC141_2021.pdf` | 21 | Native PDF text |
| Candidate totals | `Cumulative_Votes_2021.pdf` | 1 | OCR required |
| Candidate list | `Candidates_List_2021.pdf` | 3 | OCR required |
| Polling booths | `Polling_Stations_2021.pdf` | 58 | Native PDF text |

## 2026

| Dataset | Source file | Pages | Extraction |
|---|---|---:|---|
| Boothwise votes | `AC141.pdf` | 18 | Native PDF text |
| Candidate totals | `Cummulative Votes _Trichy East _2026 - Sheet1.csv` | N/A | CSV with preamble and summary rows |
| Candidate list | `Candidate_List_2026.csv` | N/A | Malformed single-field CSV |
| Polling booths | `Polling_Booth_List_2026.pdf` | 75 | Native PDF text |

## Important Reconciliation Rule

Form 20 contains general polling-station votes. Official candidate totals may also include postal votes.

```text
official candidate total
- boothwise candidate total
= postal votes

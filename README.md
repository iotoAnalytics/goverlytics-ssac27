# Goverlytics SSAC27

**Repository:** <https://github.com/iotoAnalytics/goverlytics-ssac27>

## Abstract

This repository contains the derived data and figures for the Goverlytics SSAC27 analysis of legislative attention in Canadian House of Commons debates.

## Source and Coverage

The underlying source is the **Debates (Hansard)** published by the [House of Commons of Canada](https://www.ourcommons.ca/documentviewer/en/house/latest/hansard). Hansard is the edited record of what is said in the House. This release contains derived results from the Canadian Hansard corpus, not the underlying transcript text.

Coverage runs from January 2022 through June 2026. `monthly_attention_concepts.csv` omits months with no classified records in the extraction; their absence is not a zero value. `monthly_tariff_topics.csv` instead contains every calendar month in that interval and fills a topic-series gap with zero. In a month omitted from the concept dataset, a zero in the tariff dataset can therefore mean either no tariff-topic record or no classified record at all; it must not be read as “no parliamentary activity.” The proprietary classification system and raw corpus are not distributed here.

## Included Data

All data files used to check the published figures are committed under [`data/`](data/):

- [`data/topic_share_2022_2026.csv`](data/topic_share_2022_2026.csv) - full-period topic ranking, record counts, shares, and cumulative shares.
- [`data/historic_vs_last4q.csv`](data/historic_vs_last4q.csv) - topic shares in the current four-quarter window, historical quarterly norm, and percentage-point shift.
- [`data/monthly_attention_concepts.csv`](data/monthly_attention_concepts.csv) - monthly shares for broad concepts, including the residual “All other concepts” category.
- [`data/monthly_tariff_topics.csv`](data/monthly_tariff_topics.csv) - monthly shares for the tariff-related topic series used in the line chart.
- [`data/member_tariff_records.csv`](data/member_tariff_records.csv) - member-level tariff record counts and derived shares.

## What Is a Record?

For this project, a **record** is one topic tag attached to one Hansard intervention. An intervention can receive more than one topic tag, so it can contribute more than one record. Record counts are therefore counts of topic assignments, not necessarily counts of speeches, interventions, or individual members.

## Definitions

### Legislative Attention

Legislative Attention is the share of tagged Hansard records assigned to a topic during a reporting period:

`topic records / all tagged records x 100`

This is a distribution of topic assignments and sums to 100% within each reporting period, apart from displayed rounding. It should not be interpreted as a count of distinct speeches or interventions.

In `historic_vs_last4q.csv`, `current_%` is the share in the most recent four complete quarters in the extract. `historical_norm_%` is the mean of the quarterly shares in all earlier quarters; `shift_pp` is the difference between them in percentage points.

### Topic Selection in the Notebook

Headline topic counts, shifts, tariff series, and member metrics count the populated `topic` field. The notebook's `best_match(payload)` helper is used only to select a payload entry for the broad-concept hierarchy in `monthly_attention_concepts.csv`; it does not determine those headline topic counts.

## Topic-Label Change After 2025 Q2

The source taxonomy changed after **2025 Q2**. Records through 2025 Q2 use the earlier topic-label set; records from 2025 Q3 onward use the revised label set. The CSVs retain the labels produced for each period and do not provide a one-to-one crosswalk between the two versions.

This matters for interpretation: a time-series change in a named topic across the 2025 Q2/2025 Q3 boundary can reflect the label revision as well as a change in parliamentary attention. Do not treat identically or similarly named labels on either side of the boundary as directly comparable without a validated crosswalk. Broad-concept results should likewise be read as the published aggregation for this release, rather than as a claim that every underlying topic label remained unchanged.

## Code and Reproducibility

The classification code is proprietary and is not included in this repository. The derived data needed to check every published figure is included, subject to the licensing and source-use notes below.

The exploratory analysis and chart-generation code is in `notebooks/`. The generated chart files belong in `figures/`; derived CSV outputs belong in `data/`. Install the notebook dependencies with `python -m pip install -r requirements.txt`.

The notebook requires the raw input described in [Raw Hansard Dataset](#raw-hansard-dataset) below. The committed derived CSVs are the release artifacts for checking the published tables and figures.

Users who only have this repository can run [`notebooks/artifact_results.ipynb`](notebooks/artifact_results.ipynb) to load and inspect the committed derived outputs without the raw extract.

## Raw Hansard Dataset

The anonymized raw Hansard extract is available as a separate [CSV download](https://webapp-resource.s3.us-west-2.amazonaws.com/ssac27/canada_hansard_2022_2026_anonymized.csv). It is over 300 MB and is not included in the repository because of its size and source-use conditions.

To run `notebooks/hansard_analysis.ipynb`:

1. Download the anonymized CSV.
2. Rename it to `canada_hansard_2022-2026.csv`.
3. Place it in the repository's `data/` directory.
4. Open the notebook and run the cells.

From the repository root, PowerShell users can download and save it under the expected filename with:

```powershell
Invoke-WebRequest `
  -Uri "https://webapp-resource.s3.us-west-2.amazonaws.com/ssac27/canada_hansard_2022_2026_anonymized.csv" `
  -OutFile "data\canada_hansard_2022-2026.csv"
```

The derived CSV files committed under [`data/`](data/) remain available for checking the published tables and figures without downloading the raw extract. Access to the raw extract remains subject to the House of Commons source terms and any applicable access conditions.

## Repository Structure

```text
goverlytics-ssac27/
├── README.md
├── LICENSE
├── requirements.txt
├── data/                  # Derived CSV files used to check the figures
├── figures/               # The four published chart outputs
└── notebooks/             # Exploratory analysis and validation work
```

The derived CSVs and published chart outputs are committed. The raw Hansard extract is intentionally excluded, and there is currently no separate `scripts/` directory.

## Member Data

The member-level dataset contains counts associated with elected officials speaking in public parliamentary debate. It contains no private contact information or bulk transcript text; these public-role counts are not personal data in the ordinary privacy sense. The committed file uses stable pseudonyms (`P###`) as a presentation choice, not because anonymization is required.

## Licensing and Source Credit

The repository's original materials are governed by the [LICENSE](LICENSE) rights notice. This repository publishes derived counts and figures, not bulk Hansard transcripts.

Credit the **House of Commons of Canada** as the source of the underlying Hansard material. Any reuse of Hansard material remains subject to the House of Commons' own [privacy, copyright, and important notices](https://www.ourcommons.ca/en/important-notices); nothing in this repository grants rights in that source material.

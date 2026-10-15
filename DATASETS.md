# Datasets

Every dataset used in this repository is public. This file records what each dataset is, why it was chosen, where it comes from, how to download it, and its licence or terms of use. Raw data files are **not** committed to this repository; each project provides a download step instead.

> Licence status values: **Confirmed** (licence text read and recorded) or **To confirm** (must be checked and recorded on the date of first download). Update the "Checked on" column when verified.

## Summary

| ID | Dataset | Used in | Publisher | Licence / terms | Status | Checked on |
|----|---------|---------|-----------|-----------------|--------|------------|
| D1 | NYC TLC Yellow Taxi Trip Records (Parquet) | M1, M2, M3 | NYC Taxi and Limousine Commission | City of New York Terms of Use (no Creative Commons licence stated on the TLC page) | To confirm | |
| D2 | NYC TLC Taxi Zone Lookup (CSV) | M1, M2, M3 | NYC Taxi and Limousine Commission | As D1 | To confirm | |
| D3 | Privacy Act 1988 (Cth), current compilation | M4 | Federal Register of Legislation | See legislation.gov.au Terms of use | To confirm | |
| D4 | OAIC Australian Privacy Principles guidelines | M4 | Office of the Australian Information Commissioner | Page states "© Commonwealth of Australia"; no Creative Commons licence seen on the page | To confirm | |
| D5 (later) | Australian Bureau of Statistics (ABS) tables | After M4 | ABS | ABS website states Creative Commons Attribution 4.0 International (some material excluded, such as microdata and third-party content) | To confirm per table | |

If a licence cannot be confirmed for D3 or D4 in a form that permits reuse, M4 will use another openly licensed document set. This file will be updated and the change recorded in an ADR.

---

## D1 and D2: NYC TLC trip records and taxi zones

### What it is
Trip-level records for New York City yellow taxis: pickup and drop-off timestamps and zones, passenger count, distance, fare components, tip, tolls, taxes, surcharges (including the congestion-related fees) and payment type. The zone lookup maps each location ID to a borough, zone name and service zone.

### Why this dataset was chosen
1. **Familiar to recruiters and engineers.** It is the best-known public dataset in the Spark and Databricks ecosystem, so a reader recognises the problem immediately and can judge the quality of the work.
2. **Right shape for the projects.**
   - It is published as monthly Parquet files, which suits incremental ingestion with Auto Loader (M1).
   - It has natural dimensional structure: date, time, pickup zone and drop-off zone (a role-playing dimension), payment type, rate code, vendor (M2).
   - It is large enough to show real engineering (partitioning or clustering, file sizes, data-quality rules) without needing a cluster.
3. **Interesting, defensible findings.** Recent files include congestion-related fee columns, which allows a before-and-after analysis of New York's congestion pricing (introduced in January 2025) on trip volumes, fares and tips by zone. Other questions include airport versus city trips, tipping by payment type, and time-of-day demand.
4. **Open and reproducible.** Anyone can download the same files and rerun the work.

### Known limitations (to state in the projects)
- Data quality is imperfect (negative fares, zero distances, timestamps outside the file's month). Handling these is part of the Silver layer.
- Zones do not change over time, so a Slowly Changing Dimension Type 2 cannot be demonstrated from real history. M2 will demonstrate SCD Type 2 using a clearly labelled, simulated second snapshot of a dimension.
- The TLC page states that the trip data was not created by the TLC and that no representation is made as to its accuracy.

### Sources
- TLC Trip Record Data (official page, lists all files by month): https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page
- Data dictionary and user guide: https://www.nyc.gov/assets/tlc/downloads/pdf/trip_record_user_guide.pdf
- AWS Registry of Open Data entry (lists the City of New York terms of use as the licence): https://registry.opendata.aws/nyc-tlc-trip-records-pds
- City of New York Terms of Use: http://www1.nyc.gov/home/terms-of-use.page

### Download URLs
Yellow taxi trips (one file per month, replace YYYY-MM):

```
https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_YYYY-MM.parquet
```

Taxi zone lookup:

```
https://d37ci6vzurychx.cloudfront.net/misc/taxi_zone_lookup.csv
```

The official page listed files from 2016 to August 2026 (and early 2015) when this file was written.

### Months used
- **Start (M1):** December 2024, January 2025, February 2025 (about 3 months, kept small for Databricks Free Edition quotas, and spanning the introduction of congestion pricing).
- **Extend (optional, M2/M3):** July 2024 to June 2025 (12 months) if quotas allow.

### How to download (Windows PowerShell)
Download on your own computer, then upload to Databricks. Databricks Free Edition restricts outbound internet access, so downloading inside the workspace may fail.

```powershell
$dest = "C:\data_projects\lakehouse-architect-portfolio\data_raw\nyc_tlc"
New-Item -ItemType Directory -Force -Path $dest | Out-Null
$months = "2024-12","2025-01","2025-02"
foreach ($m in $months) {
  $url = "https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_$m.parquet"
  Invoke-WebRequest -Uri $url -OutFile "$dest\yellow_tripdata_$m.parquet"
}
Invoke-WebRequest -Uri "https://d37ci6vzurychx.cloudfront.net/misc/taxi_zone_lookup.csv" -OutFile "$dest\taxi_zone_lookup.csv"
```

Add `data_raw/` to `.gitignore` so the raw files are never committed.

### How to load into Databricks
1. In the workspace, open **Catalog**, create a catalog and schema for the project, then create a **Volume** (for example `landing`).
2. Create two folders in the volume: `yellow_trips` and `reference`.
3. Use **Upload to this volume** to upload the monthly Parquet files into `yellow_trips` and the CSV into `reference`.
4. Ingest from the volume path (for example `/Volumes/<catalog>/<schema>/landing/yellow_trips/`) with Auto Loader.

To demonstrate incremental ingestion, upload one month first, run the pipeline, then upload the next month and run it again.

### Attribution and licence record
Add here on the day of first download:
- Date checked:
- Exact terms wording or link to the version read:
- Attribution statement used in the projects: "Contains data from the NYC Taxi and Limousine Commission (TLC) Trip Record Data, published by the City of New York."

---

## D3 and D4: Privacy Act 1988 and OAIC guidance (used in M4)

### What it is
The text of the Privacy Act 1988 (Cth), including the 13 Australian Privacy Principles, and the OAIC's published guidance on applying them.

### Why this dataset was chosen
1. A retrieval-augmented question-answering assistant needs authoritative text where answers must be traceable to sources. Legislation and regulator guidance are well suited, because every answer can cite a section or paragraph.
2. The domain is relevant to the Australian public sector, financial services, insurance and energy, where data governance and privacy are part of an architect's responsibilities.
3. It supports a meaningful evaluation set (for example, "when may an organisation use personal information for a secondary purpose?") with answers that can be checked against the source text.

### Sources
- Federal Register of Legislation (Privacy Act 1988): https://www.legislation.gov.au
- Terms of use for legislation content: https://www.legislation.gov.au/terms-of-use
- OAIC Australian Privacy Principles guidelines: https://www.oaic.gov.au/privacy/australian-privacy-principles/australian-privacy-principles-guidelines

### Download
Use the "download" option on the Federal Register page for the current compilation (PDF or Word), and the OAIC page content saved as text. Store them under `data_raw/privacy/` (not committed).

### Licence note
The OAIC page shows "© Commonwealth of Australia" and no Creative Commons licence on the page viewed. Before use, read the OAIC copyright and legislation.gov.au terms of use pages, record the wording here, and confirm reuse is permitted. M4 does not redistribute the source documents; it publishes only its own code, chunk metadata and evaluation questions with section references.

---

## D5 (later): Australian Bureau of Statistics

Planned for exploration after M1 to M4. The ABS website states that its content is available under a Creative Commons Attribution 4.0 International licence, with exclusions (for example the ABS logo, microdata and third-party content). Confirm the licence on the page of each table used, and attribute the ABS as required.

- ABS website copyright statement: https://www.abs.gov.au/website-privacy-copyright-and-disclaimer
- ABS data: https://www.abs.gov.au/statistics

---

## Rules for adding a dataset

1. Record publisher, URL, licence and the date checked before using it.
2. Do not use data with non-commercial, no-derivatives or unknown licences.
3. Do not commit raw data; commit download steps.
4. State known quality limits and any simulated data.
5. Never use data from an employer or client.
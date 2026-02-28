# Terra Pipeline for Large Data Science Health Equity Study

Code for my pipeline analyzing financial toxicity and healthcare access among disaggregated Asian American cancer survivors. Built to run end-to-end inside Juputer on Terra. Handles the full workflow from raw EHR and survey pulls down to the final regression tables and plots.

**Note:** Standard data privacy disclaimer—no PHI or individual-level data in this repo per DUAs.

## Tech / Pipeline Notes

* Polars over pandas with lazy eval and scanning parquets directly from GCS, for efficient transfromation and joins
* Basic `google-cloud-bigquery` wrappers to execute SQL queries and send the results straight to GCS buckets
* Uses `phetk.phecode` to map raw ICD billing codes to Phecodes. Calculates time between the first EHR-confirmed cancer dx and the survey date to define the cohort (and flags solid vs hematologic). Also aggregates baseline comorbidities (HTN, diabetes, MDD, etc.) from historical records.
*Hooked up `rpy2` to calculate the exact cohort drop-off numbers in python and pass them to R's `ggconsort` to render the CONSORT on the fly
* Auto-generates Table 1s (`tableone`), runs the multivariable logit models (adjusting for age, sex, income, multiracial identity, comorbidity burden, etc), and renders forest plots (`plotnine`) and html tables (`great_tables`).

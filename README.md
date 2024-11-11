# Guide: Converting OMOP Data to CDISC Format for External Controls

This guide provides a step-by-step process for converting OMOP data into the CDISC format for the purpose of analyzing OMOP data as external controls. This includes an overview of both CDISC and OMOP formats, defining mappings between them, and transforming the data using sample code in Python, R, STATA, and SPSS.

---

## Table of Contents
[Overview of CDISC and OMOP Formats](#1-overview-of-cdisc-and-omop-formats)  
   - [CDISC Format (Clinical Data Standards)](#cdisc-format-clinical-data-standards)  
   - [CDISC SDTM Domains Overview](#cdisc-sdtm-domains-overview)  
   - [OMOP Format (Observational Medical Outcomes Partnership)](#omop-format-observational-medical-outcomes-partnership)
     
[Resources](#resources)  

[Summary](#summary)  

[Extract Data from OMOP](extract-data-from-omop)  

[Transform Data to CDISC Format](transform-data-to-cdisc-format)  

[Export Transformed Data to CDISC-Compliant Format](export-transformed-data-to-cdisc-compliant-format)  

[Additional Code Samples in R, STATA, and SPSS](additional-code-samples-in-r-stata-and-spss)  

[Validate the CDISC Output](validate-the-cdisc-output)  

---

## 1. Overview of CDISC and OMOP Formats

### CDISC Format (Clinical Data Standards)

CDISC is designed to standardize clinical trial data, allowing for consistency and reproducibility. The main CDISC standards relevant to clinical trials include:

- **SDTM (Study Data Tabulation Model)**: Organizes clinical trial data into domains like DM (Demographics), AE (Adverse Events), LB (Laboratory), and more.
- **ADaM (Analysis Data Model)**: Structures datasets for statistical analysis, with a focus on traceability.
- **SEND (Standard for Exchange of Nonclinical Data)**: Used for nonclinical studies, primarily in regulatory submissions.

### CDISC SDTM Domains Overview

| Domain | Description                          | Commonly Used Variables               |
|--------|--------------------------------------|---------------------------------------|
| DM     | **Demographics**: Basic participant data | `SUBJID`, `AGE`, `SEX`, `RACE`         |
| AE     | **Adverse Events**: Reported adverse events | `AESEQ`, `AESTDTC`, `AEENDTC`, `AETERM` |
| LB     | **Laboratory**: Lab test results       | `LBSEQ`, `LBDTC`, `LBORRES`, `LBTEST`   |
| CM     | **Concomitant Medications**: Medications taken during the trial | `CMSEQ`, `CMSTDTC`, `CMENDTC`, `CMTRT` |
| EX     | **Exposure**: Study treatment details | `EXSEQ`, `EXSTDTC`, `EXENDTC`, `EXDOSE` |
| VS     | **Vital Signs**: Measurements of vitals | `VSSEQ`, `VSDTC`, `VSORRES`, `VSTEST`   |
| MH     | **Medical History**: Participant medical history | `MHSEQ`, `MHTERM`, `MHDTC`              |
| PR     | **Procedures**: Performed medical procedures | `PRSEQ`, `PRSTDTC`, `PRENDTC`, `PRTRT`  |
| EG     | **ECG Test Results**: Electrocardiogram results | `EGSEQ`, `EGDTC`, `EGORRES`, `EGTEST`   |
| DS     | **Disposition**: Status and reason for discontinuation | `DSDECOD`, `DSTERM`, `DSSTDTC`          |

### OMOP Format (Observational Medical Outcomes Partnership)

OMOP is a common data model primarily used for observational research, organizing it into standardized tables optimized for large-scale analytics. Below is an overview of key OMOP tables relevant for mapping to CDISC:

| Table               | Description                         | Commonly Used Variables                        |
|---------------------|-------------------------------------|------------------------------------------------|
| Person              | **Demographics**: Patient data     | `person_id`, `gender_concept_id`, `year_of_birth`, `race_concept_id` |
| Condition Occurrence| **Conditions**: Recorded conditions| `condition_occurrence_id`, `person_id`, `condition_concept_id`, `condition_start_date`, `condition_end_date` |
| Drug Exposure       | **Drug Exposure**: Medication data | `drug_exposure_id`, `person_id`, `drug_concept_id`, `drug_exposure_start_date`, `drug_exposure_end_date` |
| Measurement         | **Measurements**: Lab tests, vitals| `measurement_id`, `person_id`, `measurement_concept_id`, `measurement_date`, `value_as_number` |
| Observation         | **Observations**: General observations | `observation_id`, `person_id`, `observation_concept_id`, `observation_date`, `value_as_number` |
| Procedure Occurrence| **Procedures**: Recorded procedures| `procedure_occurrence_id`, `person_id`, `procedure_concept_id`, `procedure_date` |
| Visit Occurrence    | **Visits**: Patient visits         | `visit_occurrence_id`, `person_id`, `visit_concept_id`, `visit_start_date`, `visit_end_date` |

---

## Resources

- **[PharmaSUG 2022: RWD (OMOP) to SDTM (CDISC): A Primer for Your ETL Journey](https://www.lexjansen.com/pharmasug/2022/RW/PharmaSUG-2022-RW-160.pdf)** - A comprehensive guide on transforming OMOP data into CDISC SDTM format.
- **[BRIDG Model](https://bridgmodel.nci.nih.gov/)** - A reference model linking healthcare and clinical research data.
- **[Leveraging the OMOP CDMv5 for CDISC SDTM RCT Data](https://www.ohdsi.org/web/wiki/lib/exe/fetch.php?media=projects:workgroups:shyft-ohdsi-poster.pdf)** - Poster on using OMOP CDM to support SDTM-based RCTs.
- **[From OMOP to CDISC SDTM: Successes, Challenges, and Future Opportunities](https://www.ohdsi.org/wp-content/uploads/2023/10/3-AndersonBriefreport-Wes-Anderson.pdf)** - Insights on using OMOP for CDISC SDTM in COVID-19 research.
- **[OMOP to CDISC SDTM Mapping Project on GitHub](https://github.com/vojtechhuser/project/tree/master/sdtm-omop)** - GitHub tools and resources for OMOP to CDISC mapping.

---

## Summary

1.**Define Data Mappings** by identifying mappings between OMOP tanles and CDISC SDTM domains.
2. **Extract** data from OMOP tables.
3. **Map** OMOP fields to CDISC-compliant fields.
4. **Transform** data to align with CDISC formatting.
5. **Export** transformed data.
6. **Validate** using CDISC validation tools.


## Extract Data from OMOP

Load OMOP data using SQL, Python, or R. Here’s an example using Python.

```python
import pandas as pd
import sqlite3  # or any library to access your OMOP database

# Connect to OMOP database
conn = sqlite3.connect("omop_db.sqlite")

# Extract data from OMOP tables
person_df = pd.read_sql_query("SELECT * FROM person", conn)
condition_occurrence_df = pd.read_sql_query("SELECT * FROM condition_occurrence", conn)
measurement_df = pd.read_sql_query("SELECT * FROM measurement", conn)
drug_exposure_df = pd.read_sql_query("SELECT * FROM drug_exposure", conn)

# Close the connection
conn.close()

# Display the data for verification
print("Person Data:")
print(person_df.head())
print("\nCondition Occurrence Data:")
print(condition_occurrence_df.head())
print("\nMeasurement Data:")
print(measurement_df.head())
print("\nDrug Exposure Data:")
print(drug_exposure_df.head())
```

---

## Transform Data to CDISC Format

Use identified mappings to transform OMOP variables into CDISC-compliant formats. See Python example code below.

```python
# Map Demographics (DM) data
dm = person_df.rename(columns={
    'person_id': 'SUBJID',
    'gender_concept_id': 'SEX',
    'year_of_birth': 'BRTHDTC'
})
dm['STUDYID'] = 'YourStudyID'

# Map Adverse Events (AE) data
ae = condition_occurrence_df.rename(columns={
    'condition_occurrence_id': 'AESEQ',
    'condition_start_date': 'AESTDTC',
    'condition_end_date': 'AEENDTC',
    'condition_concept_id': 'AETERM'
})
ae['STUDYID'] = 'YourStudyID'

# Map Laboratory (LB) data
lb = measurement_df.rename(columns={
    'measurement_id': 'LBSEQ',
    'measurement_date': 'LBDTC',
    'value_as_number': 'LBORRES'
})
lb['STUDYID'] = 'YourStudyID'
```

---

## Export Transformed Data to CDISC-Compliant Format

Export transformed DataFrames in formats compatible with CDISC (e.g., `.xpt` or `.csv` for SDTM datasets).

```python
dm.to_csv("DM.csv", index=False)
ae.to_csv

("AE.csv", index=False)
lb.to_csv("LB.csv", index=False)
```

---

## Additional Code Samples in R, STATA, and SPSS

### R Code Sample

```r
# Load necessary libraries
library(dplyr)
library(DBI)

# Connect to OMOP database
con <- dbConnect(RSQLite::SQLite(), "omop_db.sqlite")

# Extract tables
person_df <- dbGetQuery(con, "SELECT * FROM person")
condition_occurrence_df <- dbGetQuery(con, "SELECT * FROM condition_occurrence")

# Transform Demographics (DM) Data
dm <- person_df %>%
  rename(SUBJID = person_id, SEX = gender_concept_id, BRTHDTC = year_of_birth) %>%
  mutate(STUDYID = "YourStudyID")

# Export to CSV
write.csv(dm, "DM.csv", row.names = FALSE)

# Disconnect from database
dbDisconnect(con)
```

### STATA Code Sample

```stata
// Load OMOP data
use person, clear

// Transform Demographics for CDISC DM
gen SUBJID = person_id
gen SEX = gender_concept_id
gen BRTHDTC = year_of_birth
gen STUDYID = "YourStudyID"

// Save transformed DM data
save DM, replace
```

### SPSS Code Sample

```spss
GET DATA /TYPE=ODBC /CONNECT="DSN=omop_db" /SQL="SELECT * FROM person".
RENAME VARIABLES (person_id = SUBJID)(gender_concept_id = SEX)(year_of_birth = BRTHDTC).
COMPUTE STUDYID = "YourStudyID".
SAVE OUTFILE='DM.sav'.
```

---

## Validate the CDISC Output

Validation using Pinnacle 21 or similar tools is essential to ensure SDTM compliance.

### Pinnacle 21 Overview

[Pinnacle 21](https://www.pinnacle21.com/) is a software product for validating compliance with CDISC standards. It is widely used by regulatory agencies, Pinnacle 21 offers two versions:

- **Community Version**: Free, open-source tool for validation and Define.xml generation.
- **Enterprise Version**: Advanced features for continuous compliance checks and submission readiness.

Pinnacle 21 plays a role in:
- **Standardizing Data**
-   Maing sure data sets comply with CDISC (SDTM, ADaM) and facilitating easier transformations from formats like OMOP to CDISC.
- **Regulatory Compliance**
-   Helping organizations achieve "submission-ready" datasets, which are critical for regulatory approval.
- **Supporting CDISC Transformation Projects**
-   Serving as a primary tool for data validation, Pinnacle 21 is invaluable for projects like the OMOP to CDISC transformation detailed in this guide.

**Additional Resources**:
- [Download Pinnacle 21 Community](https://www.pinnacle21.com/downloads)
- [About Pinnacle 21](https://www.pinnacle21.com/about)


# Guide: Converting OMOP Data to CDISC Format for External Controls

This guide provides a step-by-step process for converting OMOP data into the CDISC format for the purpose of analyzing OMOP data as external controls. This includes an overview of both CDISC and OMOP formats, defining mappings between them, and transforming the data using sample code in Python, R, STATA, and SPSS.

---

## Table of Contents
[Overview of CDISC and OMOP Formats](#overview-of-cdisc-and-omop-formats)  
   - [CDISC Format (Clinical Data Standards)](#cdisc-format-clinical-data-standards)  
   - [CDISC SDTM Domains Overview](#cdisc-sdtm-domains-overview)  
   - [OMOP Format (Observational Medical Outcomes Partnership)](#omop-format-observational-medical-outcomes-partnership)
     
[Resources](#resources)  

[Summary](#summary)  

[Extract Data from OMOP](#extract-data-from-omop)  

[Transform Data to CDISC Format](#transform-data-to-cdisc-format)  

[Export Transformed Data to CDISC-Compliant Format](#export-transformed-data-to-cdisc-compliant-format)  

[Additional Code Samples in R, STATA, and SPSS](#additional-code-samples-in-r-stata-and-spss)  

[Validate the CDISC Output](#validate-the-cdisc-output)  

---

## <a name="overview-of-cdisc-and-omop-formats"></a>1. Overview of CDISC and OMOP Formats

### <a name="cdisc-format-clinical-data-standards"></a>CDISC Format (Clinical Data Standards)

CDISC is designed to standardize clinical trial data, allowing for consistency and reproducibility. The main CDISC standards relevant to clinical trials include:

- **SDTM (Study Data Tabulation Model)**: Organizes clinical trial data into domains like DM (Demographics), AE (Adverse Events), LB (Laboratory), and more.
- **ADaM (Analysis Data Model)**: Structures datasets for statistical analysis, with a focus on traceability.
- **SEND (Standard for Exchange of Nonclinical Data)**: Used for nonclinical studies, primarily in regulatory submissions.

### <a name="cdisc-sdtm-domains-overview"></a>CDISC SDTM Domains Overview

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

### <a name="omop-format-observational-medical-outcomes-partnership"></a>OMOP Format (Observational Medical Outcomes Partnership)

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

## <a name="resources"></a>Resources

## <a name="summary"></a>Summary

## <a name="extract-data-from-omop"></a>Extract Data from OMOP

## <a name="transform-data-to-cdisc-format"></a>Transform Data to CDISC Format

## <a name="export-transformed-data-to-cdisc-compliant-format"></a>Export Transformed Data to CDISC-Compliant Format

## <a name="additional-code-samples-in-r-stata-and-spss"></a>Additional Code Samples in R, STATA, and SPSS

## <a name="validate-the-cdisc-output"></a>Validate the CDISC Output

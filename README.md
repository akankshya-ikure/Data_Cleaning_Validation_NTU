# Data_Cleaning_Validation_NTU
Data Cleaning and Validation for NTU
# Dataset Cleaning and Analysis

## Overview

Scripts and documentation for cleaning and analyzing patient data from multiple Excel sheets.

## Features
- Standardize patient IDs
- Identify participation inconsistencies
- Detect and resolve duplicates
- Calculate patient completion rates
- Analyze CHW involvement
- Map geographic coverage

## Prerequisites

- Python 3.8+
- Pandas
- OpenPyXL

Install the dependencies:

```bash
pip install pandas openpyxl
Here is a structured template for your GitHub repository documentation with the provided code integrated, ready for use in your project:

---

# Dataset Cleaning and Analysis

## Overview

This repository contains scripts and documentation for cleaning and analyzing patient data across multiple Excel sheets. The objective is to ensure consistency and accuracy in handling similar datasets, with specific steps for data processing, standardization, and analysis.

---



## Table of Contents

1. [Getting Started](#getting-started)
2. [Data Description](#data-description)
3. [Cleaning and Analysis Steps](#cleaning-and-analysis-steps)
4. [Key Metrics and Results](#key-metrics-and-results)
5. [Usage](#usage)
6. [Contributing](#contributing)
7. [License](#license)

---


### File Structure

- `ntu_cleaned_data.xlsx`: Source data with the following sheets:
  - `CHW_Details`
  - `Hypertension_Visit1`
  - `Hypertension_Visit2`
  - `Hypertension_Visit3`
  - `Hypertension_Visit4`
- `analysis_scripts/`: Directory containing Python scripts for data cleaning and analysis.
- `results/`: Directory for output logs and results.

---

## Data Description

The dataset consists of patient information and their participation across four visits. Each sheet includes:

- Patient IDs (`patientUHID`)
- Visit details
- Community Health Worker IDs (`chwId`)
- Geographic information (`district`, `state`, `country`)

---

## Cleaning and Analysis Steps

### 1. Loading Data

Load data from Excel sheets into separate Pandas DataFrames:

```python
import pandas as pd

# Load data
df1 = pd.read_excel('ntu_cleaned_data.xlsx', sheet_name='CHW_Details')
df2 = pd.read_excel('ntu_cleaned_data.xlsx', sheet_name='Hypertension_Visit1')
df3 = pd.read_excel('ntu_cleaned_data.xlsx', sheet_name='Hypertension_Visit2')
df4 = pd.read_excel('ntu_cleaned_data.xlsx', sheet_name='Hypertension_Visit3')
df5 = pd.read_excel('ntu_cleaned_data.xlsx', sheet_name='Hypertension_Visit4')
```

---

### 2. Standardizing Patient IDs

Ensure `patientUHID` is formatted consistently across all sheets:

```python
for df in [df2, df3, df4, df5]:
    df['patientUHID'] = "'" + df['patientUHID'].astype(str)
```

---

### 3. Identifying Patient Visit Participation

Count unique patient IDs for each visit and detect participation inconsistencies:

```python
visit1_patients = set(df2['patientUHID'])
visit2_patients = set(df3['patientUHID'])
visit3_patients = set(df4['patientUHID'])
visit4_patients = set(df5['patientUHID'])

patients_not_in_1 = visit4_patients.union(visit3_patients, visit2_patients) - visit1_patients

print("Patients in visits 4, 3, 2 but not in visit 1:", patients_not_in_1)
```

---

### 4. Detecting Duplicates

Identify duplicate records for each visit:

```python
print("Duplicates in Visit 1:", df2.duplicated().sum())
print("Duplicates in Visit 2:", df3.duplicated().sum())
print("Duplicates in Visit 3:", df4.duplicated().sum())
print("Duplicates in Visit 4:", df5.duplicated().sum())
```

---

### 5. Calculating Completion Rates

Determine how many patients completed all four visits:

```python
all_patients = visit1_patients.intersection(visit2_patients, visit3_patients, visit4_patients)
completion_rate = (len(all_patients) / len(df2)) * 100
print("Patient Completion Rate:", completion_rate)
```

---

### 6. Analyzing CHW Involvement

Unify CHW IDs across visits:

```python
chw_ids_visit1 = set(df2['chwId'].dropna().astype(str).str.strip().str.lower())
chw_ids_visit2 = set(df3['chwId'].dropna().astype(str).str.strip().str.lower())
chw_ids_visit3 = set(df4['chwId'].dropna().astype(str).str.strip().str.lower())
chw_ids_visit4 = set(df5['chwId'].dropna().astype(str).str.strip().str.lower())

all_chw_ids = chw_ids_visit1.union(chw_ids_visit2, chw_ids_visit3, chw_ids_visit4)

print("Total number of CHWs involved:", len(all_chw_ids))
```

---

### 7. Geographic Coverage

Extract unique geographic locations:

```python
def extract_locations(df):
    return set(df[['district', 'state', 'country']].dropna().apply(tuple, axis=1))

locations_visit1 = extract_locations(df2)
locations_visit2 = extract_locations(df3)
locations_visit3 = extract_locations(df4)
locations_visit4 = extract_locations(df5)

all_geographic_locations = locations_visit1.union(locations_visit2, locations_visit3, locations_visit4)

print("Total geographic coverage (unique locations):", len(all_geographic_locations))
```

---

## Key Metrics and Results

- **Patient Completion Rate**: `<calculated rate>`
- **Total CHWs Involved**: `<count>`
- **Unique Geographic Locations**: `<count>`

---

## Usage

Run the provided script:

```bash
python analysis_scripts/cleaning_and_analysis.py
```

---

## Contributing

Contributions are welcome! Please fork this repository, make changes, and submit a pull request.

---

## License

This project is licensed under the MIT License.

---



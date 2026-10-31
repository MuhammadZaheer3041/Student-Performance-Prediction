# Student Performance Prediction - Data Preprocessing & Validation
This repository tracks the structured data engineering pipeline for the Student Performance Prediction workspace. The core focus of this project is transforming raw, inconsistent tracking parameters into verified database schemas ready for predictive machine learning models.

## Operational Objective
Raw educational databases often contain collection overlaps, missing logs, and boundary errors. This pipeline handles tracking integrity, systematically structures overlapping dimensions, and logs record collisions across 1,000 baseline rows.

## Tech Stack & Workspace
1.**Language:** Python 3
2.**Libraries:** Pandas, NumPy
3.**Environment:** Kaggle Notebook Runtime (`/kaggle/working`)

## Preprocessing & Data Cleaning Checklist
### 1. Structural Feature Dropping
Programmatic verification exposed overlapping variables (`attendance_pct` and `study_hours`) that compromised data normalization. 
a. Applied safe dropping structures (`errors='ignore'`) to clear secondary variants.
b. Successfully dropped the highly corrupted structural column `attendance_part` which contained 960 invalid missing entries.

### 2. ID Collision Control
a.**The Diagnostic:** The code caught 86 records sharing matching `student_id` entries. Granular profiling validated that these records mapped to entirely separate students with independent grades, rather than duplicate logs.
b.**The Fix:** Preserved raw row validation by adding an independent tracking column `record_id` at index 0 and injecting an explicit `id_collision` boolean flag to keep row data intact.

### 3. Null Imputation Matrix
A dynamic missing-value routing strategy was created based on specific data properties:
a.**Target Feature Integrity:** Dropped entries containing null elements in the prediction target column (`final_grade`) to maintain accuracy.
b.**Identity Variables:** Preserved historical tracking parameters, keeping the remaining 38 empty fields in `student_id` properly contained without synthetic value insertions.
c.**Categorical / Continuous Columns:** Handled missing tracking attributes using column modes for object features and rounded column medians for numeric blocks.

## Cleaned Output Asset
The workspace exports a standardized production dataset:
a.**Output Path:** `/kaggle/working/student_performance_cleaned.csv`
b.**Row Status:** Zero missing fields across core active training features.

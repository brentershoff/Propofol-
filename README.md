# Propofol Age-Exposure Reconstruction

Code for retrospective reconstruction of propofol effect-site exposure during anesthetic induction across the adult lifespan.

## Overview

This repository contains the analytic workflow used to assemble a retrospective propofol induction dataset, reconstruct modeled effect-site concentration trajectories, perform age-stratified statistical analyses, and generate manuscript figures and tables.

The workflow was developed for a study evaluating the relationship between clinician-administered propofol dose, modeled peak brain exposure, and age-adjusted pharmacodynamic requirement.

## Environment

Core Python dependencies are listed in `Requirements.txt`.

## Repository structure

This repository is organized to mirror the major stages of the analysis:

1. dataset extraction and assembly from the perioperative SQL warehouse
2. pharmacokinetic simulation of propofol effect-site concentration trajectories
3. statistical modeling of age-associated dose and exposure patterns
4. generation of main and supplementary manuscript figures and tables

## Notebook descriptions

### 01_extract_propofol_induction_dataset.ipynb

Creates the analysis-ready propofol induction dataset from the perioperative SQL warehouse. This notebook:

- identifies eligible adult general anesthesia cases receiving propofol
- identifies first documented induction and intubation timestamps
- extracts propofol medication administrations occurring within 10 minutes after induction
- retains both bolus and infusion events
- combines age-batched extraction files into a master dataset
- cleans timestamps, units, and dose fields
- computes patient-level dose summary variables, including total bolus dose, total infusion dose, total propofol dose, and dose normalized to total body weight
- applies the primary total-bolus filtering rule
- merges patient-level summary variables back into the long medication-event dataset
- exports the analysis-ready CSV used in downstream modeling

### 02_generate_table1_age_stratified.ipynb

Generates the manuscript Table 1 age-stratified baseline summary table. This notebook:

- loads the cleaned analysis-ready propofol induction dataset
- collapses the long-format medication-event data to a patient-level analytic table
- retains demographic and induction variables used in Table 1
- assigns patients to prespecified age strata
- computes descriptive summary statistics within each stratum
- formats manuscript-ready summary values
- exports LaTeX output for direct inclusion in Overleaf

### 03_simulate_propofol_ce_models.ipynb

Reconstructs modeled propofol effect-site concentration trajectories from the cleaned induction medication dataset. This notebook:

- loads the cleaned analysis-ready propofol induction dataset
- preprocesses timestamps and case-level variables for simulation
- defines helper functions for dosing weight and model inputs
- runs the primary Eleveld-based PK/PD simulation workflow
- computes peak modeled effect-site concentration for each case
- merges model outputs back into the patient-level analytic dataset
- repeats the workflow using the Schnider model as a sensitivity analysis
- exports model-specific analytic datasets for downstream regression, figure generation, and manuscript analysis

### 04_analyze_eleveld_outputs.ipynb

Performs the primary regression and figure-generation workflow using the patient-level Eleveld simulation output dataset. This notebook:

- loads the Eleveld-based patient-level peak exposure dataset
- applies the analytic exclusions and cohort restrictions used in the primary manuscript analysis
- defines the age-adjusted Eleveld pharmacodynamic reference function
- fits spline-based regression models for weight-normalized dose and modeled peak effect-site concentration
- generates standardized adjusted predictions across age
- produces the main multidimensional age-exposure figure
- fits logistic models for overexposure probability analyses
- generates manuscript-ready table outputs and illustrative model-based predictions


### 05_sensitivity_analyses.ipynb

Runs the supplementary sensitivity analyses used to evaluate robustness of the primary findings. This notebook:

- loads the appropriate patient-level simulation dataset for each sensitivity analysis section
- reproduces the analysis using the Schnider pharmacokinetic model
- extends the cohort to include the administratively masked 90+ age group
- reruns the primary models without adjustment for BMI, sex, or ASA physical status
- reruns the workflow using a broader-input cohort with relaxed preprocessing assumptions
- reruns the models with adjustment for co-administered induction medications, including fentanyl, ketamine, midazolam, and etomidate
- exports supplementary sensitivity-analysis figures and age-stratified tables
- combines individual sensitivity-analysis tables into a master supplementary table


## Reproducibility note

This public repository contains cleaned analytic code corresponding to the manuscript workflow. Institution-specific database connection details have been removed. Table and schema names have been generalized in the public version. Protected source data are not distributed with this repository.

## Important implementation notes

- The primary extraction workflow applies prespecified data-quality filters, including exclusion of cases with implausible total bolus propofol doses.
- Sensitivity analyses repeat the workflow under broader inclusion criteria to evaluate robustness of the main findings.
- Adjusted body weight (ABW)-based variables used in the final manuscript analyses are derived in downstream preprocessing and modeling notebooks rather than in the initial extraction notebook.

## Code availability and citation

The code in this public GitHub repository is available for review and reuse. A version corresponding to the final accepted manuscript will be archived and linked here.



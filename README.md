# Propofol Induction Lifespan Reconstruction

**Code and documentation for pharmacokinetic reconstruction of 250,640 adult propofol inductions.**

This repository contains the analytic pipeline used for cohort extraction, pharmacokinetic (PK) simulation, statistical modelling, and validation for the manuscript: **"Large-scale pharmacokinetic reconstruction of propofol effect-site concentrations during anaesthetic induction."**

## Project Summary

This project reconstructs propofol effect-site concentration trajectories during anaesthetic induction using high-resolution medication timestamps from a large retrospective perioperative dataset (UCLA, 2013–2026).

The primary objective is to evaluate whether age-related reductions in clinician-administered doses are sufficient, in model-based terms, to track the age-adjusted decline in propofol hypnotic sensitivity. The analysis focuses on the divergence between:

1. **Observed clinician behaviour:** weight-normalised induction dose ($\mathrm{mg~kg^{-1}}$)
2. **Modelled exposure:** peak effect-site concentration ($C_{e,\max}$)
3. **The age-adjusted $Ce_{50}$ benchmark:** derived from the propofol-only Eleveld model
4. **The model-predicted required dose ($D_{\mathrm{req}}$):** the proportional dose reduction required to align modelled exposure with the benchmark while preserving the observed pattern of drug delivery

## Repository Workflow

The workflow is organized into sequential stages for transparency and reproducibility.

### 1. Cohort Extraction (`01_extract_propofol_induction_dataset.ipynb`)
SQL-based retrieval of eligible adult general anaesthesia cases, identification of the 10-minute induction window, and assembly of bolus and infusion records.

### 2. Baseline Characterization (`02_generate_table1_age_stratified.ipynb`)
Generation of age-stratified cohort summaries and the baseline descriptive statistics presented in **Table S1**.

### 3. Pharmacokinetic Reconstruction (`03_simulate_propofol_ce_models.ipynb`)
The core simulation engine. Reconstructs $C_e$ trajectories at 1-second resolution using observed event-level medication records.

- **Primary framework:** Eleveld (2018) model
- **Sensitivity framework:** Schnider (1998) model
- **Alignment:** all cases anchored to the first propofol administration ($t = 0$)

### 4. Primary Regression and Main Figure (`04_analyze_eleveld_outputs.ipynb`)
Implements B-spline regression (5 degrees of freedom) to model continuous age-associated trajectories for dose, exposure, and benchmarks. This notebook generates **Figure 1 (Panels A–C)**, including the $D_{\mathrm{req}}$ trajectory.

### 5. Sensitivity Analyses (`05_sensitivity_analyses.ipynb`)
Evaluates robustness under alternative specifications, including:

- Schnider PK model comparison
- inclusion of the administratively masked age $\geq 90$ yr cohort
- adjustment for co-administered induction adjuncts (for example fentanyl, midazolam, ketamine, and etomidate)

### 6. External Validation (`06_external_validation_against_tivatrainer.ipynb`)
Assesses simulation fidelity by comparing Python-generated trajectories against TivaTrainer benchmark outputs for representative bolus and infusion scenarios (**Figure S2**).

## Main Manuscript Output

**Figure 1. Divergence between observed induction dosing and age-adjusted model-derived reference trajectories.**

- **Panel A:** modelled $C_{e,\max}$ versus the age-adjusted $Ce_{50}$ benchmark
- **Panel B:** observed clinician dose versus the $D_{\mathrm{req}}$ required to track the benchmark
- **Panel C:** relative age-associated decline indexed to a young-adult baseline

## Data Transparency and Reproducibility

Due to institutional privacy restrictions (UCLA IRB #26-0331), raw patient-level source data cannot be shared publicly. The repository therefore provides the full analytic workflow, manuscript-aligned notebooks, and validation examples to support transparency and code review.

## Disclaimer

All effect-site concentration values in this repository are **model-derived PK/PD constructs**. They do not represent measured blood concentrations or directly observed clinical drug effect.

# Propofol-

Code and documentation for **large-scale pharmacokinetic reconstruction of propofol effect-site concentrations during anaesthetic induction**.

This repository contains the code used for cohort extraction, pharmacokinetic simulation, statistical modeling, and generation of the main manuscript and supplementary appendix outputs corresponding to the current BJA correspondence and supplement.

## Project summary

This project reconstructs propofol effect-site concentration trajectories during anaesthetic induction using high-resolution medication timestamps from a large retrospective perioperative dataset. The primary pharmacokinetic framework is the **Eleveld model**, with **Schnider-model sensitivity analyses**. The main goal is to examine whether age-related reductions in clinician-administered induction dose are sufficient, in model-based terms, to track the age-adjusted decline in propofol hypnotic requirement.

The current correspondence focuses on the relationship between:

* observed clinician-administered induction dose
* modelled peak effect-site concentration (**Ce,max**)
* the age-adjusted **Ce50** benchmark derived from the propofol-only Eleveld model
* the model-predicted proportional dose reduction required to align modelled peak exposure with the age-adjusted benchmark

## Associated manuscript and supplement

This repository corresponds to the current BJA correspondence, **“Large-scale pharmacokinetic reconstruction of propofol effect-site concentrations during anaesthetic induction,”** and its supplementary appendix. The correspondence describes the primary cohort, modeling framework, and age-associated divergence between observed clinician dosing and age-adjusted model-derived benchmarks. The supplement provides expanded methodology, cohort derivation, statistical details, sensitivity analyses, validation against TivaTrainer, and repository organization.

## Data source and availability

Analyses were performed on a de-identified dataset derived from the UCLA perioperative data warehouse. Because of institutional privacy restrictions, the source perioperative electronic health record data cannot be shared publicly.

This repository provides the code used for data extraction, pharmacokinetic reconstruction, statistical analyses, and generation of manuscript-ready figures and tables.

## Repository workflow

The repository is organized around the core analytic steps of the study:

### 1. Cohort extraction and medication retrieval

Notebook for extraction of eligible adult general anaesthesia cases, identification of the induction window, and retrieval of propofol administrations recorded as boluses and infusion events.

### 2. Baseline cohort characterization

Notebook for generation of age-stratified cohort summaries and manuscript-ready baseline descriptive tables.

### 3. Pharmacokinetic reconstruction

Notebook for retrospective effect-site concentration simulation using observed event-level propofol administration records.

Key features include:

* event alignment relative to the first propofol dose
* simulation at 1-second resolution over a 900-second horizon
* handling of discrete bolus doses and continuous infusion events
* primary Eleveld-model simulation
* Schnider-model sensitivity reconstruction
* export of patient-level peak effect-site concentrations

### 4. Primary regression and figure generation

Notebook for spline-based regression modeling of age-associated changes in:

* observed induction dose normalized to adjusted body weight
* modelled peak effect-site concentration (**Ce,max**)
* age-adjusted **Ce50** benchmark
* model-predicted required dose to align modelled peak exposure with the benchmark

This workflow also generates the main manuscript figure and supporting summary tables.

### 5. Sensitivity analyses

Notebook for alternative modeling and preprocessing analyses, including:

* Schnider pharmacokinetic model
* unadjusted analyses
* broader-input cohort definitions
* inclusion of the administratively masked age ≥90 cohort
* adjustment for co-administered induction medications

## Main manuscript output

### Figure 1. Divergence between observed propofol induction dosing and age-adjusted model-derived benchmarks across the adult lifespan

This figure is the primary manuscript figure and contains three panels:

* **Panel A:** modelled peak effect-site concentration (**Ce,max**) versus the age-adjusted **Ce50** benchmark
* **Panel B:** observed clinician-administered induction dose versus the model-predicted dose required to align modelled peak exposure with the benchmark
* **Panel C:** relative age-associated decline in observed dose, modelled exposure, benchmark requirement, and model-predicted required dose

Together, these analyses show that although clinician-administered weight-normalized induction dose declines with age, modelled peak effect-site exposure declines much less than the age-adjusted benchmark.

## Supplementary outputs discussed in the current supplement

The README is intentionally aligned with the **current supplementary appendix** and focuses on the tables and figures described there.

### Figure S1. Study flow diagram

Shows derivation of the final analytic cohort from the original extraction, including sequential exclusions for:

* implausible induction dose
* missing dose documentation
* missing simulation variables
* physiologic outliers
* administratively masked age ≥90 years in the primary analysis

### Table S1. Baseline characteristics and induction practice by age stratum

Provides age-stratified cohort characteristics across the prespecified age groups, including:

* age
* weight
* body mass index
* female sex proportion
* ASA physical status distribution
* proportion receiving an infusion during induction
* proportion receiving more than one bolus during induction

### Table S2. Primary age-stratified model outputs

Provides the age-stratified adjusted estimates underlying the primary analysis, including:

* observed induction dose (mg/kg adjusted body weight)
* modelled peak effect-site concentration (**Ce,max**)
* age-adjusted **Ce50** benchmark
* percentage surplus by which **Ce,max** exceeds the benchmark

### Table S3. Sensitivity analyses

Summarizes robustness analyses across alternative specifications, including:

* Schnider PK model
* arithmetic/raw analysis
* broader-input cohort
* age-90+ extended analysis
* additional adjustment for co-administered induction medications

These analyses show that the central age-associated divergence pattern remains consistent across alternate modeling assumptions.

## External validation against TivaTrainer

### Figure S2 and validation table

The supplement also includes an external face-validation section comparing the automated reconstruction workflow against **TivaTrainer**.

Two representative induction scenarios were recreated independently in TivaTrainer:

* a **single-bolus** scenario
* a **bolus-plus-infusion** scenario

Figure S2 displays representative effect-site concentration trajectories for these scenarios under:

* the **Eleveld model** (solid lines)
* the **Schnider model** (dashed lines)

The accompanying validation table reports the corresponding scenario characteristics and peak effect-site concentrations generated by:

* the study workflow
* TivaTrainer under Eleveld
* TivaTrainer under Schnider

Across these benchmark scenarios, the reconstructed peak values closely matched the TivaTrainer outputs, supporting the fidelity of:

* event parsing
* time alignment
* unit conversion
* model implementation

This validation section is important because it demonstrates that the retrospective simulation workflow reproduces expected outputs for representative dosing patterns before being applied at scale to the full observational cohort.

## Current notebook-to-output correspondence

The supplement describes the repository as containing notebooks for the following purposes:

* **01_extract_propofol_induction_dataset.ipynb**
  Cohort extraction, induction-window medication retrieval, and assembly of the analysis-ready long-format propofol dataset.

* **02_generate_table1_age_stratified.ipynb**
  Generation of age-stratified baseline descriptive tables.

* **03_simulate_propofol_ce_models.ipynb**
  Primary Eleveld-based pharmacokinetic reconstruction, Schnider sensitivity-model simulation, and export of patient-level peak effect-site concentration outputs.

* **04_analyze_eleveld_outputs.ipynb**
  Primary regression workflow using Eleveld-derived exposure outputs, generation of the main manuscript figure, and export of manuscript-ready summary tables.

* **05_sensitivity_analyses.ipynb**
  Robustness analyses under alternative modeling assumptions and preprocessing definitions.

* **requirements.txt**
  Python dependencies required to reproduce the computational environment.

* **README.md**
  Repository overview, workflow description, notebook map, and reproducibility notes.

## Reproducibility notes

To reproduce the analyses, the general order is:

1. extract and assemble the induction dataset
2. generate baseline descriptive summaries
3. simulate propofol effect-site concentration trajectories
4. run the primary Eleveld analysis
5. run sensitivity analyses
6. export manuscript and supplementary tables/figures

Because the underlying source data are not public, full end-to-end execution requires access to the institutional source dataset or an equivalent local extract.

## Citation

If this repository is used or referenced, please cite the associated correspondence and supplementary appendix.

## Disclaimer

All effect-site concentration values in this project are **model-derived PK/PD constructs** and do not represent directly observed clinical drug effect or clinical outcomes.

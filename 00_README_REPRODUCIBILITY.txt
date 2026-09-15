REPRODUCIBILITY PACKAGE
Psychological selection, relationship gain, maintenance/sorting, and anticipation
BHPS / Understanding Society (UKHLS)

PURPOSE
-------
This folder contains the analysis scripts underlying the current manuscript,
plus a master runner and codebook. The scripts preserve the validated analysis
logic used to generate the reported results.

IMPORTANT REVISION TO THE PSYCHOLOGICAL-SELECTION ANALYSES
---------------------------------------------------------
The former selection specifications based on a respondent's prior psychological
mean plus the current deviation from that mean have been removed from the
reproduction workflow. The current selection analyses use the final
wave-adjusted model-based stable-trait / within-person variability score (WPVS)
decomposition.

Primary measurement specification:
  T = 4 = current clean-unpartnered baseline + the 3 immediately preceding
  UKHLS waves, all with valid psychological measurements.

History-length sensitivities:
  T = 5 and T = 6.

For both life satisfaction and GHQ, the final selection models enter
simultaneously:
  1. stable_score_SD   = model-based stable between-person component;
  2. current_wpvs_SD  = current within-person component at the clean-unpartnered
                         baseline.

GHQ is favorably oriented in this workflow, so higher scores indicate lower
psychological distress. Before decomposition, psychological scores are centered
on the mean of their actual UKHLS survey wave to remove common secular/wave-level
shifts.

The obsolete prior-mean/deviation selection scripts are not included in this
package. Two small preparation scripts create the exact clean-unpartnered risk
set and non-overlapping episode objects required by the final latent analyses;
they do not estimate psychological-selection effects.

DATA ARE NOT INCLUDED
---------------------
The BHPS/UKHLS and Marital and Cohabitation Histories (PHISTORY) microdata are
licensed UK Data Service data and are not redistributed in this package.
Reproduction therefore requires access to the same source release and files.

The scripts currently use these study paths:

UKHLS:
D:/Documenti/psicologia/big data/Understanding Society+BHPS/UKDA-6614-stata/stata/stata14_se/ukhls

BHPS:
D:/Documenti/psicologia/big data/Understanding Society+BHPS/UKDA-6614-stata/stata/stata14_se/bhps

PHISTORY:
D:/Documenti/psicologia/big data/Understanding Society+BHPS/Marital and Cohabitation Histories/stata/phistory_long.dta

The latent measurement stage writes to:
D:/Documenti/UKHLS_latent_stable_WPVS_final_measurement_stage_v2_wavecentered

The final H1 selection stage writes to:
D:/Documenti/UKHLS_H1_latent_stable_WPVS_selection_v1

If the data or project outputs are stored elsewhere, edit the path/configuration
blocks near the start of the component scripts.

HOW TO RUN
----------
1. Put all files in this package in one directory.
2. Confirm the source-data and output paths above, or edit them in the scripts.
3. Install the required R packages.
4. Start a fresh R session.
5. Run:
      source("00_RUN_ALL_ANALYSES.R.txt")
   or from a terminal:
      Rscript 00_RUN_ALL_ANALYSES.R.txt

The master runner launches each component in a separate R process and creates
reproduction_logs/MASTER_RUN_LOG.csv.

REQUIRED R PACKAGES ACROSS THE WORKFLOW
---------------------------------------
haven, dplyr, tidyr, purrr, tibble, readr, stringr, ggplot2, fixest,
survival, splines, brglm2, lavaan, MASS

Each component checks its own dependencies. Component scripts also write
sessionInfo() or equivalent diagnostics where implemented; those files should
be retained with archived results to document exact package versions.

DEFAULT EXECUTION ORDER
-----------------------
00A_prepare_person_wave_panel.R.txt
  Harmonizes BHPS/UKHLS person-wave data used by the LAT and selection modules.

00B_prepare_interview_timing.R.txt
  Reconstructs interview-month timing.

00C_prepare_PHISTORY.R.txt
  Cleans and validates marital/cohabitation histories and date information.

00_dependency_core_v4_1.R.txt
  Recreates the validated v4.1 relationship panel. It is retained because the
  pathway add-ons and final selection episode construction use its reusable RDS
  objects.

01_core_relationship_development_proximity.R.txt
  v4.2 primary relationship-age, lagged coupled-change, and proximity workflow.

01B_relationship_development_sensitivities.R.txt
  Balanced/common-spell relationship-age analyses and strict temporal-ordering
  sensitivities used in the manuscript and supplement.

01C_RDAS_gradient_pair_balanced_diagnostics.R.txt
  Additional RDAS proximity-gradient and pair-balanced diagnostics. Included
  for completeness; controlled by RUN_RDAS_DIAGNOSTIC_ADDON in the master.

01D_Table_S2_postestimation.R.txt
  Reconstructs Table S2 Panels A, B, and D directly from the four saved primary
  relationship-age trajectory models and validates the published Wald tests and
  marriage-minus-dissolution contrasts. No model is refitted.

02_incident_LAT_formation_gain.R.txt
  Preformation and formation-associated psychological change around clean
  incident LAT formation. It also produces the validated incident-LAT event and
  matched-risk-set objects reused by the final selection analysis.

03_prepare_current_unpartnered_selection_base.R.txt
  DATA PREPARATION ONLY. Creates the exact-wave clean-unpartnered landmark base
  (status=single and NCRR1=NO at the same interview). It does not estimate
  psychological-selection models.

03B_prepare_unpartnered_episode_structure.R.txt
  DATA PREPARATION ONLY. Creates non-overlapping clean-unpartnered episode IDs
  and follow-up boundaries. It does not use prior psychological means/deviations
  and does not estimate psychological-selection models.

UKHLS_latent_stable_WPVS_final_measurement_stage_v2_1_WAVECENTERED_AGEFIX.R.txt
  Final wave-adjusted latent measurement/decomposition stage. Produces T=4
  stable between-person and current WPVS scores; T=5 and T=6 are history-length
  sensitivities.

UKHLS_H1_latent_stable_WPVS_selection_v1.R.txt
  FINAL psychological-selection analyses. Tests stable between-person and
  current within-person psychological functioning simultaneously for incident
  LAT formation and subsequent cohabitation/marriage transitions.

05_RDAS_sorting_progression.R.txt
  Current and recent RDAS relationship quality predicting subsequent marriage
  or breakup (primary horizon = 24 months).

06_LAT_to_cohabitation_pathway_no36.R.txt
  CURRENT primary LAT-to-cohabitation pathway analysis. Cohabitation can occur
  at any later observed time; controls are risk-set matched through the treated
  relationship duration.

07_pathway_subjective_anticipation.R.txt
  Marriage pathway and subjective NCRR11/NCRR12 anticipation analyses.
  IMPORTANT: this file also contains an older fixed-36-month LAT-to-cohabitation
  branch. For the CURRENT manuscript, do NOT use that branch as the primary
  cohabitation-pathway result; use script 06 instead.

90_optional_parallel_anticipation_diagnostic.R.txt
  Older parallel/construct-discovery diagnostic. It is NOT run by default and
  is not needed to reproduce the current primary manuscript results.

KEY IDENTIFICATION NOTE
-----------------------
The component workflows are complementary observational designs. They should
not be combined or described as a formal causal mediation/decomposition model.
The stable/WPVS decomposition separates stable between-person psychological
variation from current within-person variation for the prospective selection
analyses; it does not make those associations causal.

REPRODUCIBILITY STATUS
----------------------
The scripts in this package were collected from the validated workflows used in
the manuscript and their dependencies. The licensed UK Data Service source
files are not included, so the numerical analyses require the licensed source
data on the study computer or another authorized environment.

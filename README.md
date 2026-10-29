# Building on prior knowledge

Replication materials for **Building on prior knowledge: Conceptual alignment, knowledge-base coreness, and invention impact across stages of technological development**, by Tae-Seok (Brian) Kim, Eunjung Hyun, and Kyuho Jin.

This package supports the PLOS ONE submission version prepared on 29 October 2026. It contains derived patent-level data, annual co-citation networks, analysis code, and reference outputs for examining stage-specific associations with invention impact in blockchain technology.

## Download

Open the repository's [Releases page](https://github.com/kyuhojin/technological-evolution/releases) and download the attached **PLOS_ONE_Replication_Package_Final.zip**. Extract it and open the `Replication_Package` folder.

Use the attached replication package for the complete data, code, and reference outputs. GitHub's automatically generated source-code archives contain only files committed to the repository.

This package supersedes the earlier release that used centroid similarity as the baseline. The primary conceptual-alignment measure is now **average pairwise similarity**; centroid similarity is a robustness specification.

## Run in Python

Use Python 3.12. From the extracted `Replication_Package` directory:

```bash
python -m pip install -r requirements.txt
python run_all.py
```

Outputs are written to `regenerated/`; existing regenerated outputs are replaced. Deposited comparison outputs are in `reference_results/`. The pipeline includes numerical validation checks. Stata is not required for this workflow.

Exact package versions are recorded in `requirements.txt` and `environment.json`.

## Run the regression models in Stata 18

Set Stata's working directory to the extracted `Replication_Package` directory, then run:

```stata
do code/stata18_replication.do
```

The working directory must contain the `data/` and `code/` subdirectories. The script saves its log and estimation results in `stata_output/`. It estimates the primary models with heteroskedasticity-robust standard errors and separate sensitivity models with `vce(cluster assignee_id)`.

The Stata script covers the regression specifications. Use the Python workflow for the full pipeline, including cross-stage clustered tests, bootstrap analyses, and figures.

## Data and empirical scope

`data/analysis_20260921.dta` contains 11,199 unique patent records. The primary analysis uses 7,892 USPTO blockchain patents filed during 2008–2020:

| Developmental stage | Application years | Primary sample |
| --- | --- | ---: |
| Formation | 2008–2013 | 612 |
| Expansion | 2014–2017 | 1,776 |
| Consolidation | 2018–2020 | 5,504 |

The primary outcome is five-year non-self forward citations. Extended consolidation analyses cover 2018–2022. Alternative stage boundaries and three-year citation-horizon analyses are also provided. Estimation samples differ across specifications.

Conceptual alignment measures overlap with recent prior inventions through average pairwise patent-text similarity (`p_sim_pair_abs_5yr`). Knowledge-base coreness measures the mean weighted Rombach coreness of eligible cited antecedents (`p_core_rb_w_5yr`). See the manuscript and S1 Appendix for construction details.

## Package contents

| File or directory | Purpose |
| --- | --- |
| `run_all.py` | Executes the full Python workflow |
| `requirements.txt`, `environment.json` | Recorded software dependencies |
| `data/analysis_20260921.dta` | Derived patent-level data |
| `data/katz_patent_scores.csv` | Antecedent Katz scores aggregated to focal patents |
| `data/networks/` | Annual weighted co-citation edges and node lists, 2008–2020 |
| `data/eligible_antecedents.csv.gz` | Citation records used to aggregate Katz scores |
| `code/regressions.py` | All 24 reported regression specifications and cross-stage tests |
| `code/stata18_replication.do` | Stata 18 regression replication |
| `code/katz.py` | Reconstructs the Katz measure from annual networks |
| `code/five_year.py`, `code/three_year.py` | Selective-amplification analyses |
| `code/surfaces.py` | Descriptive surface fitting, tuning, and prediction grids |
| `code/descriptives.py`, `code/plots.py` | Descriptive statistics and plots |
| `code/validate.py` | Numerical reproducibility checks |
| `figure2/` | Figure 2 source materials, layout, and provenance |
| `reference_results/` | Verified numerical outputs and figures |
| `reference_results/stata_verification/` | Stata–Python comparison tables and verification notes |
| `submitted_figures/` | Original author-supplied figures for comparison |
| `documents/` | Accompanying manuscript and S1/S2 appendices |
| `README.txt`, `VALIDATION.txt` | Detailed instructions and validation record |
| `MANIFEST_SHA256.txt` | File checksums |

## Analyses and reporting

The package includes primary stage-specific Poisson QMLE models, NB2 models, centroid-alignment specifications, alternative stage boundaries, extended-window specifications, Katz-adjusted models, and assignee-clustered inference. It also supplies selective-amplification analyses, descriptive surfaces, and figure materials.

Complete regression coefficients, standard errors, confidence intervals, and p values are in `reference_results/regression/full_coefficients.csv`. Model-fit summaries are in `model_fit.csv`, covariance matrices in `fits.json`, estimation-sample identifiers in `model_sample_ids.csv`, and cross-stage tests in `cross_stage_tests.csv` in the same directory. S2 Appendix provides readable tables.

Nonclustered sandwich covariance matrices use the N/(N − 1) correction. Assignee-clustered covariance matrices use G/(G − 1), without a regressor-count adjustment. Cross-stage clustered comparisons account for assignees represented in more than one stage. These are not assignee fixed-effects models; the primary clustered analyses retain all 7,892 patents.

The Katz robustness specification uses an attenuation parameter of 0.75 divided by each annual network's largest eigenvalue. Original-unit regression coefficients and standardized incidence-rate ratios are alternative presentations of the same fitted models.

## Validation and software

The complete Python pipeline was successfully executed on 29 October 2026. It checks focal coefficients, estimation samples, clustered tests, surface-tuning choices, and numerical agreement with the reference outputs.

**All 24 regression specifications were independently run in Stata 18.** Comparison of 497 coefficient rows in the displayed Stata log with the Python output confirmed identical estimation-sample sizes and agreement in coefficients, standard errors, confidence intervals, and NB2 dispersion estimates within negligible numerical differences. This comparison used the printed log, not full-precision extraction of `.ster` files.

**Cross-stage clustered tests were implemented and checked in Python; they were not independently reproduced in Stata.** Small numerical differences may occur across software versions and platforms.

Stata reference: StataCorp. 2023. *Stata Statistical Software: Release 18.* College Station, TX: StataCorp LLC.

## Scope and limitations

This is replication from derived inputs, rather than an end-to-end reconstruction from raw patent records and text. It does not recreate PatentSBERTa embeddings, the BERTopic fit, original citation-window and self-citation construction, or stochastic Rombach optimization from scratch. Those measures are supplied as inputs.

The package does reconstruct the added Katz measure from annual networks, including isolates, and checks it against deposited patent scores. Figure 2 uses supplied coordinates and coreness scores; it does not rerun the stochastic layout or original coreness estimation. Some regenerated figures may differ stylistically from the submitted figures while preserving the reported quantities.

## Citation and contact

Please acknowledge the manuscript and the specific replication release used:

Kim, T.-S. (Brian), Hyun, E., and Jin, K. *Building on prior knowledge: Conceptual alignment, knowledge-base coreness, and invention impact across stages of technological development.*

Repository: https://github.com/kyuhojin/technological-evolution

Correspondence:

- Kyuho Jin: kyuhojin@gist.ac.kr
- Eunjung Hyun: ejhyun@hongik.ac.kr

## Licence

No new code or data reuse licence is assigned by this README. Please contact the corresponding authors concerning reuse and applicable source-data terms.

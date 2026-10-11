# Semantic and knowledge-base coreness in technological evolution

Analysis replication materials for the manuscript by Tae-Seok (Brian) Kim, Eunjung Hyun, and Kyuho Jin.

This repository contains the derived patent-level dataset and code for examining stage-specific associations between semantic coreness, knowledge-base coreness, and subsequent patent citations in blockchain technology.

## Download and run

1. Download [Replication_Package.zip](https://github.com/kyuhojin/technological-evolution/raw/refs/heads/main/Replication_Package.zip).
2. Extract the archive and open a terminal in the extracted `Replication_Package` directory, which contains `run_all.py`.
3. Using Python 3.12, install the tested dependencies and run:

```bash
python -m pip install -r requirements-tested.txt
python run_all.py
```

Outputs are written to `regenerated/`. The supplied comparison outputs are in `reference_results/`. Stata is not required for the Python workflow.

## Data and empirical scope

The file `analysis_20260921.dta` contains 11,199 unique patent observations. The primary analysis uses 7,892 USPTO blockchain patents filed during 2008–2020:

| Developmental period | Application years | Primary sample |
| --- | --- | ---: |
| Formative | 2008–2013 | 612 |
| Expansion | 2014–2017 | 1,776 |
| Consolidation | 2018–2020 | 5,504 |

The primary outcome is five-year non-self forward citations, with citation data observed through 31 December 2025. The extended consolidation check covers 2018–2022; alternative period boundaries and a three-year citation horizon are also examined. Not all observations in the data file enter every analysis.

Semantic coreness measures proximity to the field's recent conceptual center. Knowledge-base coreness measures the structural coreness of an invention's antecedent knowledge. See the manuscript and Supplementary Information for definitions and construction methods.

## Package contents

| File or directory | Purpose |
| --- | --- |
| `analysis_20260921.dta` | Derived patent-level analysis data |
| `run_all.py` | Executes the Python replication workflow |
| `requirements-tested.txt` | Exact dependency versions used in the recorded validation |
| `requirements.txt` | Unpinned dependency list |
| `code/matched_nb.py` | Negative-binomial estimation |
| `code/standardize.py` | Poisson estimation, final robust covariance convention, and cross-stage tests |
| `code/figures.py` | Regression-based Figures 3 and S2 |
| `code/five_year.py` | Five-year cohort-adjusted selective-amplification analysis |
| `code/three_year.py` | Three-year cohort-adjusted selective-amplification analysis |
| `code/matched_nb.do` | Alternative Stata model-specification reference |
| `reference_results/` | Reference regression and selective-amplification outputs |
| `VALIDATION.txt` | Record of the completed replication check |
| `MANIFEST_SHA256.txt` | File checksums |

## Analyses reproduced

The workflow reproduces:

- Primary stage-specific Poisson estimates and matched-sample negative-binomial robustness estimates.
- Cross-stage Wald tests, alternative period boundaries, the pairwise semantic-similarity specification, and extended consolidation estimates.
- Standardized regression plots for manuscript Figure 3 and Supplementary Figure S2.
- Cohort-adjusted selective-amplification estimates for cumulative five-year and three-year citations, including exclusion of highly cited patents.

For final regression reporting, use `regenerated/all_estimates.csv` and `regenerated/fits.json`. Cross-stage tests are in `regenerated/cross_stage.csv`. Each citation-horizon subdirectory contains `amplification.csv` and `cohort_amplification.csv`.

The final covariance convention applies the N/(N − 1) finite-sample adjustment to the robust covariance matrix. Intermediate negative-binomial diagnostic outputs precede this adjustment.

The extended Poisson implementation omits the cohort-specific exposure offset because saturated application-year fixed effects absorb it; the focal slope estimates are unchanged. This equivalence does not independently validate an exposure assumption.

Selective-amplification analyses use 4,000 bootstrap resamples stratified by application year, with seed 20261009. They do not control for earlier citation counts. Seven-year citation values are used only for source-data consistency checks, not for reported horizon analyses.

## Validation

The recorded full run completed on 10 October 2026 using Python 3.12.14, NumPy 2.3.5, pandas 2.2.3, SciPy 1.17.0, and Matplotlib 3.10.8. Regenerated regression coefficients, standard errors, and p values, and both citation-horizon amplification estimates and bootstrap limits matched the reference CSV values at stored precision.

This validates reproduction from the supplied derived data. Small numerical differences may occur with other software versions or platforms.

## Scope and limitations

This package reproduces statistical analyses from the derived dataset. It is not an end-to-end reconstruction from raw patent records.

It does not include the raw citation edge list, abstract embedding vectors, embedding pipeline, annual core-periphery construction pipeline, or original citation-window and self-citation construction code. It also does not regenerate manuscript Figures 1, 2, and 4 or Supplementary Figures S1 and S3.

The manuscript and Supplementary Information describe the patent search query, embedding model, and self-citation definition. Their original implementation cannot be independently audited from this package alone.

The archive's `DEPOSIT_INSTRUCTIONS.txt` and pre-deposit note in `README.txt` describe preparation before this GitHub upload; they are not additional analysis steps.

## Citation and contact

Please acknowledge the manuscript and this repository when using these materials:

Kim, T.-S. (Brian), Hyun, E., and Jin, K. *Semantic and knowledge-base coreness in technological evolution.*

Repository: https://github.com/kyuhojin/technological-evolution

Correspondence:

- Kyuho Jin: kyuhojin@gist.ac.kr
- Eunjung Hyun: ejhyun@hongik.ac.kr

## Licence

No explicit code or data licence is included in the replication archive. Public access should not be interpreted as an unrestricted reuse licence. Please contact the corresponding authors concerning reuse and applicable source-data terms.

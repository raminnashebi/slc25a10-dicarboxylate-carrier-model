# Mathematical Modelling of the Mitochondrial Dicarboxylate Carrier (SLC25A10)

This repository contains the core MATLAB code and experimental datasets for the revised manuscript
**“Mathematical Modelling of the Mitochondrial Dicarboxylate Carrier (SLC25A10)”**.

The model is calibrated against two experimental systems:

- **Intact mitochondria**: succinate, malate and phosphate uptake assays.
- **Proteoliposomes**: reconstituted-carrier malate/phosphate exchange assays.

For each system, the repository includes Bayesian parameter inference (Metropolis–Hastings MCMC), convergence diagnostics, and an ODE simulation that uses the inferred parameters.

The complete bundle used to reproduce *every* figure and table in the revised article is archived on Zenodo (see [below](#full-reproducibility-package-zenodo)).

---

## Repository structure

```
.
├── data/                          Experimental datasets (.mat), see data/README.md
│   ├── intact_mitochondria/
│   └── proteoliposome/
├── src/                           Reusable functions
│   ├── models/
│   │   ├── slc25a10_mito_ode.m        ODE right-hand side, intact mitochondria
│   │   └── slc25a10_liposome_ode.m    ODE right-hand side, proteoliposomes
│   └── utils/
│       └── optimalBatchSizeMultivariate.m   MCMC batch size / MCSE / ESS helper
├── scripts/                       Runnable analysis scripts, numbered in run order
│   ├── intact_mitochondria/
│   │   ├── mito_01_run_mcmc.m
│   │   ├── mito_02_mcmc_diagnostics.m
│   │   ├── mito_03_rhat_analysis.m
│   │   └── mito_04_simulate_ode.m
│   └── proteoliposome/
│       ├── liposome_01_run_mcmc.m
│       ├── liposome_02_mcmc_diagnostics.m
│       ├── liposome_03_rhat_analysis.m
│       └── liposome_04_simulate_ode.m
├── results/                       Created when scripts run (not tracked by git)
├── LICENSE
└── README.md
```

## Requirements

- MATLAB. Only base MATLAB is required (no toolboxes). The scripts use `tiledlayout`, which needs R2019b or later. Tested with R2026a.

## How to run

Every script finds the repository root from its own location, so you can run it from any working folder (e.g. `run('scripts/proteoliposome/liposome_01_run_mcmc.m')` or open it in the editor and press **Run**). Scripts read inputs from `data/` and write all outputs to `results/<system>/`, creating the folder if needed.

Run the scripts for each system in numbered order:

| Step | Intact mitochondria | Proteoliposome | What it does |
|------|---------------------|----------------|--------------|
| 1 | `mito_01_run_mcmc.m` | `liposome_01_run_mcmc.m` | Metropolis–Hastings MCMC (10⁶ iterations, `rng(42)`). Saves the posterior `.mat`, a LaTeX summary table and posterior/fit figures. |
| 2 | `mito_02_mcmc_diagnostics.m` | `liposome_02_mcmc_diagnostics.m` | Trace plots, autocorrelation, ESS and MCSE for the step 1 result. |
| 3 | `mito_03_rhat_analysis.m` | `liposome_03_rhat_analysis.m` | Gelman–Rubin R̂ across four chains. |
| 4 | `mito_04_simulate_ode.m` | `liposome_04_simulate_ode.m` | Solves the ODE model with the posterior mean from step 1. If step 1 has not been run, it falls back to the posterior means reported in the manuscript. |

**Notes**

- Step 1 runs 1,000,000 MCMC iterations. To regenerate figures from an existing result without re-sampling, set `run_mcmc = false` in `liposome_01_run_mcmc.m`.
- Step 3 expects four chain files named `chain1_result_….mat` … `chain4_result_….mat` in `results/<system>/` (exact names are listed in each script's header). No script in this repository produces these files, so add them before running step 3.

### Model parameters

| System | Sampled parameters | Derived quantities |
|--------|--------------------|--------------------|
| Intact mitochondria | `Ts_max, Tm_max, Tp_max` (µmol/min/g), `K_D_s, K_D_m, K_D_p` (mM), `sigma2` | `lambda21 = (Tm_max·K_D_s)/(Ts_max·K_D_m)`, `lambda31 = (Tp_max·K_D_s)/(Ts_max·K_D_p)` |
| Proteoliposome | `Tmax_m, Tmax_p` (mmol/min/g), `K_D_m, K_D_p` (mM), `sigma2` | `rho_pm = (Tmax_p·K_D_m)/(Tmax_m·K_D_p)` |

---

## Mapping to the previous layout

Files were renamed when the repository was reorganised. Use this table to match files here with the Zenodo archive.

| Previous path | New path |
|---------------|----------|
| `Bayesian_MCMC_MH_CODES/Mito_calibration/SLC25A10_REA_Palmier_MH_MCMC_umolmin_g5_reduced_uniform.m` (and its identical copy `…_umolmin_g5.m`) | `scripts/intact_mitochondria/mito_01_run_mcmc.m` |
| `Bayesian_MCMC_MH_CODES/Mito_calibration/MCMC_Diagnostics.m` | `scripts/intact_mitochondria/mito_02_mcmc_diagnostics.m` |
| `Bayesian_MCMC_MH_CODES/Mito_calibration/Rhat_analysis.m` | `scripts/intact_mitochondria/mito_03_rhat_analysis.m` |
| `ODE_CODES/Trial.m` | `scripts/intact_mitochondria/mito_04_simulate_ode.m` |
| `Bayesian_MCMC_MH_CODES/proteoliposome_calibration/run_FIG5_reduced_Jpho_gaussian_IGsigma_no_exMal1mM.m` (and its identical copy `MCMC_REA_SLC25A10_fit_expUnits_mmolMinPerG5.m`) | `scripts/proteoliposome/liposome_01_run_mcmc.m` |
| `Bayesian_MCMC_MH_CODES/proteoliposome_calibration/MCMC_Diagnostics.m` | `scripts/proteoliposome/liposome_02_mcmc_diagnostics.m` |
| `Bayesian_MCMC_MH_CODES/proteoliposome_calibration/Rhat_analysis.m` | `scripts/proteoliposome/liposome_03_rhat_analysis.m` |
| `ODE_CODES/Trial_proteoliposome_reduced_Jpho.m` | `scripts/proteoliposome/liposome_04_simulate_ode.m` |
| `ODE_CODES/slc25a10_model_updated_REA.m` | `src/models/slc25a10_mito_ode.m` |
| `ODE_CODES/slc25a10_model_liposome_reduced_Jpho.m` | `src/models/slc25a10_liposome_ode.m` |
| `Bayesian_MCMC_MH_CODES/*/optimalBatchSizeMultivariate.m` | `src/utils/optimalBatchSizeMultivariate.m` |
| `Experimental_Datasets/intact_mitochondria_uptake_assay/*.mat` | `data/intact_mitochondria/*.mat` |
| `Experimental_Datasets/proteoliposome_uptake_assa/*.mat` | `data/proteoliposome/*.mat` |

---

## Full reproducibility package (Zenodo)

The full package (including scripts used to generate **each figure and table** in the revised manuscript, plus additional large files) is available on Zenodo:

- **Zenodo record:** https://zenodo.org/records/20524461?token=eyJhbGciOiJIUzUxMiJ9.eyJpZCI6IjgzZDViZTUwLWQyYWYtNDMwYS1iZTlkLTgyYzRlNzg3NGNlMCIsImRhdGEiOnt9LCJyYW5kb20iOiIxZjM4MWM4YWNhYmM2ZDExYWU2YmE5NWRlZjcxNGI4ZSJ9.IEoJOHUwlRs3cI6kTqOXIvbjhLWixUzvv5xIut8qXzCsxc5qgVuByEjlM6pPW8a2qISO3UrtYsem8-Rkn0YLYw
- **DOI:** [10.5281/zenodo.18664645](https://doi.org/10.5281/zenodo.18664645)

Recommended citation format:
> Please cite the Zenodo archive (DOI above) when using the full figure/table generation workflow.

## License

Released under the MIT License. See [LICENSE](LICENSE).

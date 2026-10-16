# Experimental datasets

All files are MATLAB `.mat` files containing row or column vectors. Concentrations are in mM. Source references are given in the manuscript.

The "Used by" column says how the scripts in `scripts/` read each variable. Variables marked *not used* are kept for completeness and are not read by any script in this repository.

## `intact_mitochondria/`

Uptake assays in intact mitochondria. Rates are in µmol/min/g protein.

| File | Variables | Used by |
|------|-----------|---------|
| `Palmier_DIC_Exp_Data.mat` | `S`, `T_S` | `mito_01_run_mcmc.m`: external succinate and succinate uptake rate, used as stored. |
| | `P`, `T_P` | `mito_01_run_mcmc.m`: stored as reciprocals; the script uses `1./P` (phosphate) and `1./T_P` (rate). |
| | `M`, `T_M` | *not used* |
| `Palmier_DIC_Competition_Exp_Data.mat` | `M_with_S`, `T_M_with_S` | `mito_01_run_mcmc.m`: stored as reciprocals; the script uses `1./M_with_S` (malate, with 0.5 mM external succinate) and `1./T_M_with_S` (rate). `mito_04_simulate_ode.m` uses `1./M_with_S(1)` as the initial external malate. |
| | `M_control`, `T_M_control`, `M_with_P`, `T_M_with_P` | *not used* |
| `Palmier_DIC_Pho_Exp_Data.mat` | `P_with_M`, `T_P_with_M` | *not used* |

## `proteoliposome/`

Exchange assays with reconstituted carrier. Each file holds an x-vector and a matching `_Rate` vector (mmol/min/g protein). Fixed concentrations in the table are the values set in `load_liposome_data` in `liposome_01_run_mcmc.m`.

| File | x-variable | Fixed condition | Used by |
|------|------------|-----------------|---------|
| `Proteoliposome_intPho_exMAL.mat` | external malate | internal phosphate 15 mM | `liposome_01_run_mcmc.m` (malate flux) |
| `Proteoliposome_intMAl_exPHO.mat` | external phosphate | internal malate 15 mM | `liposome_01_run_mcmc.m` (phosphate flux) |
| `Proteoliposome_1993_intPho_10mM_exMAL.mat` | external malate | internal phosphate 1.0 mM | `liposome_01_run_mcmc.m` (malate flux) |
| `Proteoliposome_1993_intPho_25mM_exMAL.mat` | external malate | internal phosphate 2.5 mM | `liposome_01_run_mcmc.m` (malate flux) |
| `Proteoliposome_1993_exMal_020mM_inPho.mat` | internal phosphate | external malate 0.2 mM | `liposome_01_run_mcmc.m` (malate flux) |
| `Proteoliposome_1993_exMal_1mM_inPho.mat` | internal phosphate | | *not used* (deliberately excluded from the fit; see the script header) |
| `Proteoliposome_1993_exMal_010mM_inPho.mat` | internal phosphate | | *not used* |
| `Proteoliposome_1993_exMal_013mM_inPho.mat` | internal phosphate | | *not used* |
| `Proteoliposome_1993_exMal_030mM_inPho.mat` | internal phosphate | | *not used* |
| `Proteoliposome_1993_intPho_05mM_exMAL.mat` | external malate | | *not used* |
| `Proteoliposome_1993_intPho_50mM_exMAL.mat` | external malate | | *not used* |

# LF-NGRC code (compact submission package)

Minimal package for reproducing the paper figures/tables: one merged
plotting script, the data-generating experiment scripts, and the plotting
data (bundled into a few files).

## Layout

```
LF_NGRC_code_compact/
├── code/
│   ├── plot_figures.py         ALL figures (Fig. 1-7) in one script
│   ├── (experiment scripts, see table below)
│   └── outputs/
│       ├── sweeps/
│       │   ├── j.json          J sweep per-seed results        (Fig. 3c)
│       │   ├── ntraj.json      N_traj sweep                    (Fig. 6a)
│       │   ├── lamb.json       lambda sweep                    (Fig. 6b)
│       │   ├── dt.json         Delta t sweep                   (Fig. 6c)
│       │   ├── verify20.json   independent-seed runs (20-39)
│       │   ├── d3_ablation.json spectral ablation, 20 seeds    (Fig. 5)
│       │   ├── poly_rbf_seed_stats.json                    (Fig. 2)
│       │   └── mlp_multi_seed_summary.json                 (Fig. 2)
│       ├── fine_dt_summary.json
│       └── figures/            PNG output folder
└── data/                       canonical precomputed arrays (24 files)
```

## Quick start (figures only, from saved data)

```bash
cd code
python plot_figures.py                 # all seven figures
python plot_figures.py fig3 fig6       # selected figures
```

Figure mapping: `fig1` pendulum basins, `fig2` error bars, `fig3` J=4
over-complete (three panels), `fig4` sensitivity + force field, `fig5`
spectral-radius ablation, `fig6` parameter dependence (three panels),
`fig7` Kuramoto.

## Experiment scripts (data generation)

| Script | Produces |
|---|---|
| `repro_catch22.py` | Catch-22 exact baseline (Table I) |
| `baseline_poly_rbf.py` | poly / random-RBF baselines (Table I, Fig. 1b,c) |
| `poly_rbf_seed_stats.py` | 20-seed poly/RBF statistics (Fig. 2) |
| `run_main_experiment.py` | main pendulum 20-seed results (Fig. 2) |
| `j4_experiment.py` | J=4 runs + nine-initialization test (Fig. 3) |
| `sensitivity_fixed_init.py` | sensitivity curve + fixed-attractor init (Fig. 4a) |
| `spectral_radius_diagnostics.py` | rho / sigma_min(I-J) (Sec. IV.C) |
| `sweep_pendulum.py` | parameter sweeps j / ntraj / lamb / dt (Fig. 3c, Fig. 6) |
| `dt_onestep_fill.py` | one-step column of the Delta t sweep |
| `verify_new_seeds.py` | independent-seed runs (seeds 20-39) |
| `fine_dt_baseline.py` | fine-step exact baseline (Table I) |
| `kuramoto_lf.py` | Kuramoto representative run (Fig. 7) |
| `kuramoto_multi_seed.py` | Kuramoto 20-seed statistics |
| `kuramoto_gamma_sweep_fix.py` | Kuramoto gamma sweep (Fig. 7d) |
| `mlp_baseline.py`, `mlp_multi_seed.py` | MLP baselines (Table IV, Fig. 2) |
| `spectral_ablation_compute.py` | spectral ablation (Fig. 5) |

Shared modules: `rc_core.py`, `repro_catch22.py`, `lf_ngrc.py`,
`run_lfngrc.py`.

After a full rerun (per-seed json files in `code/outputs/sweeps/`), the
bundles can be regenerated with:

```bash
python plot_figures.py pack
```

## Environment

- Python >= 3.10; tested with 3.13.5, numpy 2.1.3, scipy 1.15.3,
  torch 2.12.0, matplotlib 3.10.0 (float64, CPU).
- On Windows, set `KMP_DUPLICATE_LIB_OK=TRUE` before importing torch.
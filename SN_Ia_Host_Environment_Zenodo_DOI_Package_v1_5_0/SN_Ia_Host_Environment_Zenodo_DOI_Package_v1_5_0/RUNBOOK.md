# Reproduction runbook

## Environment

```bash
conda env create -f environment.yml
conda activate snia-host-environment
```

## Verify the extracted archive

```bash
python checks/verify_release.py
```

## Recompute the five-bin calibration

```bash
python code/76_statistical_calibration_audit.py
```

## Rerun selection-forward heterogeneity

```bash
python code/71_selection_stress_heterogeneity_Q.py --root . --outdir rerun_phase2
```

## Rerun object-level influence

```bash
python code/73_object_level_jackknife_influence.py   --input data/processed/ZTF_DR2_validated_residual_layer.csv   --outdir rerun_phase3
```

## Rerun the environmental audit

```bash
python code/74_environment_stratification_interaction.py   --input data/processed/ZTF_DR2_validated_residual_layer.csv   --outdir rerun_phase4   --n-bootstrap 2000   --n-permutation 5000   --seed 20260716
```

## Rerun the external-product audit

```bash
python code/75_external_product_common_support_influence.py   --root .   --outdir rerun_phase5   --n-bootstrap 2000   --seed 20260721
```

## Regenerate Figure 1

Python:

```bash
python code/make_fig01_correction_validity_workflow.py
```

MATLAB, from the package root:

```matlab
run('code/make_fig01_correction_validity_workflow.m')
```

The source scripts may create a PNG preview during local figure development;
only the vector PDF belongs in the deposition archive.

## Insert a reserved Zenodo DOI

```bash
python checks/set_reserved_doi.py 10.5281/zenodo.XXXXXXX
```

The release includes frozen stochastic draws and summaries. Exact draw-level
reproduction requires the listed seeds, package versions, and input tables.
Small floating-point differences can occur across BLAS implementations.

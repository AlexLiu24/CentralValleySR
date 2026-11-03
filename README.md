# CentralValleySR

Research code and artifacts for evaluating Sentinel-2 super-resolution and crop-specific vegetation signals in California's northern Central Valley.

The project notebook builds annual precipitation labels and crop-specific NDVI baselines, creates and audits low-/high-resolution image pairs, trains a 4× super-resolution model, and evaluates model results against bicubic upsampling. The study covers 2018–2022 and focuses on alfalfa, almond, citrus, and grapes. Super-resolution conclusions exclude any target crop that has no usable image patches; precipitation groups are relative to the study-period mean, not drought classifications.

## Project layout

| Path | Contents |
| --- | --- |
| `notebooks/CentralValleySR_notebook.ipynb` | End-to-end, numbered research workflow (Steps 0–15) |
| `src/` | Reusable data preparation, patching, model, and training modules |
| `data/` | Data directories and notes; large or externally sourced datasets may not be included |
| `results/` | Tabular analysis outputs and run manifest |
| `figures/` | Generated figures |
| `paper/` | Manuscript and references |

## Run the workflow

Run `notebooks/CentralValleySR_notebook.ipynb` from the repository root in Google Colab or a local Jupyter environment, in order from Step 0 through Step 15. The notebook writes its outputs to `data_pairs/sr_pairs_x4/`, `results/`, `figures/paper/`, `checkpoints/`, and `frozen_artifacts/` by default; the `CVSR_*_DIR` environment variables documented in the notebook can override these paths.

The workflow uses Python with Earth Engine, NumPy, pandas, Matplotlib, scikit-image, SciPy, PyTorch, and torchvision. Install these packages in the notebook environment before running; Step 2 records the installed package versions in `requirements.txt` for reproducibility. PyTorch uses CUDA when available, then Apple Metal (MPS), and otherwise CPU.

Steps that fetch imagery or precipitation from Google Earth Engine require an Earth Engine account, authentication, and a Cloud project. Set `EE_PROJECT` to the project ID before running; the notebook can prompt for authentication. Cached inputs can be used instead when available, without regenerating image pairs.

## Reproducibility

The notebook records configuration and provenance in `results/run_manifest.json`, produces data-quality and split summaries, and freezes final outputs for a readiness check in Step 15. Run the workflow from a clean copy or point its output-directory settings at a separate location to preserve existing results. Consult the notebook's output checklist and `paper/` materials when interpreting or citing results.

## Citation and license

See `CITATION.cff` for citation metadata and `LICENSE` for the project license.

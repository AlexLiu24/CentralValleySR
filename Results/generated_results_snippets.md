# Generated Results Snippets
## Provenance
- Baseline NDVI source: cached_csv:/content/results/baseline_ndvi_all_crops.csv
- SR pair source: cached_real_npz:/content/data_pairs/sr_pairs_x4 (15000 files)
- Precipitation-group source: cached_csv:/content/results/annual_precipitation_labels.csv (results/annual_precipitation_labels.csv)
- Paper-ready flag: recorded in results/run_manifest.json, set at the end of Step 15 against the frozen package
- Detected SR scale: x4 | channels: 4 | bands: ['B4', 'B3', 'B2', 'B8']
- Valid patches: 15000 of 15000 (lr_match enforced)
- Train/val/test patches used: 9000/3000/3000
- Seed: 42 | device: cuda
## Crop coverage of the SR results
ABSENT TARGET CROPS: CITRUS has ZERO super-resolution patches in the entire 15000-patch dataset and zero in the 3000-patch test set. It is excluded from every SR result and claim (zero SR patches in the generated pair set (CDL citrus effectively absent from this AOI); retained in the baseline NDVI analysis only), and appears only in the baseline NDVI analysis.
Target crops in the study design: ['ALFALFA', 'ALMOND', 'CITRUS', 'GRAPES']. Present in the SR dataset: ['ALFALFA', 'ALMOND', 'GRAPES']. Excluded from SR claims: ['CITRUS'].
## Methods sentences (copy these; they match the code)
Sentinel-2 scenes were filtered to CLOUDY_PIXEL_PERCENTAGE <= 60 and cloud-masked with SCL classes [4, 5, 6, 7] retained. Class 7 is unclassified/low-probability cloud and was kept to preserve coverage; residual contamination is disclosed as a limitation.
For each year, a cloud-masked 06-01 to 09-30 median Sentinel-2 composite was generated, and HR patches were extracted from these growing-season composites. Patches were not sampled year-round.
Sentinel-2 scenes passing the cloud-filtering criteria were processed individually for the baseline; for each scene, mean NDVI was calculated over pixels assigned to the corresponding crop class within the AOI, reduced at a 60 m analytical scale (B4 and B8 originate from native 10 m bands).
Higher- versus lower-precipitation NDVI differences were computed from ANNUAL mean NDVI, one value per crop-year, so that years contributing more usable Sentinel-2 scenes do not dominate the comparison (results/baseline_ndvi_annual_summary.csv). The scene-level table is retained for reference only.
A crop-coverage threshold of 0.3 was applied, meaning at least 30% of CDL pixels in a candidate patch had to belong collectively to one of the target crop classes; the dominant crop and its own fraction of the patch were recorded separately for every patch.
Years with annual PRISM precipitation above the 2018-2022 study-period mean were classified as higher-precipitation years, and years below it as lower-precipitation years. This is a relative grouping, not a climatological drought classification.
## Split strategy
Strategy actually used: **temporal**. A chronological holdout was used to prevent same-composite leakage between nearby or overlapping ~1.28 km patches drawn from the same growing-season composite: train = [2018, 2019, 2020], validation = 2021, test = 2022. Because the same geographic fields may recur across years, this tests temporal generalisation within one region rather than complete spatial independence.
Resulting sizes: 9000 train / 3000 validation / 3000 test patches.
## Dataset composition
The dataset is dominated by GRAPES (62.8% of all 15000 patches); see results/patch_counts_by_year_crop.csv.
## Baseline NDVI result (descriptive only - no inferential tests)
Across crops, Citrus showed the largest difference between precipitation groups (-0.0994 NDVI, -18.4%), comparing 3 higher-precipitation years against 2 lower-precipitation years using annual mean NDVI. Full per-crop values are in results/baseline_ndvi_annual_summary.csv. Welch t-tests were removed: repeated scene-level observations from one AOI are temporally autocorrelated, and five successive years are not independent replicates.
## Super-resolution performance (TEST set, n=3000 agricultural patches, per-patch paired evaluation)
These are AGGREGATE agricultural-patch results across all held-out test patches. They are not crop-specific and must not be presented as covering every target crop.
Bicubic PSNR 36.595 dB vs model PSNR 38.558 dB. NDVI RMSE bicubic 0.0668 vs model 0.0490. See results/sr_bicubic_vs_model_metrics.csv for SD, median, 95% CI and the paired model-minus-bicubic difference.
Where results are broken down by crop, they cover ALFALFA, ALMOND, GRAPES only, and the labels are dominant-crop labels: the 30% threshold applies to combined target-crop cover, so these are dominant-crop-labelled patches rather than pure single-crop patches. results/sr_metrics_by_crop.csv also reports the stricter subset whose dominant crop alone covers >= 30% of the patch.
Every target crop PRESENT in the SR dataset (ALFALFA, ALMOND, GRAPES) has at least 30 dominant-crop-labelled test patches; see results/sr_metrics_by_crop.csv for the breakdown. This covers the present crops only and makes no claim about absent crops.
Qualitative examples were selected by the rule 'median_psnr' (results/figure_example_patches.csv).
## Wording guardrails
- Report these as reconstruction-fidelity results, not crop-stress detection.
- Say higher-/lower-precipitation, never wet/drought.
- Say TEST set, not validation set, for every final number.
- Describe the SR headline numbers as aggregate agricultural-patch results.
- Crop-level SR statements cover ALFALFA, ALMOND, GRAPES only, as dominant-crop-labelled patches. Never write 'crop-specific patches'.
- CITRUS: baseline NDVI only. No SR claim, no SR table row with metrics, no implication of coverage.
- The baseline precipitation comparison is annual-mean based and descriptive.
- The model has SIX residual blocks.
- LR inputs are idealised 4x area-averaged degradations of the same native 10 m Sentinel-2 target, so this is not validation against independent sub-10 m imagery.
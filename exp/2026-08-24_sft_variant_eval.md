# CalibSFT SFT-Variant Evaluation

## Evaluation status

- Evaluation job: `verl-eval-0824153503` (`job-311xtgwllu25`).
- The job was stopped after the original dataset-first scheduler left most GPUs idle on `DROP`.
- Results below use the common completed set: eight mathematical datasets and four OOD datasets.
- `DROP` is excluded because its evaluation was not completed.
- Metrics are reported in percentage points. Lower is better for Brier and ECE.
- AIME accuracy is the per-generation `accuracy`; `pass@32` is reported separately.

## Checkpoints

| Variant | Checkpoint |
|---|---|
| CE | `logs/train/deep_scale_r/confidence_sft_ce/qwen3_8b/probability/2026-0823-053458/global_step_180/huggingface` |
| Full | `logs/train/deep_scale_r/confidence_sft_ce_full/qwen3_8b/probability/2026-0823-064330/global_step_180/huggingface` |
| Balanced | `logs/train/deep_scale_r/confidence_sft_ce_balanced/qwen3_8b/probability/2026-0823-074615/global_step_180/huggingface` |
| CalibSFT | `logs/train/deep_scale_r/calib_sft/qwen3_8b/probability/2026-0823-095127/global_step_180/huggingface` |
| Selective-CorrectConf | `logs/train/deep_scale_r/confidence_sft_ce_selective_correct_conf/qwen3_8b/probability/2026-0823-210959/global_step_180/huggingface` |
| Base | `Qwen/Qwen3-8B` |

## Aggregate results

### Mathematical datasets

Average over `DeepScaleR_Eval`, `Math500`, `MinervaMath`, `OlympiadBench`, `GSM8K`, `AIME2024`, `AIME2025`, and `AIME2026`.

| Variant | Accuracy | AUROC | Brier | ECE |
|---|---:|---:|---:|---:|
| CE | 43.56 | 74.62 | 20.97 | 17.43 |
| Full | 65.89 | 61.66 | 29.29 | 27.65 |
| Balanced | 65.46 | 80.67 | 17.04 | **13.87** |
| CalibSFT | 65.79 | 79.30 | 17.89 | 14.36 |
| Selective-CorrectConf | **66.59** | **81.58** | **16.95** | 14.28 |
| Base | 66.41 | 71.33 | 27.96 | 28.56 |

### OOD datasets

Average over `HotpotVanilla`, `TriviaQA`, `MuSR`, and `MuSiQue`.

| Variant | Accuracy | AUROC | Brier | ECE |
|---|---:|---:|---:|---:|
| CE | 48.34 | 64.32 | 24.43 | 12.36 |
| Full | 56.55 | 60.69 | 31.27 | 28.67 |
| Balanced | 55.96 | 63.03 | 27.85 | 20.15 |
| CalibSFT | 56.81 | 61.42 | 29.37 | 22.38 |
| Selective-CorrectConf | **56.86** | **65.39** | **24.13** | **12.27** |
| Base | 55.76 | 62.13 | 36.86 | 36.84 |

### AIME pass@32

Average over AIME2024, AIME2025, and AIME2026.

| Variant | Average pass@32 |
|---|---:|
| CE | 74.44 |
| Full | **86.67** |
| Balanced | 84.44 |
| CalibSFT | 85.56 |
| Selective-CorrectConf | 85.56 |
| Base | 84.44 |

## Confidence diversity

The table reports the mean confidence standard deviation and mean top-1 confidence share. A lower top-1 share indicates less concentration in a single confidence value.

| Variant | Math std | Math top-1 share | OOD std | OOD top-1 share |
|---|---:|---:|---:|---:|
| CE | 28.15 | **18.02** | 23.01 | 28.38 |
| Full | 36.52 | 45.33 | 30.49 | 59.32 |
| Balanced | 32.62 | **26.30** | 30.33 | **23.73** |
| CalibSFT | 33.86 | 26.62 | 31.93 | 29.37 |
| Selective-CorrectConf | 16.51 | 41.88 | 12.50 | 56.84 |
| Base | 10.38 | 48.32 | 7.40 | 63.19 |

## Findings

1. **Selective-CorrectConf is the strongest primary candidate.** It has the highest mean mathematical accuracy, AUROC, and lowest mathematical Brier score. It also has the highest OOD accuracy and AUROC, with the lowest OOD Brier and ECE in this comparison.
2. **Balanced gives the best mathematical ECE and the least concentrated confidence distribution among the high-performing variants.** Its AUROC and Brier are close to Selective-CorrectConf.
3. **CalibSFT is competitive but does not improve over Selective-CorrectConf on the aggregate metrics.**
4. **Full supervision preserves answer accuracy but degrades confidence ranking and calibration.**
5. **CE-only confidence supervision causes a large answer-accuracy drop.**
6. **The Base model has competitive answer accuracy but substantially worse aggregate calibration than the selected SFT variants.**

## Current selection

- Primary SFT checkpoint for subsequent RL: **Selective-CorrectConf, step 180**.
- Diversity/ECE ablation reference: **Balanced, step 180**.
- `DROP` should be rerun with the new model-by-dataset scheduler before using a five-dataset OOD average.

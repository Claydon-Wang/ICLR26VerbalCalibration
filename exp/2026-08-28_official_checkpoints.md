# Official Checkpoints

用于论文主实验的 DeepScaleR checkpoint 暂定如下。

## Base → DCPO

```text
logs/train/deep_scale_r/dcpo/qwen3_8b/probability/2026-0821-072022/global_step_160/actor/huggingface
```

- Initialization: `Qwen/Qwen3-8B`
- Training steps: 160
- Seed: 43
- KL: `kl_loss_coef=0`
- DAPO group filtering: enabled
- `filter_all_correct_samples`: enabled
- `norm_adv_by_std_in_grpo`: enabled
- Optimization rewards: accuracy=1.0, format=1.0

## CalibSFT → DCPO

```text
logs/train/deep_scale_r/calib_sft_dcpo/qwen3_8b/probability/2026-0827-154753/global_step_160/actor/huggingface
```

- Initialization:
  `logs/train/deep_scale_r/calib_sft/qwen3_8b/probability/2026-0826-183152/global_step_180/huggingface`
- CalibSFT seed: 46
- DCPO training steps: 160
- DCPO seed: 43
- KL: `kl_loss_coef=0`
- DAPO group filtering: enabled
- `filter_all_correct_samples`: enabled
- `norm_adv_by_std_in_grpo`: enabled
- Optimization rewards: accuracy=1.0, format=1.0

## Comparison summary

| Setting | ID Accuracy | ID AUROC | ID Brier | ID ECE | OOD Accuracy | OOD AUROC | OOD Brier | OOD ECE |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Base → DCPO | 62.16 | 77.74 | 16.33 | 14.45 | 62.37 | 63.73 | 23.96 | 16.74 |
| CalibSFT → DCPO | **63.18** | **84.97** | **13.40** | **12.45** | 62.32 | **67.57** | **22.37** | **12.96** |

The two checkpoints use the same DCPO configuration; the controlled variable is
the CalibSFT initialization.

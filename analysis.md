# Assignment 2 analysis

## Part 1 YOLOv8 before and after fine-tuning (40 points)
### 1B. YOLOv8 fine-tuning on Crack-Seg dataset

| Training setting | Value |
|---|---|
| Initial checkpoint | `yolov8n-seg.pt` |
| Epochs | 5 |
| Optimizer | TBD |
| Learning rate | TBD |
| Batch size | TBD |
| Image size | TBD |
| Weight decay | TBD |
| Other settings | TBD |
| Selected checkpoint / epoch | TBD |

| Epoch | Training box loss | Training segmentation loss | Training class loss | Validation box AP50 | Validation box mAP | Validation mask AP50 | Validation mask mAP |
|---:|---:|---:|---:|---:|---:|---:|---:|
| 1 | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| 2 | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| 3 | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| 4 | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| 5 | TBD | TBD | TBD | TBD | TBD | TBD | TBD |

**Quantitative comparison (test split)**: YOLOv8 performance comparison before and after fine-tuning on **test split**

| Metric | Unit | Before fine-tuning | After fine-tuning | Absolute change |
|---|---|---:|---:|---:|
| Box AP50   | % | TBD | TBD | TBD |
| Box mAP    | % | TBD | TBD | TBD |
| Mask AP50  | % | TBD | TBD | TBD |
| Mask mAP   | % | TBD | TBD | TBD |
| Latency    | ms/frame | TBD | TBD | TBD |
| Throughput | FPS | TBD | TBD | TBD |
| Peak GPU memory | MB | TBD | TBD | TBD |

**Qualitative comparison (test split)**: For each test image, create a side-by-side visual comparison showing the predictions before fine-tuning on the left and after fine-tuning on the right.
| Figure | Test image ID | Figure path | Category | Main observation |
|---:|---|---|---|---|
| 1 | TBD | TBD | Improved example | TBD |
| 2 | TBD | TBD | Improved example | TBD |
| 3 | TBD | TBD | Remaining failure | TBD |
| 4 | TBD | TBD | Representative / difficult case | TBD |

## Part 2 - SAM 1, SAM 2.1, and SAM 3.1 (55 points)

### 2A&B - SAM 1 and SAM 2.1 image performance using visual prompts
**Quantitative comparison (test split of crack-seg dataset)**

| Model | Checkpoint | Prompt type | Box AP50 | Box mAP | Mask AP50 | Mask mAP | Latency (ms/image) | Throughput (images/s) | Peak memory (MB) |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|
| SAM 1 | ViT-B | Positive point | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| SAM 2.1 | Hiera-Tiny | Positive point | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| SAM 1 | ViT-B | Positive + negative points | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| SAM 2.1 | Hiera-Tiny | Positive + negative points | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| SAM 1 | ViT-B | Ground-truth box | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| SAM 2.1 | Hiera-Tiny | Ground-truth box | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| SAM 3.1 | -          | text      | TBD | TBD | TBD | TBD | TBD | TBD | TBD |

### 2C SAM 3.1 Video segmentation performance

**Qualitative comparison** only 

| Video ID | Video name | Object name| Result path | Main observation |
|---:|---|---|---|---|
| 1 | `bear` | `"bear"` | TBD | TBD |
| 2 | `blackswan` | `"black swan"` | TBD | TBD |
| 3 | `dogs` | `"black dog"`  | TBD | TBD |
| 3 | `dogs` | `"yellow dog"` | TBD | TBD |
| 4 | `judo` | `"left player"`  | TBD | TBD |
| 4 | `judo` | `"right player"` | TBD | TBD |
| 5 | `swing` | `"girl"` | TBD | TBD |
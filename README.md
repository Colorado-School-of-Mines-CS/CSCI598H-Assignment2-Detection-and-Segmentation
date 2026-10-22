# CSCI 598H Assignment 2: Detection and Segmentation

## Goal

Complete two tasks on the [Crack-Seg dataset](https://docs.ultralytics.com/datasets/segment/crack-seg/):

1. compare YOLOv8 instance segmentation before and after fine-tuning;
2. compare pretrained SAM 1, SAM 2.1, and SAM 3.1 on images and video.

Crack-Seg contains 3,717 training, 200 validation, and 112 test images. Keep
the official splits unchanged. Do not use test annotations for training or
model selection.

## Part 1 — YOLOv8 before and after fine-tuning 

There are several useful resources:

- [Ultralytics GitHub](https://github.com/ultralytics/ultralytics)
- [YOLOv8 documentation](https://docs.ultralytics.com/models/yolov8/)
- [Instance-segmentation guide](https://docs.ultralytics.com/tasks/segment/)
- [Crack-Seg source](https://universe.roboflow.com/university-bswxt/crack-bphdr)
- [Crack-Seg download](https://github.com/ultralytics/assets/releases/download/v0.0.0/crack-seg.zip)
- [STMicroelectronics YOLOv8n-seg export reference](https://huggingface.co/STMicroelectronics/yolov8n_seg)

Please implement two required experimental settings below and compare their perforamce. Note: Use the same image size, confidence threshold, IoU threshold, batch size,
precision, device, and timing procedure for both runs. Do not tune thresholds on the test set. Exclude model loading and five warm-up iterations from timing. 

### 1A. Image segmentation on [Crack-Seg](https://docs.ultralytics.com/datasets/segment/crack-seg/) test set using [`yolov8n-seg.pt`](https://github.com/ultralytics/assets/releases/download/v8.3.0/yolov8n-seg.pt) (before fine-tuning) 

evaluate the untouched [`yolov8n-seg.pt`](https://github.com/ultralytics/assets/releases/download/v8.3.0/yolov8n-seg.pt) on the [Crack-Seg](https://docs.ultralytics.com/datasets/segment/crack-seg/) test split. Since COCO has no `crack` class, remap all retained predictions to `crack` and report this as a class-agnostic baseline.

### 1B. Image segmentation on [Crack-Seg](https://docs.ultralytics.com/datasets/segment/crack-seg/) test set after fine-tuning 

load the COCO-pretrained
[`yolov8n-seg.pt`](https://github.com/ultralytics/assets/releases/download/v8.3.0/yolov8n-seg.pt) checkpoint first and then fine-tune on the training split of [Crack-Seg dataset](https://docs.ultralytics.com/datasets/segment/crack-seg/), select the best checkpoint using the validation split, and evaluate it on the same test split. Fine-tune for **5 epochs**. You may tune and select the other training hyperparameters yourself. You are encouraged to use [Weights&Biases](https://wandb.ai/site/) to monitor the training process.

**Performance comparison.** Please report:

- **Quantitative comparison**: Present a table comparing the models before and after fine-tuning. Report [box AP50 and mAP (i.e. mean of box AP50 - AP95), mask AP50 and mAP (i.e. mean of mask AP50 - AP95)](https://docs.ultralytics.com/tasks/segment#val), latency in ms/frame, throughput in frames/second (FPS), and peak GPU memory. Include absolute and relative changes for each metric, with units clearly specified.

- **Qualitative comparison**: Select four representative images from the test set and show the ground truth alongside paired before/after predictions for each image. Include at least one improved example and one remaining failure. Briefly describe the changes in detection and segmentation quality and any errors that remain. Note: save the segmentation results by overlaying the mask on the original image with opacity, e.g., `data/videos/swing_first_frame_mask/00000.png`.

Quick setup check:

```bash
python -m pip install ultralytics
yolo segment train model=yolov8n-seg.pt data=crack-seg.yaml epochs=1 imgsz=640
```

Implement this part in `yolov8_experiments/`, run both YOLO entries in
`experiments.json`, and save all results in `outputs/`.

## Part 2 — SAM 1, SAM 2.1, and SAM 3.1 

Use pretrained weights only. Do not submit checkpoints.

| Model | Key distinction | Code and examples | Required checkpoint |
|---|---|---|---|
| [SAM 1](https://github.com/facebookresearch/segment-anything) | Image segmentation using visual prompts: points, boxes, or masks. | [image prompts](https://github.com/facebookresearch/segment-anything/blob/main/notebooks/predictor_example.ipynb) · [automatic masks](https://github.com/facebookresearch/segment-anything/blob/main/notebooks/automatic_mask_generator_example.ipynb) | [ViT-B](https://dl.fbaipublicfiles.com/segment_anything/sam_vit_b_01ec64.pth) |
| [SAM 2.1](https://github.com/facebookresearch/sam2) | Extends visual prompting to video segmentation and tracking through temporal memory. | [image example](https://github.com/facebookresearch/sam2/blob/main/notebooks/image_predictor_example.ipynb) · [video example](https://github.com/facebookresearch/sam2/blob/main/notebooks/video_predictor_example.ipynb) | [Hiera-Tiny](https://dl.fbaipublicfiles.com/segment_anything_2/092824/sam2.1_hiera_tiny.pt) |
| [SAM 3.1](https://github.com/facebookresearch/sam3) | Adds text and exemplar prompts for concept segmentation; improves video efficiency through joint multi-object tracking. | [examples](https://github.com/facebookresearch/sam3/tree/main/examples) | [Hugging Face checkpoint](https://huggingface.co/facebook/sam3.1) |

### 2A. Image segmentation using visual prompts with [SAM 1](https://github.com/facebookresearch/segment-anything) and [SAM 2.1](https://github.com/facebookresearch/sam2) 

Evaluate SAM 1 and SAM 2.1 on all 112 Crack-Seg test images using three separate
prompt settings:

- one deterministic positive point per ground-truth instance;
- one deterministic positive plus one negative point
- one deterministic ground-truth box per instance.

**Visual prompt preparation**: First, generate the three visual prompt settings deterministically from the ground-truth masks. Then write code to batch-process all 112 test images with SAM 1 and SAM 2.1 under each setting. 

**Performance comparison**: If multiple masks are returned, select the one with the highest predicted quality score, not ground-truth overlap. For each prompt type, report the aforementioned quantitative and qualitative comparisons in Part 1.

### 2B. Image Segmentation Using Text Prompts with [SAM 3.1](https://github.com/facebookresearch/sam3)
Evaluate [SAM 3.1](https://github.com/facebookresearch/sam3) on all 112 Crack-Seg test images using the fixed text prompt "crack". Do not provide ground-truth points, boxes, or masks as input, or tune the prompt on the test set.

**Performance comparison**: For text prompts, report the aforementioned quantitative and qualitative comparisons in Part 1.

### 2C. Video Segmentation Using Text and Visual Prompts with [SAM 3.1](https://github.com/facebookresearch/sam3)

Downlaod five videos from the **[Google Drive](https://drive.google.com/drive/folders/1lToM9FGguVAGjKaqz-dnilxmBvfPQoVI?usp=drive_link)** and put them in the dir `data/videos`:

| Video ID| Video name  | Fixed text prompts |  Visual prompts in the first video frame|
|---|---|---|---|
| 1 | `bear`      | `"bear"`       | `00000_bear.png`|
| 2 | `blackswan` | `"black swan"` | `00000_blackswan.png`|
| 3 | `dogs`      | `"yellow dog"` `"black dog"` | `00000_yellow_dog.png` `00000_black_dog.png`|
| 4 | `judo`      | `"right player"` `"left player"` | `00000_right_player.png` `00000_left_player.png`|
| 5 | `swing`     | `"girl"`  `"swing"` `"black cord held in hand"`| `00000_girl.png` `00000_swing.png` `00000_black_cord_held_in_hand.png`|

Evaluate [SAM 3.1](https://github.com/facebookresearch/sam3) using two separate settings:

- **Visual prompts:** Taking the **first-frame** GT mask as visual prompt, then propagate through the video.
- **Text prompts:** Provide the fixed text prompt on the first frame, then segment and track the matching objects throughout the video.

**Performance comparison**: Save the video segmentation results by overlaying the mask on the original image with opacity, e.g., `data/videos/swing_first_frame_mask/00000.png`.

Implement Part 2 in `sam_experiments/`, run SAM entries in
`experiments.json`, and save all results in `outputs/`.

## Submission

Submit:

- code implementation in `yolov8_experiments/` and `sam_experiments/`, 
- Visual result comparison in `outputs/` 
- Evaluation pipeline in `evaluate.py` and `visualize.py`;
- `experiments.json` and `results.csv` records intermediate experimental results, including training loss, validation metrics, and test performance after each epoch;
- completed `analysis.md`;

Do not submit datasets, checkpoints, caches, API keys, or access tokens.

## Grading

| Component | Points |
|---|---:|
| Part 1A | 20 |
| Part 1B | 20 |
| Part 2A | 20 |
| Part 2B | 20 |
| Part 2C | 20 |
| **Total** | **100** |

Follow the course policy on collaboration and AI tools.
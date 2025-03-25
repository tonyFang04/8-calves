# 8-Calves Dataset

[![arXiv](https://img.shields.io/badge/arXiv-2503.13777-b31b1b.svg)](https://arxiv.org/abs/2503.13777)

A benchmark dataset for occlusion-rich object detection, identity classification, and multi-object tracking. Features 8 Holstein Friesian calves with unique coat patterns in a 1-hour video with temporal annotations.

---

## Overview
This dataset provides:
- 🕒 **1-hour video** (67,760 frames @20 fps, 600x800 resolution)
- 🎯 **537,908 verified bounding boxes** with calf identities (1-8)
- 🖼️ **900 hand-labeled static frames** for detection tasks
- Designed to evaluate robustness in occlusion handling, identity preservation, and temporal consistency.

---

## Key Features
- **Temporal Richness**: 1-hour continuous recording (vs. 10-minute benchmarks like 3D-POP)
- **High-Quality Labels**:
  - Generated via **ByteTrack + YOLOv8m** pipeline with manual correction
  - <0.56% annotation error rate
- **Unique Challenges**: Motion blur, pose variation, and frequent occlusions
- **Efficiency Testing**: Compare lightweight (e.g., YOLOv9t) vs. large models (e.g., ConvNextV2)

---

## Dataset Structure
Download the dataset:  
[📥 OneDrive Link](https://uob-my.sharepoint.com/:u:/g/personal/xf16910_bristol_ac_uk/EUUfvrF4Qr5Or61xn0hoA7EB8cU7W9aZ1-8NuJ_po7cwrQ?e=56hFC6)

**ZIP Contents**:

upload/
├── hand_labelled_frames/ # 900 manually annotated frames
│ ├── images/ # Raw frames (600x800)
│ └── labels/ # YOLO labels (class=0 for cows)
│
├── pmfeed_4_3_16.avi # 1-hour video (4th March 2016)
└── pmfeed_4_3_16_bboxes_and_labels.pkl # Temporal annotations

### Annotation Details
**PKL File Columns**:
| Column | Description |
|--------|-------------|
| `class` | Always `0` (cow detection) |
| `x`, `y`, `w`, `h` | YOLO-format bounding boxes |
| `conf` | Ignore (detections manually verified) |
| `tracklet_id` | Calf identity (1-8) |
| `frame_id` | Temporal index matching video |

**Load annotations**:
```python
import pandas as pd
df = pd.read_pickle("pmfeed_4_3_16_bboxes_and_labels.pkl")
```

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


<img src="dataset_screenshot.png" alt="Dataset Example Frame" width="50%" /> 
*Example frame with bounding boxes (green) and calf identities. Challenges include occlusion, motion blur, and pose variation.*

---

## Key Features
- **Temporal Richness**: 1-hour continuous recording (vs. 10-minute benchmarks like 3D-POP)
- **High-Quality Labels**:
  - Generated via **ByteTrack + YOLOv8m** pipeline with manual correction
  - <0.56% annotation error rate
- **Unique Challenges**: Motion blur, pose variation, and frequent occlusions
- **Efficiency Testing**: Compare lightweight (e.g., YOLOv9t) vs. large models (e.g., ConvNextV2)

---

## Dataset Download
The **8-Calves Dataset** is available on Hugging Face 🤗.  
You can access it here: [https://huggingface.co/datasets/tonyFang04/8-calves](https://huggingface.co/datasets/tonyFang04/8-calves).

## Dataset Structure

hand_labelled_frames/ # 900 manually annotated frames and labels in YOLO format, class=0 for cows

pmfeed_4_3_16.avi # 1-hour video (4th March 2016)

pmfeed_4_3_16_bboxes_and_labels.pkl # Temporal annotations


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

## Usage
### Object Detection
- **Training/Validation**: Use the first 600 frames from `hand_labelled_frames/` (chronological split).  
- **Testing**: Evaluate on the full video (`pmfeed_4_3_16.avi`) using the provided PKL annotations.  
- ⚠️ **Avoid Data Leakage**: Do not use all 900 frames for training - they are temporally linked to the test video.  

**Recommended Split**:  
| Split      | Frames | Purpose          |  
|------------|--------|------------------|  
| Training   | 500    | Model training   |  
| Validation | 100    | Hyperparameter tuning |  
| Test       | 67,760 | Final evaluation |  

### Identity Classification
- Use `tracklet_id` (1-8) from the PKL file as labels.  
- **Temporal Split**: 30% train / 30% val / 40% test (chronological order).  

## Key Results

### Object Detection (YOLO Models)
| Model       | Parameters (M) | mAP50:95 (%) | Inference Speed (ms/sample) |
|-------------|----------------|--------------|-----------------------------|
| **YOLOv9c** | 25.6           | **68.4**     | 2.8                         |
| YOLOv8x     | 68.2           | 68.2         | 4.4                         |
| YOLOv10n    | 2.8            | 64.6         | 0.7                         |

---

### Identity Classification (Top Models)
| Model          | Accuracy (%) | KNN Top-1 (%) | Parameters (M) |
|----------------|--------------|---------------|----------------|
| **ConvNextV2-Nano** | 73.1      | 50.8          | 15.6           |
| Swin-Tiny      | 68.7         | 43.9          | 28.3           |
| ResNet50       | 63.7         | 38.3          | 25.6           |

---

**Notes**:
- **mAP50:95**: Mean Average Precision at IoU thresholds 0.5–0.95.  
- **KNN Top-1**: Nearest-neighbor accuracy using embeddings.  
- Full results and methodology: [arXiv paper](https://arxiv.org/abs/2503.13777). 


## License
This dataset is released under [CC-BY 4.0](https://creativecommons.org/licenses/by/4.0/).  
*Modifications/redistribution must include attribution.*

## Citation
```bibtex
@article{fang20248calves,
  title={8-Calves: A Benchmark for Object Detection and Identity Classification in Occlusion-Rich Environments},
  author={Fang, Xuyang and Hannuna, Sion and Campbell, Neill},
  journal={arXiv preprint arXiv:2503.13777},
  year={2024}
}
```

## Contact
**Dataset Maintainer**:  
Xuyang Fang  
Email: [xf16910@bristol.ac.uk](mailto:xf16910@bristol.ac.uk)  

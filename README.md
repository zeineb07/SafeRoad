# SafeRoad Dataset 🚶

<div align="center">

![SafeRoad Dataset](https://img.shields.io/badge/Dataset-SafeRoad-1e3a5f?style=for-the-badge)
![Images](https://img.shields.io/badge/Images-5%2C636-2d7dd2?style=for-the-badge)
![Annotations](https://img.shields.io/badge/Annotations-7%2C800-2d7dd2?style=for-the-badge)
![License](https://img.shields.io/badge/License-CC%20BY--NC%204.0-green?style=for-the-badge)

**A Multi-Orientation Pedestrian Detection and Classification Dataset for Moroccan Road Safety**

[🌐 Website](https://zeineb07.github.io/SafeRoad-dataset/) • [📄 Paper](#citation) • [📬 Request Access](https://forms.gle/xE8cQns236PRGJZp7)

</div>

---

## 📋 Overview

SafeRoad is a pedestrian detection and orientation classification dataset collected from real-world urban environments in Morocco. It provides bounding box annotations with **8-class body orientation labels**, making it the first publicly available pedestrian dataset to jointly provide detection and orientation classification in both YOLO and COCO JSON formats.

### Key Features

- **5,636 images** captured under real-world daytime urban conditions
- **7,800 bounding boxes** annotated with 8 orientation classes
- **8-class orientation taxonomy**: Front, Back, Left, Right, Left-Front, Left-Back, Right-Front, Right-Back
- **Two annotation formats**: YOLO (YOLOv8) and COCO JSON (Faster R-CNN, DETR)
- Collected from **several Moroccan cities** with structured and unstructured environments

---

## 🏷️ Orientation Classes

| Class | Angle Range | Instances |
|-------|-------------|-----------|
| Back | 157.5° – 202.5° | 2,472 |
| Front | −22.5° – 22.5° | 1,477 |
| Left | 247.5° – 292.5° | 1,146 |
| Right | 67.5° – 112.5° | 1,100 |
| Right-Back | 112.5° – 157.5° | 647 |
| Left-Back | 202.5° – 247.5° | 505 |
| Left-Front | 292.5° – 337.5° | 252 |
| Right-Front | 22.5° – 67.5° | 201 |
| **Total** | | **7,800** |

---

## 📊 Dataset Statistics

### YOLO Format — Train/Val Split (80/20)

| Split | Images | Bounding Boxes | Percentage |
|-------|--------|----------------|------------|
| Train | 4,508 | 6,233 | 80% |
| Validation | 1,128 | 1,567 | 20% |
| **Total** | **5,636** | **7,800** | **100%** |

### COCO Format — Train/Val/Test Split (80/10/10)

| Split | Images | Percentage |
|-------|--------|------------|
| Train | 4,508 | 80% |
| Validation | 564 | 10% |
| Test | 564 | 10% |
| **Total** | **5,636** | **100%** |

---

## 📁 Repository Structure

This repository contains **annotations only**. Images are available upon request (see [Access](#-access--download) section below).

```
SafeRoad-Dataset/
├── annotations/
│   ├── yolo/
│   │   ├── labels/
│   │   │   ├── train/          ← YOLO .txt files for training
│   │   │   └── val/            ← YOLO .txt files for validation
│   │   └── dataset.yaml        ← YOLOv8 configuration file
│   └── coco/
│       ├── train/
│       │   └── instances_train.json
│       ├── val/
│       │   └── instances_val.json
│       └── test/
│           └── instances_test.json
├── examples/
│   └── sample_annotation.jpg   ← Sample annotated image
└── README.md
```

---

## 📂 Annotation Formats

### YOLO Format

Each image has a corresponding `.txt` file with one line per bounding box:

```
<class_id> <x_center> <y_center> <width> <height>
```

All coordinates are **normalized to [0, 1]** relative to image dimensions.

**Class mapping:**

```yaml
# dataset.yaml
names:
  0: Front
  1: Right-Front
  2: Right
  3: Right-Back
  4: Back
  5: Left-Back
  6: Left
  7: Left-Front
```

**Example annotation:**
```
0 0.5123 0.4312 0.0874 0.2156   # Front
4 0.3241 0.5621 0.0654 0.1987   # Back
```

### COCO JSON Format

Annotations follow the standard [Microsoft COCO schema](https://cocodataset.org/#format-data). Each split has its own JSON file:

```json
{
  "images": [...],
  "annotations": [
    {
      "id": 1,
      "image_id": 1,
      "category_id": 0,
      "bbox": [x_min, y_min, width, height],
      "area": ...,
      "iscrowd": 0
    }
  ],
  "categories": [
    {"id": 0, "name": "front"},
    {"id": 1, "name": "right-front"},
    {"id": 2, "name": "right"},
    {"id": 3, "name": "right-back"},
    {"id": 4, "name": "back"},
    {"id": 5, "name": "left-back"},
    {"id": 6, "name": "left"},
    {"id": 7, "name": "left-front"}
  ]
}
```

---

## 🚀 Usage

### YOLOv8

```python
from ultralytics import YOLO

# Train on SafeRoad
model = YOLO('yolov8n.pt')
model.train(data='annotations/yolo/dataset.yaml', epochs=100)

# Evaluate
model.val()
```

### Faster R-CNN / DETR (COCO format)

```python
import json

# Load annotations
with open('annotations/coco/train/instances_train.json') as f:
    coco_train = json.load(f)

print(f"Images: {len(coco_train['images'])}")
print(f"Annotations: {len(coco_train['annotations'])}")
print(f"Categories: {[c['name'] for c in coco_train['categories']]}")
```

---

## 📈 Benchmark Results

Models trained and evaluated on the SafeRoad dataset:

| Model | mAP@0.5 | mAP@0.5:0.95 | Precision | Recall |
|-------|---------|--------------|-----------|--------|
| **Faster R-CNN** | **80.73%** | **58.50%** | **80.73%** | 70.63% |
| YOLOv8 | 78.30% | 52.60% | — | **76.20%** |
| DETR | 67.40% | 46.90% | — | 66.70% |

---

## 🔒 Access & Download

### Annotations (Free)
Annotations in both YOLO and COCO JSON formats are freely available in this repository.

### Images (Request Required)
Images are available upon request to ensure proper citation of the dataset.

**To request access:**
1. Fill out the [Access Request Form](https://forms.gle/xE8cQns236PRGJZp7)
2. You will receive the download link via email within **24 hours**

> ⚠️ By requesting access, you agree to cite the SafeRoad dataset in any publication or project resulting from its use.

---

## 📜 License

This dataset is licensed under [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/).

- ✅ Free for academic and research use
- ✅ Must cite the dataset (see Citation below)
- ❌ Commercial use not permitted
- ❌ Redistribution without permission not permitted

---

## 📖 Citation

If you use the SafeRoad dataset in your research, please cite:

```bibtex
@article{SafeRoad2025,
  author    = {Vall Kheir, Zeinebou and Dafrallah, Safaa and Amine, Aouatif},
  title     = {SafeRoad: A Multi-Orientation Pedestrian Detection and 
               Classification Dataset for Moroccan Road Safety},
  journal   = {IEEE Transactions on Intelligent Transportation Systems},
  year      = {2025},
  note      = {Under review}
}
```

---

## 📬 Contact

For questions, access requests, or collaborations:

- 📧 Email: [saferoad.dataset@gmail.com](mailto:saferoad.dataset@gmail.com)
- 🌐 Website: [https://zeineb07.github.io/SafeRoad-dataset/](https://zeineb07.github.io/SafeRoad-dataset/)
- 📄 Paper: [Read Paper](https://zeineb07.github.io/SafeRoad-dataset/assets/paper/saferoad-paper.pdf)

---

<div align="center">
<b>ENSA Kenitra — Ibn Tofail University</b><br>
Supported by NARSA & Centre National pour la Recherche Scientifique et Technique
</div>

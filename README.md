# PCB Defect Detection Using Deep Learning
### Multi-Defect Detection | Defect Localization | Automated PCB Inspection

An AI-powered PCB inspection project that explores multiple deep learning approaches to identify defects in Printed Circuit Boards (PCBs).

The project focuses on moving beyond simple defective/non-defective classification towards detailed visual inspection — identifying multiple defects in a single PCB image, locating their positions, and predicting their respective defect categories.

---

## 1. Problem Statement

Printed Circuit Boards are essential components in electronic systems. Even small manufacturing defects can affect the reliability and functionality of electronic devices.

Manual inspection can become time-consuming and challenging, particularly when:
- Multiple defects occur on the same PCB.
- Defects occupy very small image regions.
- Different defect categories appear together.
- Inspectors need to identify the exact location of each defect.

The objective of this project is to develop a deep learning-based inspection approach that can detect multiple PCB defects and visually localize them within an input image.

---

## 2. Approaches Explored

Rather than directly selecting one architecture, different deep learning approaches were explored to understand their suitability for PCB inspection.

### Approach 1: Image Classification Using MobileNetV3

**Concept:**

A classification-based approach was explored using MobileNetV3 to learn visual features and predict PCB anomaly categories.

**Advantages:**
- Lightweight architecture.
- Efficient feature extraction.
- Suitable for image-level classification.
- Lower computational requirements compared with many larger architectures.

**Limitations for the target problem:**
- Image-level classification does not inherently identify the exact location of a defect.
- Multiple defects in different regions are difficult to represent through a single image-level prediction.
- Does not naturally provide individual bounding boxes for each defective region.
- Requires additional processing or a different architecture to provide detailed localization.

**Observation:**

Although classification is useful for identifying image categories, the project required more detailed information about individual defects and their positions.

### Approach 2: YOLO-Based Detection

**Concept:**

YOLO-based object detection was explored to identify defect categories and their spatial locations directly from PCB images.

**Advantages:**
- Detects multiple objects in a single image.
- Provides bounding-box localization.
- Predicts individual defect categories.
- Produces confidence scores for detections.
- Supports end-to-end object detection.

**Considerations:**
- Requires accurately annotated bounding-box datasets.
- Small and visually subtle defects can be challenging to detect.
- Performance depends on dataset quality, class distribution, image resolution, and training configuration.
- Requires careful experimentation with augmentation and model settings.

**Observation:**

Object detection aligns more directly with the project's primary requirement: identifying multiple defective regions and showing exactly where they occur.

### Approach 3: YOLO-to-Classification Pipeline

**Concept:**

A separate YOLO-to-classification workflow was explored, combining object detection and classification-oriented processing.

**Potential advantages:**
- Allows detected regions to be processed separately.
- Provides flexibility for specialized classification.
- Can support a modular detection-and-classification workflow.

**Limitations:**
- Introduces multiple processing stages.
- Depends on the quality of the initial detection or region extraction.
- Errors in the detection stage can affect subsequent classification.
- Requires additional integration and inference management.

**Observation:**

A multi-stage approach offers flexibility, but adds complexity when the primary objective can be addressed through direct object detection.

---

## 3. Why YOLO-Based Object Detection Was Selected

After exploring classification-based and detection-oriented approaches, YOLO-based object detection was selected as the primary approach for the PCB inspection objective.

The key reason was the requirement to obtain more than a single image-level prediction.

The final approach focuses on:

| Requirement | YOLO-Based Detection |
|---|---|
| Multiple defects in one image | Supported |
| Individual defect localization | Bounding boxes |
| Defect category identification | Class predictions |
| Detection confidence | Confidence scores |
| Single-image inspection | Multiple detections per inference |

### Key Design Decision

The project prioritizes **instance-level defect detection rather than only image-level anomaly classification**.

For example, when a PCB image contains multiple defective regions, the object detection model can produce separate predictions for those regions, instead of returning only one overall class for the entire image.

This makes the output more informative for visual inspection and potential quality-control workflows.

---

## 4. Key Features

### Multiple Defect Detection
Identifies multiple defective regions within a single PCB image during one inference.

### Defect Localization
Draws bounding boxes around detected defects, indicating their approximate positions within the PCB image.

### Defect Category Prediction
Associates each detected region with a predicted defect category.

### Confidence Scores
Provides a confidence score for each individual detection.

### Data Augmentation Experiments
Explores augmentation-based training to improve robustness to variations in PCB image appearance.

### Multiple Dataset Experiments
Includes experiments using DeepPCB and PKU-Market dataset configurations.

### Classification Experiments
Includes separate MobileNetV3 and YOLO-to-classification notebooks for exploring classification-based alternatives.

---

## 5. Overall Workflow

```text
       PCB Input Image
              |
              v
       Image Preprocessing
              |
              v
       Trained YOLO Model
              |
              v
      Feature Extraction
              |
              v
       Object Detection
              |
       +------+------+
       |      |      |
       v      v      v
    Defect  Defect  Defect
    Region  Region  Region
       1      2      3
       |      |      |
       +------+------+
              |
              v
     Bounding Box Localization
              |
              v
     Defect Category Prediction
              |
              v
       Confidence Scores
              |
              v
      Annotated PCB Image
```

---

## 6. Technologies and Tools

| Technology | Application |
|---|---|
| Python | Core implementation |
| YOLO | Object detection |
| PyTorch | Deep learning framework |
| Ultralytics | YOLO training and inference |
| MobileNetV3 | Classification experiments |
| OpenCV | Image processing |
| NumPy | Numerical operations |
| Pandas | Dataset processing |
| Jupyter Notebook | Model development and experiments |

---

## 7. Datasets and Experiments

The project includes experiments involving:

- DeepPCB dataset.
- PKU-Market dataset.
- YOLO dataset configuration files.
- Data augmentation and training experiments.
- Classification dataset preparation.

Dataset configuration files are included in the repository. The actual datasets and large training artifacts are excluded to keep the repository manageable.

Users should obtain the datasets separately and configure the appropriate paths before executing the notebooks.

---

## 8. Repository Contents

The repository contains Jupyter notebooks for:

- PCB defect detection using YOLO.
- DeepPCB training experiments.
- Augmentation-based training.
- PKU-Market dataset experiments.
- MobileNetV3 classification.
- YOLO-to-classification experiments.

It also includes the corresponding dataset configuration files and selected pretrained model weights.

---

## 9. Getting Started

### Clone the repository

```bash
git clone https://github.com/gangotri-141/PCB_Defect_Detection.git
cd PCB_Defect_Detection
```

### Install dependencies

```bash
pip install numpy pandas matplotlib opencv-python
pip install torch torchvision ultralytics
pip install jupyter
```

### Launch Jupyter Notebook

```bash
jupyter notebook
```

Open the relevant notebook, configure the dataset paths, and execute the cells in sequence.

---

## 10. Limitations and Future Improvements

The current project focuses on model development and experimentation. Further improvements can include:

- Improving detection of very small PCB defects.
- Reducing false positives and missed detections.
- Evaluating performance across different PCB types.
- Adding defect severity estimation.
- Developing a real-time PCB inspection interface.
- Deploying the trained model on edge AI hardware.
- Integrating inspection results into an automated quality-control dashboard.

---

## 11. Project Outcome

This project explores multiple deep learning approaches for PCB inspection and selects object detection as the primary direction because it directly addresses the need for multi-defect identification and spatial localization.

The central idea is to move from simply asking:

**"Is this PCB defective?"**

Towards answering:

**"What defects are present, how many are detected, and where are they located?"**

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

⭐ If you find this project useful, consider giving the repository a star!

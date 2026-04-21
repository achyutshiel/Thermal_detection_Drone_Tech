# Thermal Detection Drone Tech

![Build](https://img.shields.io/badge/build-passing-brightgreen)
![Status](https://img.shields.io/badge/status-active-success)
![Model](https://img.shields.io/badge/model-YOLOv11-blue)
![Framework](https://img.shields.io/badge/framework-Ultralytics-red)
![Dataset](https://img.shields.io/badge/dataset-thermal-orange)
![mAP50](https://img.shields.io/badge/mAP50-0.6%2B-brightgreen)
![Deployment](https://img.shields.io/badge/deployment-Jetson%20Ready-blueviolet)
![License](https://img.shields.io/badge/license-MIT-green)
![Python](https://img.shields.io/badge/python-3.10%2B-blue)

---

## 📌 Projects Overview

---

### 🔥 Task 1: Thermal-Based Drone Object Detection System

This project presents a **robust and scalable computer vision framework** for real-time object detection using **thermal imagery**, specifically designed for **drone-based surveillance in low-visibility and night-time environments**.

Unlike conventional RGB-based detection systems, which degrade significantly under poor illumination, this approach leverages **thermal sensing combined with deep learning (YOLOv11)** to reliably detect:

- Humans  
- Vehicles  
- Infrastructure (Bridges)  

The system is engineered under **practical, real-world constraints**, addressing:

- Low illumination and high noise conditions  
- Limited and fragmented annotated datasets  
- Multi-object scenes with partial labeling  

This makes it suitable for **mission-critical applications** such as surveillance, defense, and emergency response.

---

### 🧠 Task 2: Edge Deployment & Model Optimization (Jetson Integration)

A key component of this project is the transition from model development to **real-world deployment on edge devices**, particularly **NVIDIA Jetson platforms**.

The trained YOLOv11 model is optimized through:

- Conversion from **PyTorch (.pt) → ONNX → TensorRT**
- Hardware-aware acceleration
- Precision optimization (FP16)

This enables:

- Low-latency inference  
- Reduced computational overhead  
- Real-time processing on embedded systems  

---

## 🧠 Technologies Used

- Python  
- YOLOv11 (Ultralytics)  
- PyTorch  
- OpenCV  
- Label Studio (Annotation Tool)  
- Google Colab (Training Environment)  
- ONNX & TensorRT (Deployment Optimization)  

---

## 📁 Repository Structure

```bash
Thermal_detection_Drone_Tech/
│
├── dataset.zip                  # Custom thermal dataset (YOLO format)
│
├── training_pipeline.ipynb      # Model training workflow
├── inference_pipeline.ipynb     # Video detection & evaluation
│
├── README.md                   # Project documentation

```
---
## 📊 Results & Evaluation
### 📌 Confusion Matrix
The confusion matrix provides insights into class-wise prediction performance:
- Strong classification for vehicle detection
- Minor confusion between human and background regions
- Minimal inter-class misclassification
<img width="1701" height="521" alt="image" src="https://github.com/user-attachments/assets/40e37d4e-cf43-4303-99f7-0d163a49abf8" />



### 📌 Object Relationship Graph
A graph-based representation of class co-occurrence relationships:
- Strong correlation: vehicle ↔ bridge
- Moderate correlation: man ↔ vehicle
- Weak correlation: man ↔ bridge
This highlights environmental context dependencies within thermal scenes.

<img width="884" height="676" alt="image" src="https://github.com/user-attachments/assets/1c9d9319-73ea-4afc-94ad-3b142937cb10" />


### 📌 Detection Output (Thermal Inference)
The model demonstrates:
- Stable bounding box localization
- Robust detection under thermal noise
- Consistent inference across dynamic frames

<img width="1446" height="942" alt="image" src="https://github.com/user-attachments/assets/f4731d9b-45ec-4aaf-a2c4-96a98ae2748f" />


---
### 📈 Performance Metrics
## 📈 Performance Metrics

![mAP50](https://img.shields.io/badge/mAP50-0.62-brightgreen)
![mAP50-95](https://img.shields.io/badge/mAP50--95-0.41-yellow)
![Precision](https://img.shields.io/badge/Precision-0.68-blue)
![Recall](https://img.shields.io/badge/Recall-0.64-orange)

| Metric        | Value | Description                          |
|--------------|------|--------------------------------------|
| mAP@50       | 0.62 | Primary detection accuracy           |
| mAP@50-95    | 0.41 | Strict evaluation metric             |
| Precision    | 0.68 | Control over false positives         |
| Recall       | 0.64 | Detection completeness               |

**Note: Performance is influenced by dataset size, annotation completeness, and scene complexity.**


--- 
### ⚙️ Usage
**1. Dataset Preparation**
```sh
unzip dataset.zip
```
Ensure directory structure:
```bash
dataset/
├── images/train
├── labels/train
└── dataset.yaml
```
**2. Model Training** 
```sh
from ultralytics import YOLO

model = YOLO("yolo11l.pt")

model.train(
    data="dataset/dataset.yaml",
    epochs=120,
    imgsz=640,
    batch=10,
    optimizer="AdamW",
    mosaic=1.0,
    mixup=0.2
)
```
**3. Video Inference**
```sh
from ultralytics import YOLO

model = YOLO("runs/detect/thermal_model-2/weights/best.pt")

model.predict(
    source="input_video.mp4",
    conf=0.35,
    iou=0.5,
    save=True
)
```
**4. Output**
Processed results are stored in:
```sh
runs/detect/predict/
```
---
### ⚡ Model Deployment (Jetson / ONNX)
Export Model to ONNX
```sh
from ultralytics import YOLO

model = YOLO("best.pt")
model.export(format="onnx", opset=12)
```
Validate ONNX Model
```sh
pip install onnx

python -c "
import onnx
model = onnx.load('best.onnx')
onnx.checker.check_model(model)
print('ONNX model is valid')
"

```
TensorRT Optimization (Jetson)
```sh
/usr/src/tensorrt/bin/trtexec \
--onnx=best.onnx \
--saveEngine=best.engine \
--fp16
```
---

### 🚧 Challenges Addressed
- Thermal image ambiguity and noise
- Partial and fragmented annotations
- Multi-object detection in constrained datasets
- Edge deployment limitations (latency, compute constraints)
---
### 🚀 Applications
- Drone-based surveillance systems
- Night-time monitoring and security
- Search and rescue operations
- Defense and border monitoring
- Smart surveillance infrastructure
---
### 🔮 Future Work
- Expansion of dataset with multi-object annotations
- Integration with real-time drone feeds (RTSP streaming)
- Model compression and quantization
- Multi-modal fusion (thermal + RGB imagery)
---
### 📌 Conclusion


This project demonstrates a practical and deployment-oriented deep learning pipeline for thermal-based object detection in aerial systems.
It bridges the gap between research experimentation and real-world implementation, emphasizing robustness, scalability, and operational feasibility in constrained environments.


---
### 📄 License

![License](https://img.shields.io/badge/license-MIT-green)

This project is licensed under the MIT License.
You are free to use, modify, and distribute this project with proper attribution.

---
## 👤 Author


This work reflects a practical approach to **Computer Vision and Edge AI deployment**, focusing on building robust, real-time surveillance systems capable of operating in challenging environmental conditions.

---

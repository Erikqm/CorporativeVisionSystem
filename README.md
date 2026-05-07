#  Vision System Platform

> **Note:** This repository was created to document a real-world project built during my professional experience. The source code is confidential and cannot be shared for ethical reasons.

---

##  Overview

An enterprise-grade, on-premise computer vision platform built to **centralize and serve AI inference across an entire manufacturing company**. The system hosts 70+ models, receives images from multiple source types, runs inference, and returns structured results — acting as the AI backbone for quality inspection across multiple assembly lines.

Built and scaled by a 2-person team over 3 years. Currently active in production.

---

##  Problem

As computer vision use cases grew across the factory floor, each application managed its own model independently — leading to duplicated infrastructure, inconsistent configurations, and no centralized way to manage or monitor AI models across the company.

---

##  Solution

A centralized inference server that:
- **Hosts all models** across the company in one place
- **Exposes a REST API** so any system can request inference by simply sending an image
- **Manages model parameters** (IoU, confidence thresholds, classes, etc.) through a frontend interface
- **Supports multiple vision techniques** beyond object detection
- **Runs entirely on-premise**, keeping sensitive manufacturing data within the company's infrastructure

---

##  End-to-End MLOps Pipeline

Every new use case follows a structured pipeline from request to production:

```
 Ticket opened by internal client
        │
        ▼
 Client captures photos of the use case
        │
        ▼
 Manual annotation of images (bounding boxes, classes)
        │
        ▼
 Model trained on dedicated GPU training server
        │
        ▼
 Model converted to ONNX and registered in the platform
        │
        ▼
 Parameters configured via frontend (IoU, confidence, classes, thresholds)
        │
        ▼
 Model live in production — accessible via REST API
```

---

##  Inference Data Flow

```
Client system (tablet, IP camera, industrial camera)
        │
        │  POST /inference
        │  { image, model_id }
        ▼
Vision Platform Server
        │
        ├─► Loads model parameters from database
        │   (IoU, confidence, classes, thresholds)
        │
        ├─► Runs inference using the appropriate technique
        │
        └─► Returns structured result
                │
                ▼
        Client system processes and acts on result
        Result logged for monitoring dashboard
```

---

##  Supported Vision Techniques

| Technique | Use Case Examples |
|---|---|
| **Object Detection** (YOLO) | Component presence, defect detection, part identification |
| **OCR** (tesseract) | Reading serial numbers, labels, and codes |
| **Blob Counting** | Counting small parts, holes, or surface features |
| **Color Filter** | Color-based validation and classification |
| **Basic Thermal Camera Integration** | Temperature-based inspection |

---

## 📷 Supported Image Sources

The platform is flexible on how images arrive — clients integrate however fits their workstation:

- 📱 **Tablets** — operators capture and send photos manually
- 📷 **IP Cameras** — registered and streamed directly to the platform
- 🏭 **Industrial Cameras** — integrated for high-precision

---

##  Key Technical Decisions

**Dedicated GPU training server**
Model training runs on a separate server equipped with a GPU, keeping the inference server lean and resource-efficient. Each model is trained, validated, and converted to ONNX before being deployed to production.

**ONNX for cross-framework portability**
All models are converted to ONNX format before deployment, allowing models trained in different frameworks (yolov4 and yolov11) to run on the same inference engine — with optimized CPU performance on the production server.

**Centralized parameter management**
Model parameters are stored in a database and managed through a frontend interface.

---

##  Monitoring Dashboard

A dedicated dashboard tracks model health and performance over time:

-  **Model accuracy** — tracked over time to detect drift or degradation
-  **Inference time** — monitored per model to catch performance regressions
-  **Detected items** — historical log of what each model has detected

---

##  Architecture

| Component | Role |
|---|---|
|  On-premise Inference Server (CPU) | Hosts all models and handles inference requests |
|  GPU Training Server | Dedicated environment for model training |
|  REST API | Interface for all client systems |
|  Database | Stores model metadata, parameters, and inference logs |
|  Frontend Dashboard | Model management and performance monitoring |

---

##  Tech Stack

| Technology | Role |
|---|---|
| **C#** | Core platform language |
| **ONNX Runtime** | Inference engine for all models |
| **YOLO (v4 / v11)** | Primary object detection model family |
| **Tesseract OCR Engine** | Text recognition |
| **REST API** | Client communication protocol |

---

##  Scale & Results

| Metric | Value |
|---|---|
|  Models in production | **70+** |
|  Factories covered | **4** |
|  Workstations served | **~70** |
|  Daily inferences | **~1,000** |
|  Years in production | **3+** |

---

##  Team

Developed by a 2-person team. The platform was initiated by one developer and taken over, completed, and scaled to its current state by me — including expanding from initial deployment to 70+ models across 4 factories, and owning the full pipeline from client request to production deployment.

---

*This platform demonstrates that production-grade AI infrastructure doesn't require cloud dependency or large teams — with the right architecture, a small team can build, operate, and continuously expand a scalable vision system serving an entire enterprise.*

# VisionGuard

### *Vision-Based Industrial Quality Inspection System*

> A vision-based industrial quality inspection system that performs real-time object detection, defect inspection, PASS/FAIL classification, and production monitoring using YOLOv11, OpenCV, and an interactive dashboard.

---

## Overview

VisionGuard is a vision-based industrial quality inspection system developed to automate defect detection in manufacturing environments. The system captures images from a camera, processes them using OpenCV, and performs object detection with YOLOv11. Detected products are inspected for defects and classified as PASS or FAIL. Inspection results are displayed on a live dashboard with production statistics and PLC simulation, demonstrating an Industry 4.0 quality control workflow.

---

## Features

- Real-time object detection
- Automatic defect inspection
- PASS / FAIL quality classification
- Live production dashboard
- PLC simulation
- Inspection data logging

---

## System Architecture

> Replace this section with your architecture diagram.

```text
                     VISIONGUARD

                     PRODUCTION LINE
                           │
                           ▼

              Camera Module (USB/IP Camera)
                           │
                           ▼

                     OpenCV Processing
                           │
                           ▼

                  YOLOv11 Object Detection
                           │
            ┌──────────────┴──────────────┐
            ▼                             ▼

   Object Classification          Defect Inspection

            └──────────────┬──────────────┘
                           ▼

                 PASS / FAIL Decision
                           │
          ┌────────────────┼────────────────┐
          ▼                ▼                ▼

     Dashboard      PLC Simulation    Data Logging
```

---

## Workflow

```text
Production Line
      │
      ▼
Camera Capture
      │
      ▼
OpenCV Processing
      │
      ▼
YOLOv11 Detection
      │
      ▼
Defect Inspection
      │
      ▼
PASS / FAIL Decision
      │
      ▼
Dashboard & PLC Simulation
      │
      ▼
Inspection Data Logging
```

---

## Technologies Used

| Category | Technologies |
|----------|--------------|
| Programming | Python, VSCode|
| Computer Vision | OpenCV, YOLOv11 |
| GUI | PySide6 |
| Dataset | Roboflow |
| Hardware | ESP32, Servo Motor, IR Sensors, LEDs, Buzzer |

---

## Hardware Components

| Component | Purpose |
|-----------|---------|
| PCCamera | Image acquisition |
| ESP32 | Industrial controller |
| IR Sensors | Product detection |
| Servo Motor | Reject mechanism |
| LEDs | Status indication |
| Buzzer | Defect alert |

---

## Project Structure

```text
VisionGuard/

├── app/
│   ├── main.py
│   ├── detector.py
│   ├── camera.py
│   └── utils.py
│
├── dashboard/
│   ├── dashboard.py
│   ├── simulation.py
│   └── analytics.py
│
├── hardware/
│   ├── esp32_code.ino
│   ├── wiring.png
│   └── circuit_diagram.png
│
├── model/
│   ├── best.pt
│   └── classes.txt
│
├── screenshots/
│   ├── architecture.png
│   ├── dashboard.png
│   ├── detection.png
│   └── simulation.png
│
├── demo/
│   └── demo.mp4
│
├── requirements.txt
├── README.md
└── LICENSE
```

---

## Installation

Clone the repository

```bash
git clone https://github.com/your-username/VisionGuard.git
```

Navigate to the project directory

```bash
cd VisionGuard
```

Install the required dependencies

```bash
pip install -r requirements.txt
```

Run the application

```bash
python main.py
```

---

## Screenshots

### System Architecture

> Add `architecture.png`

### Dashboard

> Add `dashboard.png`

### Detection Results

> Add `detection.png`

### PLC Simulation

> Add `simulation.png`

---

## Results

- Detects industrial objects in real time.
- Identifies defective products using YOLOv11.
- Performs automated PASS / FAIL classification.
- Displays live production statistics through the dashboard.
- Demonstrates an industrial quality inspection workflow with PLC simulation.

---

## Applications

- Packaging Industries
- Food & Beverage Manufacturing
- Smart Manufacturing
- Industry 4.0 Automation
- Automated Quality Inspection

---

## Future Enhancements

- PLC integration
- Multi-camera inspection
- Cloud analytics dashboard
- Automated report generation

---

## Author

**Suhani RM**

Electronics and Communication Engineering

---

## License

This project is licensed under the MIT License.

# Dual-Stream Industrial Vision & ESP32 PLC Gateway

An end-to-end industrial quality control system featuring a **parallel dual-stream processing architecture**.

The system captures optical frames via a webcam using **OpenCV and Roboflow Cloud AI**, while simultaneously processing physical line inputs using an **ESP32 microcontroller** (IR sensors, LED indicators, rejection servo motor). Both parallel streams feed real-time telemetry directly into a **Streamlit SCADA Command Center** and a **PLC Simulator**.

---

## 🛠 Parallel System Architecture

```text
                               ┌─────────────────────────┐
                               │    OBJECT DETECTION     │
                               │ (Physical Product Line) │
                               └────────────┬────────────┘
                                            │
                   ┌────────────────────────┴────────────────────────┐
                   │                                                 │
                   ▼                                                 ▼
      [ PARALLEL STREAM 1: VISION ]                    [ PARALLEL STREAM 2: ESP32 ]
                   │                                                 │
                   ▼                                                 ▼
      ┌──────────────────────────┐                      ┌──────────────────────────┐
      │ Webcam Detects Object    │                      │ IR Sensors & Hardware    │
      └────────────┬─────────────┘                      └────────────┬─────────────┘
                   │                                                 │
                   ▼                                                 ▼
      ┌──────────────────────────┐                      ┌──────────────────────────┐
      │ OpenCV / Roboflow AI     │                      │ ESP32 Hardware Unit      │
      │ Processes Products       │                      │ Actuates Servo & LEDs    │
      └────────────┬─────────────┘                      └────────────┬─────────────┘
                   │                                                 │
                   │                                                 ▼
                   │                                    ┌──────────────────────────┐
                   │                                    │ Serial Monitor           │
                   │                                    │ (serial_monitor.py)      │
                   │                                    └────────────┬─────────────┘
                   │                                                 │
                   ├─────────────────────────────────────────────────┤
                   │                                                 │
                   ▼                                                 ▼
      ┌──────────────────────────┐                      ┌──────────────────────────┐
      │      PLC SIMULATOR       │                      │   STREAMLIT DASHBOARD    │
      │  (Monitors Vision &      │                      │ (Consolidates Vision &   │
      │   ESP32 Telemetry)       │                      │   Hardware Telemetry)    │
      └──────────────────────────┘                      └──────────────────────────┘
```

---

## How the Parallel Flow Operates

1. **Stream 1: Vision Pipeline **

   * Webcam detects physical items entering the optical field.
   * OpenCV performs real-time contour geometry filtering to detect holes while Roboflow AI runs asynchronous cloud defect classification.
   * Frame buffers and vision metrics are pushed directly to the **Streamlit Dashboard** and **PLC Simulator**.

2. **Stream 2: Hardware Pipeline **

   * Simultaneously, physical IR sensors on the ESP32 detect item presence and hole alignment.
   * The ESP32 triggers physical Pass/Fail LEDs and actuates a Servo Motor for hardware rejection.
   * Captures raw JSON strings over Serial USB and routes hardware telemetry into the system data sink (`live_inspection_data.json`).

3. **Convergence **

   * **Streamlit Dashboard :** Consolidates live video frames with ESP32 hardware counts and line status in real time.
   * **PLC Simulator :** Operates alongside the pipeline, tracking logic execution states and timer delays across both vision and hardware signals.

---

## 📁 File Responsibilities Matrix

| File Name               | Core Function                                                                                                                                |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **`cloud_vision_engine_4.py`**    | Captures webcam feed, runs OpenCV geometry analysis + Roboflow AI, updates frame buffer and feeds visual data to dashboard & PLC simulator. |
| **`esp32_main.ino`**    | Reads physical entry/hole IR sensors, actuates the rejection servo motor, controls RGB LED indicators and outputs JSON metrics over Serial. |
| **`serial_inspection.py`** | Listens on USB Serial port, processes ESP32 telemetry, and syncs updates to `live_inspection_data.json`.                                     |
| **`plc_simulator.py`**  | Simulates industrial PLC ladder logic scan cycles using combined inputs from the vision script and the ESP32 hardware stream.                |
| **`dashboard_2.py`**      | Industrial Command Center UI. Renders live video, KPI metric cards, line status alerts and production analytics.                            |

---

## 🔌 Hardware Wiring Diagram

### ESP32 Pin Mapping

| Component               | ESP32 Pin | Mode / Configuration           | Function                                           |
| ----------------------- | --------- | ------------------------------ | -------------------------------------------------- |
| **IR Sensor 1 (Entry)** | `GPIO 23` | Digital Input (`INPUT_PULLUP`) | Detects when an item enters the inspection zone.   |
| **IR Sensor 2 (Hole)**  | `GPIO 22` | Digital Input (`INPUT_PULLUP`) | Aligned to inspect for physical holes/slots.       |
| **Servo Motor Signal**  | `GPIO 14` | PWM Output                     | Actuates rejection gate (0° = Pass, 90° = Reject). |
| **Green LED**           | `GPIO 26` | Digital Output                 | Line PASS indicator.                               |
| **Red LED**             | `GPIO 27` | Digital Output                 | Line DEFECT / FAULT alert.                         |
| **Blue LED**            | `GPIO 25` | Digital Output                 |System ready / inspection active.                   |
| **Blue LED**            | `GPIO 25` | Digital Output                 |System ready / inspection active.                   |


---

## 🚀 Installation & Execution Guide

### 1. Requirements & Dependencies

Install required Python packages:

```bash
pip install opencv-python requests numpy pandas plotly streamlit pyserial
```

### 2. Flashing ESP32 Firmware

1. Open `esp32_main.ino` in Arduino IDE.
2. Install the **ESP32Servo** library (`Tools` → `Manage Libraries...`).
3. Select your ESP32 board, select the COM port, and click **Upload**.

### 3. Running the Parallel Ecosystem

Launch each module in a separate terminal window inside the project directory:

**Terminal 1 — Vision Stream**

```bash
python cloud_vision_engine_4.py
```

**Terminal 2 — ESP32 Serial Bridge**

```bash
python serial_inspection.py
```

**Terminal 3 — PLC Simulator**

```bash
python plc_simulator.py
```

**Terminal 4 — Streamlit Command Dashboard**

```bash
streamlit run dashboard.py
```


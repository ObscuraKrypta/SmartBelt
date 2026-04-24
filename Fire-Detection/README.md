# SmartBelt Fire & Overheating Detection System

##  Overview

This project presents a **real-time fire and overheating detection system** for conveyor belt environments using a **thermal camera**, **Raspberry Pi**, **buzzer alarm**, **Telegram notifications**, and optional **PLC shutdown control via OPC UA**.

The system was designed for conveyor belt systems where hot or ferrous materials may accidentally enter the belt area. In this setup, a magnetic drum under the conveyor can generate heat during operation, and ferrous or overheated materials can create a risk of smoke, fire, or equipment damage.

To reduce this risk, the system continuously monitors the belt area with an **MLX90640 thermal camera** and triggers safety actions when the measured temperature exceeds a defined threshold.

---

##  Main Idea

The system continuously checks the maximum temperature in the camera frame.

If the temperature is higher than the configured threshold:

1. A thermal image is captured and saved.
2. A Telegram alert is sent to technicians.
3. The thermal image is sent through Telegram.
4. A buzzer alarm is activated.
5. A shutdown signal can be sent to a PLC using OPC UA.

---

##  System Workflow

```text
Thermal Camera
      ↓
Temperature Frame Capture
      ↓
Maximum Temperature Detection
      ↓
Threshold Check
      ↓
If Temperature >= Threshold
      ↓
Save Thermal Image + Send Telegram Alert + Activate Buzzer + PLC Shutdown Signal
```

---

##  Features

* Real-time thermal monitoring
* Overheating and fire-risk detection
* MLX90640 thermal camera support
* Telegram alert system with warning message and image
* Buzzer alarm via Raspberry Pi GPIO
* Optional PLC shutdown command using OPC UA
* Automatic thermal image saving
* Lightweight and suitable for embedded deployment

---

##  Project Structure

```text
Fire-Detection/
│
├── Figures/                     # Project figures and diagrams
├── Results/                     # Output images and test results
├── Setup/                       # Setup images or configuration files
├── dataset/                     # Thermal/fire-related data
│
└── Fire_detection_module.ipynb  # Main fire detection notebook
```

---

##  Detection Logic

The project uses the **maximum temperature** from each thermal frame.

In the current implementation:

```python
TEMPERATURE_THRESHOLD = 30.0
```

This means that if the detected maximum temperature is **30°C or higher**, the system triggers the alert and safety response.

You can modify this value depending on the industrial environment:

```python
TEMPERATURE_THRESHOLD = 50.0
```

Recommended threshold examples:

* 30°C → testing / laboratory validation
* 40–50°C → early warning
* 60°C+ → stronger fire-risk alarm, depending on material and environment

---

##  Hardware Requirements

### Required Hardware

* Raspberry Pi
* MLX90640 thermal camera
* Buzzer
* Jumper wires
* Conveyor belt system
* Power supply

### Optional Hardware

* PLC or industrial controller
* Magnetic drum system
* Relay module
* Emergency shutdown circuit

---

##  Software Requirements

* Python 3.8+
* Raspberry Pi OS
* OpenCV
* NumPy
* Adafruit MLX90640 library
* Telegram Bot API
* OPC UA Python client
* RPi.GPIO

---

##  Installation

Install the required Python libraries:

```bash
pip install numpy opencv-python adafruit-circuitpython-mlx90640 python-telegram-bot opcua RPi.GPIO
```

On Raspberry Pi, also make sure I2C is enabled:

```bash
sudo raspi-config
```

Then go to:

```text
Interface Options → I2C → Enable
```

---

##  Hardware Setup

### MLX90640 Thermal Camera

The MLX90640 uses I2C communication.

Typical Raspberry Pi wiring:

```text
MLX90640 VCC  →  3.3V
MLX90640 GND  →  GND
MLX90640 SDA  →  GPIO2 / SDA
MLX90640 SCL  →  GPIO3 / SCL
```

### Buzzer

The current code uses:

```python
BUZZER_PIN = 18
```

Typical wiring:

```text
Buzzer +  →  GPIO18
Buzzer -  →  GND
```

Use a transistor or driver circuit if your buzzer requires more current than the Raspberry Pi GPIO can safely provide.

---

##  Telegram Alert Setup

Create a Telegram bot using **BotFather**, then replace these values in the code:

```python
TELEGRAM_BOT_TOKEN = "YOUR_BOT_TOKEN"
CHAT_ID = "YOUR_CHAT_ID"
```

When overheating is detected, the bot sends:

* A fire/temperature warning message
* The measured temperature
* A captured thermal image

Example alert:

```text
Fire Alert: High temperature detected!
Temperature: 45.23 °C
```

---

##  PLC / OPC UA Integration

The project includes optional PLC shutdown integration using OPC UA.

Current configuration:

```python
PLC_URL = "opc.tcp://localhost:4840"
NODE_ID = "ns=2;s=OPC_Daten.Anlage_ausschalten"
```

When the threshold is exceeded, the code sends:

```python
node.set_value(True)
```

This can be used to trigger a machine stop, emergency shutdown, or alarm condition in an industrial control system.

Before deployment, update:

```python
PLC_URL
NODE_ID
```

according to your PLC and OPC UA server configuration.

---

##  How to Run

Clone the repository:

```bash
git clone https://github.com/ObscuraKrypta/Fire-Detection.git
cd Fire-Detection
```

Install requirements:

```bash
pip install numpy opencv-python adafruit-circuitpython-mlx90640 python-telegram-bot opcua RPi.GPIO
```

Run the notebook:

```bash
jupyter notebook Fire_detection_module.ipynb
```

Or convert the logic into a Python script and run:

```bash
python fire_detection_module.py
```

Stop the monitoring loop with:

```text
CTRL + C
```

---

##  Thermal Image Output

When an alert is triggered, the system saves a thermal image using this format:

```text
thermal_alert_YYYYMMDD_HHMMSS.jpg
```

The image is normalized and resized for easier viewing:

```python
resized_image = cv2.resize(image, (320, 240), interpolation=cv2.INTER_LINEAR)
```

---

##  Safety Notes

This project is a prototype for smart conveyor belt safety monitoring.

Before using it in a real industrial environment:

* Calibrate the temperature threshold carefully
* Test the MLX90640 readings under laberatory operating conditions
* Add electrical protection for GPIO-connected components
* Avoid exposing the Raspberry Pi directly to dust, vibration, or heat

---

##  Testing Recommendations

Test the system step by step:

1. Confirm the MLX90640 camera returns temperature frames.
2. Print the maximum temperature.
3. Save a test thermal image.
4. Test Telegram message sending.
5. Test buzzer activation.
6. Run the full monitoring loop.

---

##  Customization

You can customize:

### Temperature Threshold

```python
TEMPERATURE_THRESHOLD = 30.0
```

### Camera Refresh Rate

```python
mlx.refresh_rate = MLX90640.RefreshRate.4_HZ
```

### Buzzer Duration

```python
trigger_buzzer(duration=5)
```

### Alert Delay

```python
time.sleep(30)
```

This delay prevents repeated Telegram alerts for the same overheating event.

---

##  Results

The system can detect abnormal heating conditions in real time and trigger a multi-level safety response:

* Visual thermal evidence
* Remote technician notification
* Local acoustic alarm

This makes it suitable for early-stage fire prevention in conveyor belt and recycling environments.

---


##  Contact

For questions, collaboration, or dataset/project details:

* [shohreh.kia@tu-clausthal.de](mailto:shohreh.kia@tu-clausthal.de)
* [shohreh.kia77@gmail.com](mailto:shohreh.kia77@gmail.com)

---

 ## Acknowledgments:

 Special thanks to Professor Daniel Goldmann, Professor Benjamin Leiding and the IFAD Institute for their cooperation and support in providing access to the eddy current machine. We also sincerely appreciate the valuable assistance of technicians Jean-Marie Dornbusch and Olaf Tschenscher.
---

##  License

This project is open-source. Please contact the author for industrial usage or collaboration details.

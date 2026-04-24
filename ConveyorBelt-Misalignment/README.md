# Smart Conveyor Belt Misalignment Detection & Correction System

## Overview

This project presents a **real-time conveyor belt misalignment detection and correction system** using **computer vision (OpenCV)** and **embedded control (Raspberry Pi + stepper motors)**.

In industrial environments, conveyor belts may drift to the **left or right**, causing inefficiency or potential damage.
This system automatically:

1. **Detects belt edges using vision**
2. **Identifies misalignment**
3. **Corrects the position using stepper motors**

---

##  How It Works

###  Detection Pipeline

* A **Raspberry Pi camera** is mounted above the conveyor belt
* White edge lines are placed on both sides of the belt
* The system uses:

  * Grayscale conversion
  * Gaussian blur
  * Canny edge detection
  * Hough Line Transform

to detect **left and right belt edges**

---

###  Decision Logic

*  Both lines detected → Belt is centered → Do nothing
*  Right line missing → Belt shifted right → Move belt left
*  Left line missing → Belt shifted left → Move belt right

---

###  Actuation System

* Two **stepper motors** are installed:

  * Left motor
  * Right motor
* Controlled via **GPIO pins**
* Automatically activated based on misalignment detection

---

##  Features

* Real-time misalignment detection
* Automatic correction using stepper motors
* Lightweight and deployable on Raspberry Pi
* Robust edge detection using OpenCV
* Works in real industrial scenarios

---

##  Project Structure

```id="6mnjrv"
ConveyorBelt-Misalignment/
│
├── Dataset/                 # Sample dataset
├── Images/                  # Input images
├── Results/                 # Output results (visualizations/videos)
├── LineDetection.ipynb      # Development & testing notebook
├── final_test.py            # Real-time detection + motor control
└── README.md
```

---

##  Hardware Requirements

### Minimum Setup:

* Raspberry Pi (recommended: Raspberry Pi 4)
* Raspberry Pi Camera Module (or USB camera)

### Additional Components:

* 2 × Stepper Motors
* Motor Driver (e.g., A4988 / L298N)
* Power Supply (suitable for motors)
* Conveyor belt system

---

##  Software Requirements

* Python 3.8+
* Raspberry Pi OS (recommended)
* OpenCV
* NumPy
* Matplotlib
* Picamera2
* RPi.GPIO
* pygame

---

##  Installation

Install dependencies:

```bash id="4ub9m2"
pip install opencv-python numpy matplotlib pygame
```

On Raspberry Pi:

```bash id="4z7wkj"
pip install picamera2 RPi.GPIO
```

---

##  Detection Algorithm

The system uses:

* **Region of Interest (ROI)** to focus on belt area
* **Canny Edge Detection** for edge extraction
* **Hough Transform** for line detection

Example:

```python id="h6kq5q"
canny_image = cv2.Canny(blurred, 170, 240)
lines = cv2.HoughLinesP(canny_image, 1, np.pi/180, threshold=100)
```

---

##  Running the System

### 1. Run Notebook (for testing)

```bash id="n0q1xt"
jupyter notebook LineDetection.ipynb
```

---

### 2. Run Real-Time System

```bash id="r9zq2k"
python final_test.py
```

---

##  Motor Control Logic

* If **right edge is missing** → activate **right motor**
* If **left edge is missing** → activate **left motor**
* Motors stop when both edges are detected again

---

##  Output

* Real-time annotated video
* Detection overlay (left/right status)
* Saved output video (`output.avi`)

---

##  Results

* Reliable detection of belt edges
* Accurate correction in real-time
* Performance depends on:

  * Lighting conditions
  * Camera placement
  * Line visibility

---

##  Important Notes

* Ensure good lighting for accurate detection
* Adjust ROI based on camera position
* Tune Canny thresholds depending on environment
* Motor speed should be calibrated carefully

---

##  Future Improvements

* PID control for smoother correction
* Deep learning-based detection (YOLO)
* Multi-camera system
* Industrial-grade robustness

---

##  Contact

For questions or collaboration:

📧 [shohreh.kia@tu-clausthal.de](mailto:shohreh.kia@tu-clausthal.de)
📧 [shohreh.kia77@gmail.com](mailto:shohreh.kia77@gmail.com)


---

##  License

This project is open-source. Please contact for industrial usage.

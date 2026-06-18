# Multi-Sensor Safety Stick

> An IoT-powered smart navigation and safety assistive device for visually impaired individuals, integrating obstacle detection, fire sensing, GPS tracking, and real-time SOS alerting.

---

## Table of Contents

- [What Is This Project?](#what-is-this-project)
- [Why This Project?](#why-this-project)
- [Problem Statement](#problem-statement)
- [Proposed Solution](#proposed-solution)
- [Hardware Components](#hardware-components)
- [Software & Platform](#software--platform)
- [How Everything Is Used](#how-everything-is-used)
- [Project Type](#project-type)
- [Team & Mentorship](#team--mentorship)
- [Academic Context](#academic-context)

---

## What Is This Project?

The **Multi-Sensor Safety Stick** (originally titled "Smart Stick for Blind Person") is an embedded IoT project that transforms a conventional white cane into an intelligent, multi-modal assistive device. It is designed to dramatically improve the safety and autonomy of visually impaired individuals by detecting physical obstacles, sensing environmental hazards like fire, tracking the user's GPS location, and sending instant SOS alerts to caregivers — all in real time.

The stick integrates six hardware sensors and modules into a single handheld form factor, controlled by an ESP32 microcontroller with onboard Wi-Fi capability. When a hazard is detected, the stick responds through buzzer alerts and vibration haptic feedback, while simultaneously transmitting notifications and GPS coordinates to a caregiver's smartphone via the Pushbullet API.

---

## Why This Project?

According to the World Health Organization, over **2.2 billion people** globally suffer from some form of vision impairment, with approximately 43 million classified as blind. In India alone, millions of visually impaired individuals face daily challenges navigating urban and rural environments, often relying solely on traditional white canes or human assistance.

Traditional white canes suffer from several critical shortcomings:

- They only detect obstacles at ground level and within a short forward range.
- They provide no warning against overhead obstacles or hazards above knee height.
- They cannot detect non-physical hazards such as fire or gas.
- They offer no way to signal for help or share location in an emergency.
- Caregivers have no real-time awareness of the user's location or safety status.

The **Multi-Sensor Safety Stick** directly addresses all of these gaps through sensor fusion and IoT connectivity, providing a holistic safety net that operates passively and autonomously.

---

## Problem Statement

Visually impaired individuals navigate environments with significant risk:

1. **Obstacle Hazard** — Obstacles at chest, waist, or head height go undetected by a standard cane sweeping the ground.
2. **Fire/Heat Hazard** — Open flames, gas leaks, or fire situations cannot be sensed by touch alone before serious harm occurs.
3. **Emergency Helplessness** — In an emergency (fall, disorientation, medical event), the user has no quick way to alert caregivers.
4. **Caregiver Disconnect** — Family members and caregivers have zero visibility into the user's real-time whereabouts.
5. **Limited Range Sensing** — Conventional cane tips only detect immediate ground-level obstacles.

---

## Proposed Solution

The Multi-Sensor Safety Stick proposes a **layered, multi-modal safety system** built into a cane form factor:

### Layer 1 — Proximity & Obstacle Detection
An **HC-SR04 Ultrasonic Sensor** continuously measures the distance to objects in front of the user. When an obstacle is detected within a configurable threshold distance (e.g., 50–100 cm), it immediately triggers the buzzer and vibration motor, giving the user tactile and auditory warnings.

### Layer 2 — Fire & Flame Detection
A **Flame Sensor** (IR-based) constantly scans for open fire or intense heat sources in the environment. Upon detection, the buzzer triggers an emergency tone pattern distinct from the obstacle alert, and a push notification is instantly sent to the caregiver's device via the **Pushbullet API** over Wi-Fi.

### Layer 3 — Emergency SOS System
A physical **SOS push button** is mounted on the stick handle. When pressed by the user, the ESP32 immediately:
- Reads the current GPS coordinates from the NEO-6M module.
- Sends a Pushbullet notification to the registered caregiver with the message "SOS! Help needed!" along with a Google Maps link of the user's current location.

### Layer 4 — Real-Time GPS Tracking
The **NEO-6M GPS Module** continuously receives satellite positioning data. This data is used both reactively (on SOS button press) and can be configured for periodic location updates, letting caregivers monitor the user's movement in near real time through their smartphone.

### Layer 5 — Haptic + Auditory Feedback
A **Vibration Motor** and **Buzzer** act as the primary output channels. Different alert patterns distinguish between obstacle warnings, fire alerts, and SOS confirmations, helping the user understand the nature of the hazard without visual cues.

---

## Hardware Components

| Component | Model | Role |
|---|---|---|
| **Microcontroller** | ESP32 (Dual-Core, Wi-Fi + Bluetooth) | Central processing, sensor orchestration, Wi-Fi communication |
| **Obstacle Sensor** | HC-SR04 Ultrasonic Sensor | Detects objects via time-of-flight echo measurement (range: 2 cm – 400 cm) |
| **Fire Sensor** | IR Flame Detection Sensor | Detects infrared radiation emitted by open flames (wavelength ~760–1100 nm) |
| **GPS Module** | NEO-6M GPS Module (u-blox) | Receives satellite signals; provides latitude/longitude, speed, altitude |
| **Alert Output 1** | Buzzer (Active/Passive) | Audible alerts for obstacles and fire hazards |
| **Alert Output 2** | Vibration Motor (ERM/LRA) | Haptic feedback for silent/tactile alerts |
| **User Input** | SOS Push Button | Triggers emergency location alert to caregiver |

---

## Software & Platform

| Tool / Platform | Purpose |
|---|---|
| **Arduino IDE / ESP-IDF** | Firmware development and flashing for ESP32 |
| **C / C++ (Arduino Framework)** | Programming language for embedded logic |
| **Pushbullet API** | Cloud notification service — sends push notifications with GPS data to caregiver's phone |
| **NMEA Protocol** | Standard protocol used to parse GPS data from the NEO-6M module |
| **Google Maps (URL scheme)** | Generates clickable location links embedded in SOS notifications |
| **Wi-Fi (802.11 b/g/n)** | ESP32's onboard Wi-Fi used to connect to internet for Pushbullet API calls |
| **TinyGPS++ Library** | Arduino library for parsing NMEA sentences from NEO-6M |
| **NewSoftSerial / HardwareSerial** | Serial communication between ESP32 and GPS module |

---

## How Everything Is Used

### ESP32 — The Brain
The ESP32 is the central controller. It runs the firmware loop that:
1. Polls the ultrasonic sensor every ~100ms for distance readings.
2. Monitors the flame sensor's digital output pin for a HIGH (fire detected) signal.
3. Listens for a button press on the SOS GPIO pin.
4. Reads NMEA sentences from the GPS module via UART.
5. On any hazard event, drives the buzzer/vibration motor output and makes an HTTPS POST request to the Pushbullet API.

### Ultrasonic Sensor — Obstacle Detection
The HC-SR04 works by emitting a 40 kHz ultrasound pulse from its TRIG pin and measuring the time for the echo to return on the ECHO pin. The firmware computes: `distance = (echo_duration × 0.034) / 2` (in cm). If `distance < threshold`, it triggers the buzzer and vibration motor.

### Flame Sensor — Fire Detection
The IR flame sensor has a photodiode tuned to the infrared spectrum of fire. Its digital output goes LOW when flames are detected. The ESP32 reads this GPIO pin in every loop cycle. On detection, it triggers a distinct buzzer pattern and sends a "FIRE ALERT!" Pushbullet notification.

### NEO-6M GPS — Location Tracking
The GPS module communicates over UART at 9600 baud, sending NMEA sentences (GPGGA, GPRMC). The TinyGPS++ library parses these to extract latitude and longitude. On an SOS event, the firmware constructs a Google Maps URL: `https://maps.google.com/?q=<lat>,<lon>` and includes it in the Pushbullet notification body.

### Pushbullet API — Caregiver Alerts
Pushbullet is a free push notification service. The ESP32 makes an HTTPS POST request to `https://api.pushbullet.com/v2/pushes` with the API key in the Authorization header and a JSON body containing the notification title and body. The caregiver receives this instantly on their phone/PC.

### Buzzer & Vibration Motor — User Feedback
Different buzzer patterns communicate different alerts:
- Short beep (×2): Obstacle detected nearby.
- Long continuous tone: Fire detected.
- Three rapid beeps: SOS sent successfully.

The vibration motor mirrors these patterns for environments where audio alerts may be inappropriate or insufficient.

---

## Project Type

- **Domain:** Embedded Systems / Internet of Things (IoT)
- **Category:** Assistive Technology / Healthcare Technology
- **Nature:** Hardware + Firmware + Cloud API Integration
- **Form Factor:** Handheld stick / cane device

---

## Team & Mentorship

### Built By

| Roll No. | Name |
|---|---|
| 22CM010 | Naman Sunil Bharsakale |
| 22CM016 | Sarvesh Sham Chaudhari |
| 22CM019 | Yash Sandip Chaukhande |
| 22CM030 | Jay Ajay Ganorkar |
| 23CM206 | Mahammad Shahzad Mohammad Zameer |

### Under the Guidance Of

**Prof. (Mrs.) C. R. Chaudhari**
Department of Computer Engineering
Government Polytechnic Amravati

### Department Head

**Dr. P. P. Karde**
Head, Department of Computer Engineering
Government Polytechnic Amravati

### Principal

**Dr. V. R. Mankar**
Principal, Government Polytechnic Amravati

---

## Academic Context

This project was submitted in partial fulfillment of the **Diploma in Computer Engineering** program during the academic year **2024–2025** at:

**Government Polytechnic Amravati**
*(An Autonomous Institute of the Government of Maharashtra)*

The project fulfills the final-year capstone requirement, demonstrating applied knowledge of embedded systems, sensor integration, IoT communication, and real-world assistive technology design.
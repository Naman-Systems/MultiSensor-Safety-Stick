# Multi-Sensor Safety Stick

> A revolutionary IoT-based assistive device meticulously crafted to enhance the safety and autonomy of visually impaired individuals — integrating object detection, fire sensing, GPS tracking, and real-time SOS alerting into a single smart cane.

---

## Table of Contents

- [What Is This Project?](#what-is-this-project)
- [Why This Project?](#why-this-project)
- [Problem Statement](#problem-statement)
- [Proposed Solution](#proposed-solution)
- [Hardware Components & Tools](#hardware-components--tools)
- [Software & Platform](#software--platform)
- [How Everything Is Used — Detailed](#how-everything-is-used--detailed)
- [Project Type & Features](#project-type--features)
- [Advantages](#advantages)
- [Applications](#applications)
- [Limitations](#limitations)
- [Future Scope](#future-scope)
- [Team & Mentorship](#team--mentorship)
- [Academic Context](#academic-context)

---

## What Is This Project?

The **Multi-Sensor Safety Stick** is an IoT-based assistive device designed to transform the conventional white cane into an intelligent, multi-modal safety companion for visually impaired individuals. It integrates cutting-edge object detection technologies, intuitive feedback systems, and seamless internet connectivity to provide real-time assistance and navigation support.

At its core, the Smart Stick uses ultrasonic sensors to detect obstacles, an IR flame sensor for fire hazard awareness, a NEO-6M GPS module for real-time location tracking, and the Pushbullet API to push instant notifications to caregivers' smartphones. User feedback is delivered through a buzzer (auditory) and vibration motor (haptic), enabling the user to understand hazards without any visual input.

The device is built around the **ESP32 microcontroller**, chosen for its dual-core processing power, built-in Wi-Fi and Bluetooth, and low power consumption — making it ideal for a portable, always-on wearable device.

---

## Why This Project?

Daily routines that many take for granted — visiting a new shop, crossing a street, or enjoying a walk in the park — can be deeply challenging for those who rely on touch and sound to navigate. The traditional white cane can identify immediate obstacles at ground level, but it cannot:

- Detect obstacles at waist or chest height.
- Warn of fire or heat hazards in the environment.
- Share the user's location with a caregiver in real time.
- Send an emergency alert when the user is in distress.

Technology is constantly evolving, and with it comes the opportunity to create innovative tools that make a real difference. The Smart Stick is not just a cane — it's a step forward. It empowers visually impaired individuals with greater confidence, safety, and independence.

---

## Problem Statement

The Smart Stick project addresses several interconnected problems faced by visually impaired individuals:

**1. Limitation of Mobility and Independence**
Individuals with visual impairments face challenges navigating their environments safely and independently due to the limitations of traditional aids. Conventional white canes offer some help but often fall short in providing real-time updates in ever-changing environments.

**2. Limited Real-Time Feedback**
Existing assistive technologies may not provide real-time feedback about surroundings, making it difficult to detect obstacles and hazards in dynamic environments.

**3. Accessibility and Affordability**
Many assistive technologies for visually impaired individuals are prohibitively expensive or inaccessible, limiting adoption among the target population.

**4. Connectivity and Integration**
The lack of connectivity with other devices and systems hinders users' ability to access essential services such as emergency assistance or real-time navigation guidance.

**5. Safety Concerns**
The inability to detect obstacles and hazards in real time poses significant safety concerns, increasing the risk of accidents, injuries, and falls during navigation.

**6. Limited Customization**
Existing assistive technologies may not offer sufficient customization to meet diverse user needs across different environments and situations.

**7. Psychological Impact**
Challenges associated with limited mobility and independence can lead to feelings of frustration, anxiety, and depression, diminishing quality of life.

Addressing these challenges requires a holistic approach combining technological innovation, user-centered design, and accessibility — exactly what the Smart Stick delivers.

---

## Proposed Solution

The proposed Smart Stick system is an advanced, inclusive assistive tool built to overcome these challenges through five integrated layers:

### Obstacle Detection Mechanism
An advanced object detection system using HC-SR04 ultrasonic sensors is strategically positioned along the stick. These sensors continuously scan the surrounding environment, detecting obstacles, hazards, and other objects in the user's path. When an obstacle is detected within threshold range, the system triggers immediate auditory and haptic feedback.

### Real-Time Response System
Upon detecting obstacles or hazards, the feedback system provides users with real-time auditory and tactile cues proportional to the danger. The buzzer emits beeps and the vibration motor pulses — increasing in frequency or intensity as the user approaches closer to an obstacle.

### GPS Tracking System
Equipped with a NEO-6M GPS module, the Smart Stick provides real-time location tracking. Caretakers can view the user's current location through a companion mobile application, facilitating route planning and remote monitoring.

### Emergency Alert System
In an emergency — such as a fall or medical issue — the Smart Stick's built-in emergency alert system automatically notifies designated contacts. Users can activate the emergency alert manually using a dedicated SOS button on the stick. The system uses the Pushbullet API to push instant notifications with GPS coordinates to the caregiver's phone.

### Fire Hazard Detection
A flame sensor continuously monitors the environment for fire signatures. Upon detecting a flame, the system activates a distinct buzzer alert, triggers the vibration motor, and sends a push notification to the caregiver via the Pushbullet API.

---

## Hardware Components & Tools

### ESP32 Microcontroller

The ESP32, developed by Espressif Systems, is the brain of the Smart Stick. As the successor to the ESP8266 series, the ESP32 offers enhanced performance with dual-core processing and integrated support for both Wi-Fi and Bluetooth (Classic and BLE), making it highly versatile for IoT applications.

**Key features used in this project:**
- **Processing Power:** Handles real-time data from all sensors and manages user interaction logic simultaneously on two cores.
- **Wi-Fi Connectivity:** Enables wireless communication with the Pushbullet API for sending fire alerts and SOS notifications over the internet.
- **Multiple GPIO Pins:** Allows easy connection to all sensors (ultrasonic, flame, GPS) and actuators (buzzer, vibration motor, SOS button).
- **Low Power Consumption:** Extends battery life between charges; supports deep sleep modes for power saving.
- **Compact Size:** Fits easily into the smart stick enclosure.

**Considerations:**
- Power usage can be relatively high during active Wi-Fi operations.
- Some applications may need external memory for larger data handling.

### Ultrasonic Sensor (HC-SR04)

The HC-SR04 ultrasonic sensor uses sound waves with frequencies above 20 kHz to detect objects and measure distances — working on the same echolocation principle as bats.

**How it works:**
1. **Transmitter:** Emits 40 kHz ultrasonic waves into the environment.
2. **Propagation:** Sound waves travel through air until hitting an object.
3. **Reflection:** Waves bounce off the object's surface.
4. **Receiver:** Detects the returning echo.
5. **Time-of-Flight Measurement:** Measures the time for the round trip; calculates distance as `distance = (echo_time × 0.034) / 2` cm.

**Uses in this project:** Proximity sensing, obstacle detection, and distance-based alert triggering.

### Pushbullet API

The Pushbullet API is a simple, powerful notification service that allows the ESP32 to send real-time messages, links, and alerts directly to the caregiver's phone or computer over the internet. It is developer-friendly and requires only HTTPS POST requests with a JSON body.

**Uses in this project:**
- Sending fire alert notifications when the flame sensor is triggered.
- Sending SOS notifications with the user's Google Maps GPS link when the emergency button is pressed.
- Cross-device communication between the embedded ESP32 and the caregiver's smartphone, enabling a connected safety ecosystem.

### Buzzer

An active buzzer is an electromechanical device that produces sound when electrical current passes through it. It operates on the principle of electromagnetic induction — current through a coil creates a magnetic field that vibrates a diaphragm, producing sound waves.

**Types used consideration:**
- **Active Buzzer:** Contains an internal oscillator; produces sound when voltage is applied — used for simplicity.
- **Passive Buzzer:** Requires an external AC/DC signal — allows precise control of frequency and duration for custom tones.

**Uses in this project:** Producing distinct auditory alert patterns for obstacle detection, fire hazard, and SOS confirmation.

### NEO-6M GPS Module

The NEO-6M GPS module by U-Blox is a compact, affordable solution for navigation and tracking. It features a high-sensitivity receiver capable of acquiring satellite signals even in challenging environments.

**Key specifications:**
- Supports GPS and GLONASS satellite constellations for improved accuracy.
- Fast Time-To-First-Fix (TTFF) using advanced positioning algorithms.
- Standard UART serial interface at 9600 baud, outputting NMEA sentences.
- Compact design suitable for handheld and wearable devices.

**Uses in this project:** Real-time location tracking; providing latitude/longitude coordinates for SOS notifications; enabling caregivers to monitor the user's position via Google Maps links.

### Vibration Motor

A vibration motor is a small electromechanical device that converts electrical energy into mechanical vibrations. Inside the motor, an eccentric rotating mass (ERM) is attached to the motor shaft. When current flows through the motor coils, the shaft spins rapidly, creating unbalanced forces that produce vibrations.

**Uses in this project:**
- Providing tactile (haptic) feedback to alert the user to obstacles.
- Serving as an alternative alert channel in noisy environments where the buzzer may not be audible.
- Stronger vibration patterns for users with hearing impairments.
- Mirroring buzzer patterns for multi-modal alerts.

### Flame Sensor

A flame sensor detects the presence of fire or open flames by sensing specific wavelengths of light (infrared or ultraviolet) produced by a flame. It uses a photodiode or phototransistor sensitive to the IR/UV spectrum. When a flame is present, the captured light is converted into an electrical signal processed by the ESP32.

**Types:**
- **IR Flame Sensors:** Used in this project; suited for outdoor or varied lighting conditions.
- **UV Flame Sensors:** Better for indoor environments without direct sunlight.

**Uses in this project:** Continuous fire hazard monitoring; triggering immediate alerts to both the user (buzzer + vibration) and caregiver (Pushbullet notification) when a flame is detected.

---

## Software & Platform

| Tool / Platform | Purpose |
|---|---|
| **Arduino IDE** | Primary platform for writing, compiling, and uploading firmware to the ESP32. Supports C/C++ and provides a user-friendly interface for managing projects and libraries. |
| **Embedded C/C++ (Arduino Framework)** | Programming language for all firmware — sensor interfacing, data processing, navigation algorithms, and API calls. |
| **Pushbullet API** | Sends real-time push notifications (fire alerts, SOS + GPS location) to the caregiver's phone via HTTPS POST requests. |
| **TinyGPS++ Library** | Arduino library for parsing NMEA sentences from the NEO-6M GPS module, extracting latitude/longitude. |
| **WiFiClientSecure / HTTPClient** | ESP32 built-in libraries for making secure HTTPS connections to the Pushbullet API over Wi-Fi. |
| **Google Maps URL Scheme** | Constructs clickable location links (`maps.google.com/?q=lat,lon`) embedded in SOS notifications. |
| **MIT App Inventor** | Platform referenced for building the companion mobile application for caregivers. |

---

## How Everything Is Used — Detailed

### ESP32 as the Central Controller
The ESP32 runs a continuous firmware loop. On every iteration it:
1. Triggers the HC-SR04 ultrasonic pulse and reads the echo duration.
2. Reads the digital output of the flame sensor GPIO.
3. Parses incoming NMEA sentences from the GPS module via UART2.
4. Checks whether the SOS interrupt flag has been set by the button ISR.
5. Based on results, drives the buzzer/vibration motor and/or makes HTTPS calls.

### Ultrasonic Sensor in Detail
The ESP32 sends a 10 μs HIGH pulse on the TRIG pin, causing the HC-SR04 to emit 8 ultrasonic pulses. The ECHO pin goes HIGH until the reflected pulse returns. The ESP32 uses `pulseIn()` to measure this duration and computes distance. If distance is below the configured threshold, buzzer and vibration are activated immediately.

### Flame Sensor in Detail
The flame sensor's digital output is LOW when fire is detected, HIGH otherwise. The ESP32 reads this GPIO every loop cycle. On detection: buzzer fires a long continuous tone, vibration motor runs a sustained pulse, and an HTTPS POST fires to the Pushbullet API with a "FIRE DETECTED" notification. A cooldown timer prevents notification spam.

### GPS in Detail
The NEO-6M continuously outputs NMEA sentences over UART at 9600 baud. The TinyGPS++ library parses these character by character to extract `lat` and `lon` float values. These are stored in memory and updated continuously. On any SOS event, the latest coordinates are used to construct a Google Maps URL embedded in the alert notification.

### SOS Button in Detail
The button is connected to a GPIO configured as `INPUT_PULLUP` and uses a hardware interrupt on the falling edge. This ensures the button press is never missed even during busy sensor polling cycles. On press: the ISR sets a flag, the main loop reads GPS coordinates, constructs the alert message with a Maps link, calls Pushbullet API, and confirms with 3 rapid buzzer beeps.

### Pushbullet API in Detail
The ESP32 uses `WiFiClientSecure` for TLS and `HTTPClient` to POST to `https://api.pushbullet.com/v2/pushes` with the caregiver's API key in the Authorization header. The JSON body contains the notification title and message body (including the GPS link for SOS). The caregiver receives this as an instant push notification on their phone.

---

## Project Type & Features

- **Domain:** Embedded Systems / Internet of Things (IoT)
- **Category:** Assistive Technology / Healthcare Technology
- **Nature:** Hardware + Firmware + Cloud API Integration
- **Form Factor:** Handheld cane/stick device

### Full Feature List
- Obstacle detection with real-time auditory and haptic feedback
- Fire hazard detection with instant caregiver push notification
- GPS-based real-time location tracking
- SOS emergency button with location sharing
- Companion mobile application for caregiver monitoring
- Vibration motor for tactile alerts (especially for hearing-impaired users)
- User-friendly ergonomic design with simple controls
- Durable, weather-resistant construction

---

## Advantages

- **Enhanced Safety:** Real-time obstacle and fire hazard detection minimizes accident risk.
- **Greater Mobility Assistance:** Guides users confidently, reducing reliance on external support.
- **Live Location Monitoring:** GPS + Pushbullet lets caregivers track the user remotely.
- **Quick Emergency Assistance:** SOS button ensures timely intervention in critical situations.
- **Easy-to-Use Interface:** Simple controls and companion app make it accessible to all users.
- **Cost-Effective:** Leverages open-source platforms (Arduino, Pushbullet) to keep costs low and accessibility wide.
- **Independence and Empowerment:** Boosts user confidence and autonomy.
- **Increased Awareness via Sensory Feedback:** Vibration motor provides tactile alerts; stronger patterns accommodate hearing-impaired users.

---

## Applications

- **Personal Navigation Companion:** Helps users safely traverse diverse environments.
- **Indoor Wayfinding:** Navigating shopping malls, office buildings, and transport hubs.
- **Educational & Awareness Initiatives:** Training programs on visual impairment and assistive technology.
- **Community Assistance Programs:** Nonprofits can implement it to enhance mobility solutions.
- **Healthcare Facility Navigation:** Navigating hospitals, clinics, and medical centers independently.
- **Caregiver Support Tool:** Enhances safety for visually impaired dependents in unfamiliar settings.

---

## Limitations

- **Complexity in Initial Setup:** Configuration can be challenging for users unfamiliar with assistive technology.
- **Restricted Detection Range:** Ultrasonic sensors have a limited detection cone; hazards outside this range may go undetected.
- **Vulnerability to Water Damage:** Electronic components are sensitive to moisture; waterproofing adds cost and complexity.
- **Ongoing Maintenance Needs:** Requires regular software updates, troubleshooting, and technical support.
- **Learning Curve:** Users may need training to become fully accustomed to the feedback mechanisms.
- **Battery Limitations:** Rechargeable batteries degrade over time and require frequent recharging; power management is essential.
- **Privacy and Data Security:** GPS tracking raises concerns about handling location data; transparent data policies and user consent are necessary.

---

## Future Scope

- **Multisensory Feedback:** Combining vibration, audio, and haptic responses with user-customizable preferences.
- **AI Obstacle Classification:** Identifying and classifying obstacles (poles, benches, moving bicycles) for context-aware navigation.
- **Smart Traffic Interaction:** Receiving signals from traffic lights and crosswalks to alert users when safe to cross.
- **Voice-Controlled Interface:** Managing device functions via simple voice commands.
- **Offline Navigation:** Stored maps and routes for reliable guidance in areas with poor internet connectivity.
- **Advanced Power Management:** Solar-assisted charging, energy-efficient microcontrollers, and fast-charging options.
- **Community-Based Hazard Sharing:** Users reporting and sharing environmental hazards with one another.
- **Biometric Monitoring:** Sensors for stress or heart rate; alerting trusted contacts in times of distress.
- **GSM/4G Module:** For SOS notifications independent of Wi-Fi connectivity.
- **Fall Detection:** Automatic SOS using an accelerometer/IMU.

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
Project Guide, Department of Computer Engineering
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

Submitted in partial fulfillment of the **Diploma in Computer Engineering** at:

**Government Polytechnic Amravati**
*(An Autonomous Institute of the Government of Maharashtra)*

Academic Year: **2024–2025**

---

## References

- Arduino Documentation: https://docs.arduino.cc/learn/starting-guide/the-arduino-software-ide/
- NEO-6M GPS Interfacing: https://circuitdigest.com/microcontroller-projects/interfacing-neo6m-gps-
- ESP8266 Circuit Programming: https://circuitjournal.com/programming-esp8266-over-wifi
- MIT App Inventor: https://appinventor.mit.edu/
- Vibration Motor Usage: https://lab.arts.ac.uk/books/physical-computing/page/using-a-vibration-motor
- Pushbullet API: https://www.pushbullet.com/
- HC-SR04 with ESP8266: https://randomnerdtutorials.com/esp8266-nodemcu-hc-sr04-ultrasonic-arduino/
- ESP8266 Telegram Alerts: https://randomnerdtutorials.com/esp8266-nodemcu-door-status-telegram/
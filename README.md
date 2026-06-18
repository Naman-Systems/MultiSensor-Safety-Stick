# 🦯 Multi-Sensor Safety Stick

> An IoT-powered smart assistive device for the visually impaired — combining obstacle detection, fire sensing, GPS tracking, and real-time SOS alerting into a single handheld cane.

<p align="center">
  <img src="https://img.shields.io/badge/Platform-ESP32-blue?style=for-the-badge&logo=espressif" />
  <img src="https://img.shields.io/badge/Language-C%2FC%2B%2B%20(Arduino)-orange?style=for-the-badge&logo=c%2B%2B" />
  <img src="https://img.shields.io/badge/IoT-Pushbullet%20API-green?style=for-the-badge" />
  <img src="https://img.shields.io/badge/GPS-NEO--6M-lightgrey?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Domain-Assistive%20Technology-purple?style=for-the-badge" />
</p>

---

## 📖 Overview

The **Multi-Sensor Safety Stick** is an embedded IoT project designed to empower visually impaired individuals with a smart, sensor-fused safety device. Unlike a traditional white cane that only detects ground-level obstacles, this stick offers:

- 🚧 **Obstacle detection** at any height using ultrasonic sensing
- 🔥 **Fire / flame detection** with instant caregiver notification
- 📍 **Real-time GPS location** transmitted on SOS
- 📲 **Instant smartphone alerts** to a caregiver via Pushbullet API
- 📳 **Haptic + auditory feedback** to the user through buzzer and vibration motor

---

## ✨ Features

| Feature | Technology | User Benefit |
|---|---|---|
| Obstacle Detection | HC-SR04 Ultrasonic | Warns of obstacles up to 400cm away, any height |
| Fire Alert | IR Flame Sensor | Detects open flames, alerts user + caregiver instantly |
| SOS Button | GPIO Interrupt | Sends location to caregiver with one button press |
| GPS Tracking | NEO-6M + TinyGPS++ | Provides accurate coordinates for SOS notifications |
| Caregiver Alert | Pushbullet API (HTTPS) | Real-time push notifications to caregiver's phone |
| Buzzer Feedback | Tonal patterns | Distinct sounds for obstacle vs. fire vs. SOS confirmation |
| Vibration Feedback | ERM Motor | Tactile alerts that work in noisy environments |

---

## 🛠️ Hardware Requirements

### Components List

| Component | Model | Quantity | Approx. Cost |
|---|---|---|---|
| Microcontroller | ESP32 Development Board | 1 | ₹350–500 |
| Obstacle Sensor | HC-SR04 Ultrasonic Sensor | 1 | ₹50–80 |
| Fire Sensor | IR Flame Detection Sensor Module | 1 | ₹40–60 |
| GPS Module | NEO-6M GPS Module (u-blox) | 1 | ₹350–500 |
| Buzzer | Active Buzzer 5V | 1 | ₹10–20 |
| Vibration Motor | 3V ERM Coin Motor | 1 | ₹30–50 |
| Push Button | Momentary Tactile Switch | 1 | ₹5 |
| Transistor | NPN 2N2222 or BC547 | 2 | ₹5 |
| Diode | 1N4007 (flyback protection) | 1 | ₹2 |
| Battery | Li-Po 3.7V 2000mAh | 1 | ₹300–400 |
| Boost Converter | MT3608 (3.7V → 5V) | 1 | ₹40–60 |
| Charger Module | TP4056 Li-Po Charger | 1 | ₹20–30 |
| Miscellaneous | Jumper wires, PCB, cane body | — | ₹100–150 |

**Estimated Total Cost: ₹1,300 – ₹1,900**

---

## 💻 Software Requirements

| Software / Library | Version | Purpose |
|---|---|---|
| Arduino IDE | 2.x | Firmware development & flashing |
| ESP32 Board Package | ≥3.0 | Arduino core for ESP32 |
| TinyGPS++ | ≥1.0.3 | NMEA GPS sentence parser |
| ArduinoJson | ≥6.x | JSON body construction for API calls |
| HTTPClient | (built-in ESP32) | HTTPS POST to Pushbullet |
| WiFiClientSecure | (built-in ESP32) | TLS/SSL for secure API calls |
| Pushbullet App | Latest | Installed on caregiver's smartphone |

---

## 📌 Pin Configuration

| ESP32 GPIO | Connected To | Mode |
|---|---|---|
| GPIO 5 | HC-SR04 TRIG | OUTPUT |
| GPIO 18 | HC-SR04 ECHO | INPUT |
| GPIO 34 | Flame Sensor OUT | INPUT |
| GPIO 16 (RX2) | NEO-6M TX | UART INPUT |
| GPIO 17 (TX2) | NEO-6M RX | UART OUTPUT |
| GPIO 27 | SOS Push Button | INPUT_PULLUP + INT |
| GPIO 25 | Buzzer (via transistor) | OUTPUT |
| GPIO 26 | Vibration Motor (via transistor) | OUTPUT |

---

## ⚡ Circuit Diagram

```
                         ┌─────────────────────────────────────────┐
                         │              ESP32 DevKit               │
                         │                                         │
     HC-SR04             │  GPIO 5  ──────────────── TRIG          │
   ┌──────────┐          │  GPIO 18 ──────────────── ECHO          │
   │  TRIG ───┼──────────┤                                         │
   │  ECHO ───┼──────────┤                                         │
   │  VCC  ───┼── 3.3V ──┤  3V3 ──────────────── VCC (sensors)    │
   │  GND  ───┼── GND ───┤  GND ──────────────── GND (common)     │
   └──────────┘          │                                         │
                         │                                         │
   Flame Sensor          │  GPIO 34 ──────────────── OUT           │
   ┌──────────┐          │                                         │
   │  OUT  ───┼──────────┤                                         │
   │  VCC  ───┼── 3.3V ──┤                                         │
   │  GND  ───┼── GND ───┤                                         │
   └──────────┘          │                                         │
                         │                                         │
   NEO-6M GPS            │  GPIO 16 (RX2) ─────────── TX (GPS)    │
   ┌──────────┐          │  GPIO 17 (TX2) ─────────── RX (GPS)    │
   │  TX   ───┼──────────┤                                         │
   │  RX   ───┼──────────┤                                         │
   │  VCC  ───┼── 3.3V ──┤                                         │
   │  GND  ───┼── GND ───┤                                         │
   └──────────┘          │                                         │
                         │                                         │
   SOS Button            │  GPIO 27 ──────── Button ──── GND      │
   ┌──────────┐          │  (10kΩ pull-up internal)                │
   │  PIN1 ───┼──────────┤                                         │
   │  PIN2 ───┼── GND ───┤                                         │
   └──────────┘          │                                         │
                         │                                         │
   Buzzer (5V)           │  GPIO 25 ──┐                            │
   ┌──────────┐          │            │                            │
   │  + ─── Collector ◄──┘    [NPN transistor BC547]              │
   │  - ─── GND               Base ── GPIO 25                     │
   └──────────┘               Emitter ── GND                      │
                         │                                         │
   Vibration Motor        │  GPIO 26 ──┐                            │
   ┌──────────┐          │            │                            │
   │  + ─── Collector ◄──┘    [NPN transistor BC547]              │
   │  - ─── GND               Base ── GPIO 26                     │
   └──────────┘               Emitter ── GND                      │
          │                                                        │
        [1N4007 flyback diode across motor terminals]              │
                         │                                         │
                         │  VIN (5V) ──── MT3608 Boost ──── Li-Po │
                         └─────────────────────────────────────────┘

 Power Chain:
   [Li-Po 3.7V] ──► [TP4056 Charger] ──► [MT3608 5V Boost] ──► [ESP32 VIN]
                                                              ──► [Buzzer 5V]
                                                              ──► [Motor 5V]
   ESP32 3V3 output ──► HC-SR04 VCC, Flame Sensor VCC, NEO-6M VCC
```

> **Note:** Always use a transistor driver for the buzzer and vibration motor — driving them directly from ESP32 GPIO pins will damage the chip. The ESP32's GPIO can only source ~12 mA safely; both loads require 100–200+ mA.

---

## 🚀 Getting Started

### 1. Setup Arduino IDE

```bash
# Install ESP32 board package:
# File → Preferences → Additional Board Manager URLs:
https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json

# Tools → Board Manager → Search "esp32" → Install
```

### 2. Install Libraries

```
Tools → Manage Libraries:
- TinyGPS++ by Mikal Hart
- ArduinoJson by Benoit Blanchon
```

### 3. Configure `config.h`

```cpp
// Wi-Fi Credentials
#define WIFI_SSID        "YourWiFiName"
#define WIFI_PASSWORD    "YourWiFiPassword"

// Pushbullet API Key (from pushbullet.com → Account Settings)
#define PUSHBULLET_API_KEY  "o.XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"

// Sensor Thresholds
#define OBSTACLE_THRESHOLD_CM   50    // Alert if obstacle within 50cm
#define FLAME_COOLDOWN_MS       10000 // 10s between fire notifications

// GPIO Pins
#define PIN_TRIG        5
#define PIN_ECHO        18
#define PIN_FLAME       34
#define PIN_SOS_BTN     27
#define PIN_BUZZER      25
#define PIN_VIBRATION   26
```

### 4. Flash the ESP32

1. Connect ESP32 to your PC via USB.
2. Select `Tools → Board → ESP32 Dev Module`.
3. Select the correct COM port.
4. Click Upload (▶).

### 5. Setup Pushbullet on Caregiver's Phone

1. Download the **Pushbullet** app (Android / iOS).
2. Create a free account at [pushbullet.com](https://www.pushbullet.com).
3. Go to **Account Settings → Access Tokens → Create Access Token**.
4. Copy the token into `config.h` as `PUSHBULLET_API_KEY`.
5. Ensure the caregiver's phone allows notifications from Pushbullet.

---

## 📡 How It Works — Quick Summary

```
┌─────────────────────────────────────────────────────────────────┐
│                         BOOT                                    │
│   ESP32 starts → connects to Wi-Fi → waits for GPS fix          │
└─────────────────────────────────┬───────────────────────────────┘
                                  │
                    ┌─────────────▼──────────────┐
                    │        MAIN LOOP            │
                    │ Poll ultrasonic every 100ms │
                    │ Poll flame sensor every loop│
                    │ Parse GPS NMEA continuously │
                    │ Check SOS interrupt flag    │
                    └─────────────┬──────────────┘
                                  │
          ┌───────────────────────┼──────────────────────┐
          ▼                       ▼                      ▼
  [Obstacle < 50cm]        [Flame = LOW]         [SOS pressed]
          │                       │                      │
   Buzzer 2 beeps         Buzzer long tone        Read GPS coords
   Vibration pulse        Vibration sustained     Build Maps URL
                          Pushbullet: FIRE!       Pushbullet: SOS!
                                                  Buzzer 3 beeps
```

---

## 📊 Advantages

- **Low Cost** — Full BOM under ₹2,000, making it accessible in India.
- **Real-Time Alerting** — Caregiver notified in under 2 seconds of an event.
- **No Smartphone Required by User** — All processing is on the stick; user needs no tech skills.
- **Multi-Hazard** — Covers obstacles, fire, and emergencies in one device.
- **Extendable** — ESP32's extra GPIOs and Wi-Fi allow easy addition of new sensors or features.
- **Energy Efficient** — Deep sleep between sensor polls can extend battery life significantly.

---

## ⚠️ Limitations

- Requires **Wi-Fi connectivity** for remote notifications (local alerts still work offline).
- GPS cold-start takes **30–60 seconds** for first fix after power-on.
- Ultrasonic sensor has a ~**15° detection cone** — narrow-angle obstacle misses are possible.
- **Battery life ~4–6 hours** in full-active mode without power optimization.
- Flame sensor effective range approximately **60–100 cm**.

---

## 🔭 Future Scope

- **GSM/4G Module** (SIM800L) for offline SOS when Wi-Fi unavailable.
- **Fall Detection** using MPU-6050 accelerometer for automatic SOS.
- **Text-to-Speech** announcements of distance and hazard type.
- **Multiple Ultrasonic Sensors** for wider field of detection.
- **Indoor Navigation** using RFID tags at key locations.
- **Companion Mobile App** with real-time map tracking for caregivers.
- **Voice Feedback** for nearest obstacle distance in local language.

---

## 👨‍💻 Built By

| Roll No. | Name |
|---|---|
| 22CM010 | Naman Sunil Bharsakale |
| 22CM016 | Sarvesh Sham Chaudhari |
| 22CM019 | Yash Sandip Chaukhande |
| 22CM030 | Jay Ajay Ganorkar |
| 23CM206 | Mahammad Shahzad Mohammad Zameer |

**Under the guidance of:**
Prof. (Mrs.) C. R. Chaudhari
Department of Computer Engineering
Government Polytechnic Amravati *(An Autonomous Institute of Govt. of Maharashtra)*

**Academic Year:** 2024–2025
**Program:** Diploma in Computer Engineering

---

## 📚 References

- ESP32 Technical Reference Manual — [Espressif Systems](https://www.espressif.com/sites/default/files/documentation/esp32_technical_reference_manual_en.pdf)
- HC-SR04 Ultrasonic Sensor Datasheet — Cytron / ELEC Freaks
- u-blox NEO-6M GPS Module Product Summary — [u-blox.com](https://www.u-blox.com/en/product/neo-6-series)
- Pushbullet API Documentation — [docs.pushbullet.com](https://docs.pushbullet.com)
- TinyGPS++ Library — [arduiniana.org](http://arduiniana.org/libraries/tinygpsplus/)
- World Health Organization, "Blindness and Vision Impairment" — WHO, 2023
- NMEA 0183 Standard — National Marine Electronics Association

---

<p align="center">Made with ❤️ at Government Polytechnic Amravati | Empowering independent mobility for the visually impaired</p>
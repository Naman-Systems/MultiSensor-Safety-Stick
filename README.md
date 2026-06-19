# 🦯 Multi-Sensor Safety Stick

> A revolutionary IoT-based assistive device enhancing the safety and autonomy of visually impaired individuals — integrating object detection, fire sensing, GPS tracking, and real-time SOS alerting into a single smart cane.

<p align="center">
  <img src="https://img.shields.io/badge/Platform-ESP32-blue?style=for-the-badge&logo=espressif" />
  <img src="https://img.shields.io/badge/Language-C%2FC%2B%2B%20Arduino-orange?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Notification-Pushbullet%20API-green?style=for-the-badge" />
  <img src="https://img.shields.io/badge/GPS-NEO--6M%20u--blox-lightgrey?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Domain-Assistive%20Technology-purple?style=for-the-badge" />
</p>

---

## 📖 Abstract

The Smart Stick is a revolutionary IoT-based assistive device meticulously crafted to enhance the safety and autonomy of visually impaired individuals. It integrates cutting-edge object detection technologies, intuitive feedback systems, and seamless connectivity to provide real-time assistance and navigation support.

Through the integration of ultrasonic sensors, the Smart Stick swiftly identifies objects within the user's surroundings, activating a buzzer that emits auditory cues proportional to the proximity of the obstacle. It also incorporates a robust fire detection mechanism, a GPS module for accurate real-time location tracking, and an integrated emergency SOS button that promptly notifies designated family members with the user's exact location. A vibration motor provides tactile feedback for users with hearing impairments.

---

## ✨ Features

| Feature | Technology | Benefit |
|---|---|---|
| Obstacle Detection | HC-SR04 Ultrasonic Sensor | Real-time warnings for objects in path at any height |
| Fire Hazard Detection | IR Flame Sensor | Detects fire, alerts user + caregiver instantly |
| GPS Location Tracking | NEO-6M GPS Module | Real-time coordinates shared on SOS or emergency |
| Emergency SOS Alert | SOS Button + Pushbullet API | One press sends location to caregiver's phone |
| Caregiver Notification | Pushbullet API (HTTPS) | Instant push notification to smartphone |
| Auditory Feedback | Active Buzzer | Distinct tone patterns for obstacle / fire / SOS confirm |
| Haptic Feedback | ERM Vibration Motor | Tactile alerts; stronger patterns for hearing-impaired users |
| Companion App | MIT App Inventor + Pushbullet | Caregiver location monitoring interface |

---

## 🛠️ Hardware Requirements

### Components List

| Component | Model / Type | Qty | Role |
|---|---|---|---|
| Microcontroller | ESP32 Development Board (Espressif) | 1 | Central processor, Wi-Fi connectivity |
| Obstacle Sensor | HC-SR04 Ultrasonic Sensor | 1 | Detect objects up to 400 cm |
| Fire Sensor | IR Flame Detection Sensor Module | 1 | Detect flames (760–1100 nm IR) |
| GPS Module | NEO-6M (u-blox) | 1 | Real-time location via NMEA over UART |
| Buzzer | Active Buzzer 5V | 1 | Auditory alert patterns |
| Vibration Motor | ERM Coin Vibration Motor 3V | 1 | Haptic / tactile alerts |
| Push Button | Momentary Tactile Switch | 1 | SOS emergency trigger |
| NPN Transistor | BC547 | 2 | Drive buzzer and motor (current amplifier) |
| Flyback Diode | 1N4007 | 1 | Motor back-EMF protection |
| Battery | Li-Po 3.7V 2000mAh Rechargeable | 1 | Portable power source |
| Boost Converter | MT3608 (3.7V → 5V) | 1 | Regulated 5V for ESP32 and actuators |
| Charger Module | TP4056 Li-Po Charger + Protection | 1 | Safe battery charging |
| Miscellaneous | Jumper wires, PCB/perf board, cane body | — | Assembly |

---

## 💻 Software Requirements

| Software / Library | Purpose |
|---|---|
| **Arduino IDE 2.x** | Firmware development, compilation, and flashing to ESP32 |
| **ESP32 Board Package** | Arduino core for ESP32 (add via Board Manager) |
| **TinyGPS++ Library** | Parses NMEA GPS sentences from NEO-6M (by Mikal Hart) |
| **ArduinoJson Library** | Constructs JSON body for Pushbullet API calls |
| **HTTPClient** (built-in ESP32) | HTTPS POST requests to Pushbullet |
| **WiFiClientSecure** (built-in ESP32) | TLS/SSL for secure API communication |
| **Pushbullet App** | Installed on caregiver's smartphone for receiving alerts |
| **MIT App Inventor** | Companion mobile app for caregiver location monitoring |

---

## 📌 Pin Configuration

| ESP32 GPIO | Connected Component | Direction | Protocol |
|---|---|---|---|
| GPIO 5 | HC-SR04 TRIG | OUTPUT | 10 μs digital pulse |
| GPIO 18 | HC-SR04 ECHO | INPUT | Time-of-flight pulse measurement |
| GPIO 34 | Flame Sensor OUT | INPUT | Digital (LOW = fire) |
| GPIO 16 (RX2) | NEO-6M GPS TX | INPUT | UART2 @ 9600 baud |
| GPIO 17 (TX2) | NEO-6M GPS RX | OUTPUT | UART2 @ 9600 baud |
| GPIO 27 | SOS Push Button | INPUT_PULLUP + INT | Falling edge hardware interrupt |
| GPIO 25 | Buzzer (via BC547 NPN) | OUTPUT | Digital / PWM Tone |
| GPIO 26 | Vibration Motor (via BC547 NPN) | OUTPUT | Digital / PWM |

---

## ⚡ Circuit Diagram

```
┌────────────────────────────────────────────────────────────────────┐
│                         ESP32 DevKit V1                            │
│                                                                    │
│   HC-SR04 Ultrasonic Sensor                                        │
│   ┌────────────┐                                                   │
│   │ VCC  ──────┼── 3V3 ──────────────────────────── 3V3 pin       │
│   │ GND  ──────┼── GND ──────────────────────────── GND (common)  │
│   │ TRIG ──────┼── GPIO 5                                          │
│   │ ECHO ──────┼── GPIO 18                                         │
│   └────────────┘                                                   │
│                                                                    │
│   IR Flame Sensor Module                                           │
│   ┌────────────┐                                                   │
│   │ VCC  ──────┼── 3V3                                             │
│   │ GND  ──────┼── GND                                             │
│   │ OUT  ──────┼── GPIO 34 (reads LOW when fire detected)          │
│   └────────────┘                                                   │
│                                                                    │
│   NEO-6M GPS Module                                                │
│   ┌────────────┐                                                   │
│   │ VCC  ──────┼── 3V3                                             │
│   │ GND  ──────┼── GND                                             │
│   │ TX   ──────┼── GPIO 16 (UART2 RX)                              │
│   │ RX   ──────┼── GPIO 17 (UART2 TX)                              │
│   └────────────┘                                                   │
│                                                                    │
│   SOS Push Button                                                  │
│   ┌────────────┐                                                   │
│   │ PIN1 ──────┼── GPIO 27 (INPUT_PULLUP + interrupt)              │
│   │ PIN2 ──────┼── GND                                             │
│   └────────────┘                                                   │
│                                                                    │
│   Buzzer (Active, 5V)                                              │
│   GPIO 25 ──[1kΩ]──▶ Base of BC547 NPN                            │
│                       Collector ──── Buzzer (+)                   │
│                       Emitter  ──── GND                           │
│                       Buzzer (-) ──── GND                         │
│                                                                    │
│   Vibration Motor (ERM, 3V)                                        │
│   GPIO 26 ──[1kΩ]──▶ Base of BC547 NPN                            │
│                       Collector ──── Motor (+)                    │
│                       Emitter  ──── GND                           │
│                       Motor (-) ──── GND                          │
│                  [1N4007 diode across Motor terminals, cathode +]  │
│                                                                    │
│   Power Supply Chain                                               │
│   [Li-Po 3.7V] ──▶ [TP4056 Charger] ──▶ [MT3608 Boost → 5V]      │
│                                               │                   │
│                                    ┌──────────┤                   │
│                                    ▼          ▼                   │
│                              [ESP32 VIN]  [Buzzer 5V]             │
│                              [ESP32 3V3] ──▶ Sensors VCC          │
└────────────────────────────────────────────────────────────────────┘

⚠️  IMPORTANT: Always drive buzzer and vibration motor through NPN
    transistors. ESP32 GPIO pins can only safely source ~12 mA.
    Both loads require 100–200+ mA. Direct connection WILL damage ESP32.
```

---

## 🚀 Getting Started

### Step 1 — Install Arduino IDE & ESP32 Board Package

```
1. Download Arduino IDE 2.x from https://www.arduino.cc/en/software
2. Open: File → Preferences → Additional Board Manager URLs
3. Add: https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
4. Go to: Tools → Board → Boards Manager → Search "esp32" → Install
```

### Step 2 — Install Required Libraries

```
Tools → Manage Libraries → Search and install:
  - TinyGPS++ by Mikal Hart
  - ArduinoJson by Benoit Blanchon
```

### Step 3 — Configure `config.h`

```cpp
// ─── Wi-Fi Credentials ────────────────────────────────────────────
#define WIFI_SSID           "YourWiFiNetworkName"
#define WIFI_PASSWORD       "YourWiFiPassword"

// ─── Pushbullet API Key ───────────────────────────────────────────
// Get this from: pushbullet.com → Settings → Access Tokens
#define PUSHBULLET_API_KEY  "o.XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"

// ─── Sensor Thresholds ────────────────────────────────────────────
#define OBSTACLE_THRESHOLD_CM   50      // Warn if obstacle within 50 cm
#define FLAME_COOLDOWN_MS       10000   // 10s between fire notifications

// ─── GPIO Pin Definitions ─────────────────────────────────────────
#define PIN_TRIG        5
#define PIN_ECHO        18
#define PIN_FLAME       34
#define PIN_GPS_RX      16
#define PIN_GPS_TX      17
#define PIN_SOS_BTN     27
#define PIN_BUZZER      25
#define PIN_VIBRATION   26
```

### Step 4 — Flash the Firmware

```
1. Connect ESP32 to PC via USB-C or Micro-USB cable.
2. In Arduino IDE: Tools → Board → ESP32 Dev Module
3. Select correct Port: Tools → Port → COMx (Windows) or /dev/ttyUSBx (Linux)
4. Click Upload ▶
5. Open Serial Monitor at 115200 baud to verify sensor readings.
```

### Step 5 — Setup Pushbullet on Caregiver's Phone

```
1. Download Pushbullet app (Android / iOS).
2. Create free account at pushbullet.com.
3. Go to: Account Settings → Access Tokens → Create Access Token.
4. Copy token → paste into config.h as PUSHBULLET_API_KEY.
5. Ensure the caregiver's phone allows Pushbullet notifications.
```

---

## 📡 How It Works — Quick Reference

```
                    [BOOT]
              Connect Wi-Fi → Get GPS Fix
                         │
                    [MAIN LOOP]
           ┌─────────────┼────────────────┐
           ▼             ▼                ▼
     [Ultrasonic]   [Flame Sensor]   [GPS Update]
      Read dist      Read GPIO          NMEA parse
           │             │
     dist < 50cm?    GPIO == LOW?
           │             │
          YES            YES
           │             │
     Buzzer 2 beeps  Buzzer long tone
     Vibrate pulse   Vibrate sustained
                     Pushbullet: FIRE ALERT
                              │
                    [SOS Button ISR]
                         │
                    Flag detected in loop
                         │
                    Read GPS coordinates
                    Build Google Maps URL
                    Pushbullet: SOS + location
                    Buzzer: 3 rapid beeps
```

---

## 📊 Advantages

- **Enhanced Safety** — Real-time obstacle and fire detection minimizes accident risk.
- **Live Location Monitoring** — GPS + Pushbullet lets caregivers track the user remotely.
- **Quick Emergency Response** — SOS button delivers instant location to caregiver.
- **Cost-Effective** — Built on open-source platforms (Arduino, Pushbullet); affordable for wider adoption.
- **Multimodal Alerts** — Buzzer + vibration covers users with hearing impairments.
- **Independence** — Users navigate without depending on human assistance.

---

## ⚠️ Limitations

- Wi-Fi required for remote notifications (local alerts still work offline).
- GPS cold-start takes 30–60 seconds after first power-on.
- Ultrasonic ~15° cone — extreme-angle obstacles may not be detected.
- Battery life ~4–6 hours in full-active mode.
- Electronic components sensitive to moisture/water.

---

## 🔭 Future Scope

- **AI Obstacle Classification** — Identify object types for context-aware navigation.
- **Smart Traffic Interaction** — Receive signals from traffic lights for safe crossing.
- **Voice-Controlled Interface** — Manage functions via voice commands.
- **Offline Navigation** — Stored maps for low-connectivity areas.
- **GSM/4G Module** — SOS notifications without Wi-Fi.
- **Fall Detection** — Auto-SOS using MPU-6050 accelerometer.
- **Solar-Assisted Charging** — Extended battery life.
- **Community Hazard Sharing** — Users report and share environmental hazards.

---

## 👨‍💻 Built By

| Roll No. | Name |
|---|---|
| 22CM010 | Naman Sunil Bharsakale |
| 22CM016 | Sarvesh Sham Chaudhari |
| 22CM019 | Yash Sandip Chaukhande |
| 22CM030 | Jay Ajay Ganorkar |
| 23CM206 | Mahammad Shahzad Mohammad Zameer |

**Under the guidance of Prof. (Mrs.) C. R. Chaudhari**
Department of Computer Engineering
**Government Polytechnic Amravati** *(An Autonomous Institute of Govt. of Maharashtra)*
Academic Year: **2024–2025** | Program: **Diploma in Computer Engineering**

---

## 📚 References

- Arduino IDE Documentation: https://docs.arduino.cc/learn/starting-guide/the-arduino-software-ide/
- NEO-6M GPS Interfacing Guide: https://circuitdigest.com/microcontroller-projects/interfacing-neo6m-gps-
- ESP Circuit Programming: https://circuitjournal.com/programming-esp8266-over-wifi
- MIT App Inventor: https://appinventor.mit.edu/
- Vibration Motor with NPN: https://lab.arts.ac.uk/books/physical-computing/page/using-a-vibration-motor
- Pushbullet API: https://www.pushbullet.com/
- HC-SR04 with ESP: https://randomnerdtutorials.com/esp8266-nodemcu-hc-sr04-ultrasonic-arduino/

---

<p align="center">
Made with ❤️ at Government Polytechnic Amravati<br>
Empowering independent mobility for visually impaired individuals
</p>
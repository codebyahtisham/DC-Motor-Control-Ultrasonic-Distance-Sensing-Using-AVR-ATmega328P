# ⚡ DC Motor Control & Ultrasonic Distance Sensing — AVR ATmega328P

> Bare-metal embedded systems project featuring DC motor direction control via L293D H-bridge, 2WD robot chassis assembly, and real-time ultrasonic distance measurement with LCD display — all on ATmega328P.

![Status](https://img.shields.io/badge/Status-Completed-brightgreen)
![Domain](https://img.shields.io/badge/Domain-Embedded%20Systems-blue)
![Type](https://img.shields.io/badge/Type-University%20Project-orange)
![MCU](https://img.shields.io/badge/MCU-ATmega328P-purple)

---

## 📌 Overview

This lab project covers **DC motor interfacing** and **ultrasonic sensor integration** with the ATmega328P microcontroller. A DC motor's direction is controlled through the **L293D dual H-bridge driver** using GPIO register manipulation. The project extends to assembling a **2WD robot chassis** with coordinated forward/reverse movement. Finally, the **HC-SR04 ultrasonic sensor** is interfaced using Timer1 input capture for real-time distance measurement, displayed on a **16×2 LCD** via a custom 4-bit driver — all written in bare-metal C without Arduino libraries.

---

## 🎯 Objectives

- Control DC motor direction (CW / CCW / Stop) via L293D and ATmega328P GPIO
- Assemble and program a 2WD robot chassis for forward/reverse movement
- Interface HC-SR04 ultrasonic sensor using Timer1 input capture for distance measurement
- Display real-time distance on a 16×2 LCD using custom 4-bit driver routines
- Simulate all circuits in Proteus and implement on hardware

---

## ⚙️ Motor Direction Control

Motor direction is controlled by toggling **PD0** and **PD1** on PORTD, routed through the L293D H-bridge:

| PD0 | PD1 | Motor State      |
|:---:|:---:|------------------|
|  0  |  0  | **Stop**         |
|  0  |  1  | **Anti-clockwise** |
|  1  |  0  | **Clockwise**    |
|  1  |  1  | **Stop (brake)** |

---

## 🏗️ System Architecture

```
                        ┌──────────────┐
                        │  ATmega328P  │
                        │   (16 MHz)   │
                        └──┬───┬───┬───┘
                           │   │   │
              PD0/PD1      │   │   │  PB1 (Trig)  PB0 (Echo/ICP1)
                 │         │   │   │      │              │
                 ▼         │   │   │      ▼              │
          ┌──────────┐     │   │   │  ┌──────────┐       │
   +9V ──▶│  L293D   │    │   │   │  │ HC-SR04  │◀──────┘
          │ H-Bridge │    │   │   │  │ Sensor   │
          └──┬───┬───┘    │   │   │  └──────────┘
             │   │        │   │   │
          Motor A  Motor B│   │   │
             (2WD Robot)  │   │   │
                          │   │   │
                    PD2-PD7   PD0 PD1
                          │   │   │
                     ┌────┴───┴───┴────┐
                     │   16×2 LCD      │
                     │  (4-bit mode)   │
                     └─────────────────┘
```

---

## 🧰 Tech Stack

### Hardware Components

| Component              | Details                          |
|-----------------------|----------------------------------|
| Microcontroller       | ATmega328P (16 MHz crystal)      |
| Motor Driver          | L293D (dual H-bridge)            |
| Ultrasonic Sensor     | HC-SR04                          |
| Display               | 16×2 LCD (4-bit mode)            |
| Motors                | 9V DC / BO Motors                |
| Chassis               | 2WD Robot Chassis                |
| Programmer            | USBasp                           |
| Other                 | Breadboard, LEDs, resistors, capacitors, crystal oscillator |

### Software

| Tool             | Purpose                                |
|-----------------|----------------------------------------|
| Embedded C      | Bare-metal firmware (AVR-GCC)          |
| Microchip Studio| IDE for development & compilation      |
| Proteus         | Circuit simulation & verification      |

---

## 💻 What I Built

### 1 — Single Motor Direction Control

Toggled PD0/PD1 through the L293D to cycle through clockwise → stop → anti-clockwise → brake states with 2-second intervals.

### 2 — 2WD Robot Chassis

Assembled a two-wheel-drive robot and programmed dual-motor coordination — forward for 10 seconds, reverse for 10 seconds — using both L293D channels.

### 3 — Ultrasonic Distance Measurement

Interfaced HC-SR04 using Timer1 input capture mode to measure echo pulse width. Distance derived from timer count:

```
Distance (cm) = Timer_Count / 466.47
```

**Derivation (8 MHz, no prescaler):**
```
Sound velocity          = 34300 cm/s
Distance                = (34300 × Timer_Value) / 2
Timer tick @ 8 MHz      = 0.125 µs
∴ Distance              = 17150 × Timer_Value × 0.125 × 10⁻⁶
                        = Timer_Value / 466.47  cm
```

Results displayed live on a 16×2 LCD using a custom 4-bit LCD driver with position control.

---

## 🔬 Key Concepts Demonstrated

| Concept                    | Application                                              |
|---------------------------|----------------------------------------------------------|
| **PWM & Duty Cycle**       | Motor speed control via pulse width modulation           |
| **Timer Input Capture**    | Precise echo pulse measurement for distance sensing      |
| **GPIO Register Control**  | Direct DDR/PORT manipulation for motor & LCD interfacing |
| **Interrupt Handling**     | Timer1 overflow ISR for extended range measurement       |
| **Peripheral Interfacing** | LCD (4-bit), HC-SR04, L293D H-bridge                     |

---

## 📁 Repository Structure

```
├── README.md                    # Project documentation
├── ES_LAB12_19.pdf              # Full lab report with code, circuits & photos
└── src/
    ├── motor_control.c          # Motor direction control firmware
    ├── robot_2wd.c              # 2WD robot forward/reverse firmware
    └── ultrasonic_lcd.c         # HC-SR04 + LCD distance display firmware
```

---

## 📄 Documentation

| Document              | Link                                  |
|----------------------|---------------------------------------|
| 📘 Full Report (PDF) | [View Report](./ES_LAB12_19.pdf)      |

---

## 📸 Screenshots

<details>
<summary>Click to view hardware & simulation</summary>

### Hardware Implementation
The 2WD robot chassis with ATmega328P, L293D motor driver, HC-SR04 ultrasonic sensor, and USBasp programmer connected on a breadboard. Motors are driven via the L293D outputs and the ultrasonic sensor is mounted at the front.

### Proteus Simulation
Circuit simulated in Proteus showing ATmega328P connected to L293D (IN1/IN2 to PD0/PD1), HC-SR04 (Trig to PB1, Echo to PB0/ICP1), and 16×2 LCD on PORTD upper pins.

</details>

---

## 👤 Author

| Detail          | Info                              |
|----------------|-----------------------------------|
| **Name**       | Ahtisham Saleem                   |
| **Roll Number**| NIM-BSEE-2021-19                  |
| **Course**     | EE-252L: Introduction to Embedded Systems |
| **Instructor** | Dr. Hamza Zad Gul                 |
| **University** | Namal University, Mianwali        |
| **Program**    | BS Electrical Engineering         |

---

## 📬 Contact

- **Email:** engr.ahtishamsaleem@gmail.com
- **LinkedIn:** [Ahtisham Saleem](https://www.linkedin.com/in/ahtisham-salim)
- **GitHub:** [@codebyahtisham](https://github.com/codebyahtisham)

---

<p align="center">
  ⭐ If you found this project interesting, consider giving it a star!
</p>

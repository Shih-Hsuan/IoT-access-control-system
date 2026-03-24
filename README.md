<h1 align="center">IoT Access Control & Audio Feedback System</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Language-C-00599C?style=for-the-badge&logo=c" alt="C">
  <img src="https://img.shields.io/badge/MCU-8051%20%7C%20ESP32-red?style=for-the-badge" alt="MCU">
  <img src="https://img.shields.io/badge/Protocol-UART%20%7C%20MQTT-success?style=for-the-badge" alt="Protocol">
  <img src="https://img.shields.io/badge/Hardware-EEPROM%20%7C%20PWM-yellow?style=for-the-badge" alt="Hardware">
</p>

## Project Overview
This repository contains the source code for an **IoT-enabled Access Control System**, originally developed as a course project at National Taiwan Ocean University. The system bridges a bare-metal **8051 microcontroller (Local MCU)** with an **ESP32 (IoT Gateway)**, combining physical security control with remote cloud connectivity and PWM-based audio feedback.

## System Demonstration

| 🖥️ 8051 Local Core Board | 📱 MyMQTT Mobile Interface |
| :---: | :---: |
| <img src="https://github.com/user-attachments/assets/1eaeec85-92d8-4e13-a7bd-7b44596807fa"> | <img src="https://github.com/user-attachments/assets/8d631130-e799-42e4-a36b-55c068d5e3e7" width="300" alt="MQTT App"> |
| *Hardware setup displaying system status via 7-segment LEDs and matrix keypad.* | *Remote interface used for sending `SET` passwords and PWM `PLAY` commands.* |

## System Architecture & Hardware Partitioning
The project adopts a Hardware-Software Co-design approach, dividing tasks between two microcontrollers via **UART**:

### 1. 8051 Microcontroller (Local Core)
- **User Interface:** Scans a 4x4 Matrix Keypad for password entry and drives an 8-digit 7-Segment LED array for status display.
- **Non-Volatile Storage:** Utilizes **EEPROM** for reliable, persistent password storage.
- **Actuator Control:** Controls a physical relay (mocking a door lock mechanism).

### 2. ESP32 (IoT Gateway & Audio Engine)
- **Cloud Connectivity:** Connects to Wi-Fi and utilizes the **MQTT** protocol to interact with a mobile application.
- **Audio Generation:** Implements Hardware **PWM (Pulse Width Modulation)** to drive a speaker, generating authentication alerts and custom melodies.

## Key Features

### 1. Bidirectional Authentication (1A2B Logic)
The system features a dual-mode authentication process utilizing a "Bulls and Cows" (XAXB) evaluation logic:
- **Local-Set, Remote-Guess:** A 4-digit password is set via the 8051 keypad. Users attempt to unlock it remotely via the MQTT App, receiving real-time `XAXB` feedback.
- **Remote-Set, Local-Guess:** The password is set via the MQTT App (`SETxxxx`). Users attempt to unlock it physically using the 8051 keypad, with the ESP32 evaluating the input.

### 2. Remote Music Player (PWM Melody Engine)
Upon successful authentication, the system unlocks an audio mode. Users can send formatted string payloads via MQTT to dynamically play music on the local hardware.
- **Payload Format:** `PLAY[Frequency/Note][Pitch][Beat]`


## Communication Protocols

- **Local Bus:** UART bridging 8051 and ESP32.
- **Cloud Protocol:** MQTT 
  - **Topic:** `es/project/01057006`

## Hardware Wiring & Pinouts

**8051 Core Board:**
- `P0` : 8-Digit 7-Segment Display (Data)
- `P1` : Matrix Keypad
- `P2^2` : Segment Latch Enable
- `P2^3` : Bit Latch Enable
- `P2^7` : Relay Control (Lock Actuator)

**ESP32 ↔ 8051 Interface:**
- `GPIO17` (ESP32 TX) ↔ `RX` (8051)
- `GPIO16` (ESP32 RX) ↔ `TX` (8051)
- `GPIO15` (ESP32 PWM) ↔ Speaker Module
- `GND` ↔ Common Ground



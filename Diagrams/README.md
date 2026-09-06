# P2 – Edge TinyML Acoustic Event Classifier

## Group Members
- Tanveer Singh
- Harpreet

## Project Overview
This project develops an edge-based TinyML acoustic event classifier using an ESP32 DevKit v1 and an INMP441 I²S microphone.

The system classifies three sound categories:
- Background
- Clap
- Whistle

Audio is processed locally on the ESP32 using an Edge Impulse TinyML model. Only accepted clap and whistle events are transmitted to ThingSpeak over Wi-Fi/HTTPS. Raw audio is not uploaded to the cloud.

## Hardware
- ESP32 DevKit v1
- INMP441 I²S MEMS microphone
- Breadboard
- Jumper wires

## Machine Learning
- Edge Impulse
- MFE audio features
- Quantized int8 classifier
- Final model testing accuracy: 76.67%

## Cloud
- ThingSpeak
- HTTPS / REST API
- Event-only telemetry

## Repository Structure
- `Arduino/` – ESP32 source code
- `Diagrams/` – wiring, data-flow and security diagrams
- `Evidence/` – implementation screenshots/results
- `docs/` – implementation documentation

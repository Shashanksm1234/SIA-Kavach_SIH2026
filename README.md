# NAAN-Sense_SIA-Kavach_SIH2026
# SIH 2026 Project Repository

## 1. Project Information
* **Project Title:** SIA-Kavach – High-Altitude Tactical Counter-UAS Defense Platform
* **PS ID:** SIH26050
* **PS Title:** High Altitude Performance Optimization and Robust Design of Anti-Drone System
* **Category:** Hardware
* **Theme:** Smart Automation
* **Ministry/Organization:** DRDO
* **Team Members:** Shashank Shekhar Mishra (2025UBT1072), Reyansh Adlakha, Dhruv Bharti

## 2. Problem Statement
Anti-drone systems are deployed for the detection, tracking, identification, and neutralization of unauthorized drones. However, operational performance significantly degrades in high-altitude environments characterized by extreme cold, low atmospheric pressure, reduced air density, and high winds. Components such as cables, motors, and batteries experience altered material properties and thermal stresses, while structural dynamics and component responses suffer, leading to severe degradation in precision tracking and stabilization. There is a critical requirement to develop robust design methodologies and compensation mechanisms to ensure reliable performance in these harsh operational conditions.

## 3. Proposed Solution
SIA-Kavach is an indigenously architected, extreme-altitude counter-UAS platform engineered to guarantee 24/7 airspace defense between 3,500m and 5,500m MSL. The hardware is secured within a Bud Industries IP65 NEMA enclosure featuring M12 waterproof venting for pressure equalization. To combat cold-induced voltage collapse, it utilizes a Daly Smart BMS managing LiFePO4 cells and active Stego PTC core heating. On the software side, an NVIDIA Jetson Orin NX runs an Extended Kalman Filter for Bayesian sensor fusion (FMCW Radar + LWIR Thermal + Optical) alongside Adaptive PID gain-scheduling to maintain sub-degree pointing accuracy during high-wind buffeting, culminating in automated RF soft-kill mitigation.

## 4. Key Features
* **Ruggedized Environmental Housing:** IP65 sealed with bidirectional pressure equalization.
* **Active Thermal Management:** PTC heating and custom silicone thermal barriers to prevent localized hotspots and sensor noise.
* **Dynamic Sensor Fusion:** Bayesian confidence weighting dynamically shifts reliance to 77 GHz FMCW radar during optical whiteouts.
* **Mechanical Stabilization:** Adaptive PID gain-scheduling counters wind jitter, maintaining a torque residual of <0.05°.
* **Tactical C2 Dashboard:** Real-time telemetry, AES-256 secure authentication, and active OSINT threat tracking.
* **Soft-Kill Mitigation:** Targeted RF directed jammer (2.4/5.8 GHz & GNSS L1/L2) to trigger drone fail-safes.

## 5. Technology Stack
* **Hardware Compute:** NVIDIA Jetson Orin NX
* **Sensors:** Texas Instruments IWR6843 (mmWave Radar), FLIR Lepton 3.5 (LWIR Thermal)
* **Actuation & Power:** SimpleFOC Mini Brushless Drivers, Daly Smart BMS (4S 100A), Aatral Sodium-Ion/LiFePO4 Packs
* **Communications:** Microchip RN2903 LoRa Mesh (Encrypted Telemetry)
* **Software Backend:** Python, FastAPI, C++ (Sensor Drivers)
* **Frontend UI:** React / Vue.js (Web-based Tactical Dashboard)

## 6. Architecture
See `docs/architecture.md` for full schematic breakdowns.

[Sensors: Radar/Thermal/EO] ---> [NVIDIA Jetson Orin NX (Sensor Fusion & Vision)]
                                      |
                                      +---> [SimpleFOC Gimbal Controllers] ---> [Target Tracking]
                                      |
[Power: Daly BMS + LiFePO4] --------> +---> [Stego PTC Heating & Thermal Mgmt]
                                      |
[Encrypted LoRa Mesh] <---------------+---> [Tactical C2 Dashboard] ---> [RF Jammer Deployment]

## 7. Repository Structure

YOUR-SIH-PROJECT/
├── README.md
├── SUBMISSION_GUIDE.md
├── submission/
│   ├── PRESENTATION.md
│   └── DEMO.md
├── src/
│   ├── main.py
│   ├── fusion_engine/
│   └── dashboard_ui/
├── docs/
│   ├── architecture.md
│   └── challenge_accommodation_matrix.md
├── assets/
│   └── screenshots/
│       ├── hardware_build/
│       ├── software_dashboard/
│       └── README.md
├── requirements.txt
├── .gitignore
└── LICENSE

## 8. Final Presentation
Our complete challenge accommodation matrix, hardware specifications, and system architecture presentation can be found in `submission/PRESENTATION.md`.

## 9. Demo Video
A full simulation of the detection-to-neutralization pipeline and hardware integration is available in `submission/DEMO.md`.

## 10. Screenshots / Prototype Photos
Detailed images of the 3D-printed PETG mounts, wiring harnesses, PDB integration, and the tactical UI are located in `assets/screenshots/`.

## 11. Installation
```bash
git clone <YOUR_REPOSITORY_URL>
cd <YOUR_PROJECT_FOLDER>
pip install -r requirements.txt

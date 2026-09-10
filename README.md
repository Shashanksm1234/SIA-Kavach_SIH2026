# NAAN-Sense_SIA-Kavach_SIH2026
# SIH 2026 Project Repository


## 1. Project Information
* **Project Title:** SIA-Kavach – High-Altitude Tactical Counter UAS Defense Platform
* **PS ID:** SIH26050
* **PS Title:** High Altitude Performance Optimization and Robust Design of Anti-Drone System
* **Category:** Hardware
* **Theme:** Robotics and Drone
* **Ministry/Organization:** DRDO

## 2. Problem Statement
Anti-drone systems are deployed for the detection, tracking, identification, and neutralization of unauthorized drones. However, operational performance significantly degrades in high-altitude environments characterized by extreme sub-zero cold (-40°C), thin atmospheric air pressure, low air density, and heavy wind buffeting. Components such as cables, motors, and batteries experience altered material properties, cold-induced voltage collapse, and thermal stresses. Concurrently, optical lenses fog, sensors de-sync, and structural vibration impairs target lock. There is a critical requirement to engineer robust hardware compensation mechanisms alongside environmental health telemetry to guarantee operational reliability in these harsh border conditions.

## 3. Proposed Solution
SIA-Kavach is an indigenously architected, extreme-altitude counter-UAS platform engineered to guarantee 24/7 airspace defense between 3,500m and 5,500m MSL. The hardware is enclosed in a modified Bud Industries IP65 NEMA enclosure featuring M12 waterproof venting for ambient pressure equalization. 

To eliminate cold soak failure, the system integrates a dual-zone thermal control loop featuring Stego PTC heating elements, segmented silicone thermal barriers, and active thermal probe telemetry tracking compute cluster and battery core temps. System health is continuously evaluated using barometric, ambient temperature, wind velocity, and tri-axial vibration sensors. 

Target tracking is executed on an NVIDIA Jetson Orin NX running an Extended Kalman Filter (EKF) that fuses 77 GHz FMCW Radar, LWIR Thermal, EO Visual, and Passive RF signals. Mechanical pointing accuracy is stabilized via Adaptive PID gain-scheduling on SimpleFOC drivers, culminating in an automated multi-band RF soft-kill neutralization countermeasure.

## 4. Key Features
* **Environmental & Atmospheric Diagnostics:** Real-time tracking of station altitude (MSL), ambient air temperature, barometric pressure (hPa), and crosswind velocity.
* **Structural Health Telemetry:** Tri-axial IMU monitoring to detect and compensate for RMS mechanical vibration caused by high-altitude gales.
* **Active Closed-Loop Thermal Management:** Internal temperature telemetry actively monitors Jetson CPU, GPU Tensor Cores, and power rails, cycling Stego PTC heaters to prevent thermal clock-gating and cold collapse.
* **Ruggedized Pressure Equalization:** IP65 sealed housing fitted with bidirectional M12 breathable vents to prevent seal blowouts under low ambient atmospheric pressure.
* **Multi-Modal Bayesian Sensor Fusion:** Dynamic covariance inflation shifts tracking reliance off degraded optical sensors onto 77 GHz FMCW Radar during blizzards and mountain whiteouts.
* **Sub-Zero Power Architecture:** Daly Smart BMS paired with cold-tolerant Aatral Sodium-Ion / LiFePO4 cells rated for operation down to -40°C.
* **Targeted Soft-Kill Mitigation:** Directed RF suppression covering 2.4 GHz ISM, 5.8 GHz FPV, and GNSS L1/L2 bands to force hostile drones into blind fail-safe landing.

## 5. Technology Stack

### A. Sensor Suite (Target Acquisition & Tracking)
* **Radar:** 77 GHz / TI IWR6843 FMCW mmWave Radar
* **Thermal Vision:** FLIR Lepton 3.5 Long-Wave Infrared (LWIR) Camera
* **Optical Tracking:** High-Definition Electro-Optical (EO) Telephoto Camera
* **Passive RF Detection:** Software-Defined Radio (SDR) / Broadband Spectrum Receiver
* **Acoustic Array:** Beamforming microphone array for acoustic signature profiling

### B. Environmental & Diagnostic Sensors (System Health)
* **Ambient Temperature & Humidity:** High-precision digital temperature sensor
* **Barometric Pressure & Altitude:** Digital Barometer / Altimeter sensor
* **Wind / Anemometry:** Solid-state ultrasonic wind velocity sensor
* **Vibration & Dynamics:** 6-DoF IMU (Tri-Axial Accelerometer & Gyroscope) for RMS vibration logging
* **Board & Core Thermistors:** Multi-point internal temperature probes (Jetson CPU/GPU, PDB, and Enclosure cavity)

### C. Compute, Actuation & Mitigation
* **Edge Processing:** NVIDIA Jetson Orin NX (AI Vision, Bayesian EKF Fusion, Gain Scheduling)
* **Gimbal Actuation:** High-torque brushless motors driven by SimpleFOC Mini drivers via SPI/PWM
* **Power & Battery:** Daly Smart BMS (4S 100A), Aatral Sodium-Ion / LiFePO4 cold-resilient cells, regulated PDB
* **Thermal Hardware:** Stego PTC Heater 028, custom 150x150mm silicone thermal isolation barrier
* **Telemetry & Communications:** Microchip RN2903 LoRa Mesh (AES-256 encrypted telemetry)
* **Countermeasure:** Multi-band directional RF jammer (2.4 GHz, 5.8 GHz, GNSS L1/L2)

### D. Software & C2 Stack
* **Backend:** Python 3.10+, FastAPI, C++ (Low-level sensor drivers & SimpleFOC interface)
* **Algorithms:** Extended Kalman Filter (EKF), Adaptive PID Gain-Scheduling, YOLOv8 TensorRT
* **Frontend:** Tactical C2 Web Dashboard (React, TailwindCSS, WebSocket real-time telemetry)

## 6. Architecture
    =======================================================================
          PHASE 1: DATA ACQUISITION & SENSING (INPUT LAYER)
    =======================================================================
 
    [ Environmental & Health Telemetry ]        [ Target Detection Suite ]
    │                                           │
    ├─ Atmospheric (BME280/Anemometer)          ├─ Primary: 77GHz FMCW Radar
    │   └─ Temp, Pressure, Wind Speed           │   └─ Range, Velocity, Bearing
    │                                           │
    ├─ Structural (BMI088 IMU)                  ├─ Secondary: FLIR LWIR Thermal
    │   └─ Tri-axial RMS Vibration              │   └─ Heat Signature & Plume
    │                                           │
    └─ Internal Health (INA219/Thermistors)     ├─ Tertiary: EO Optical Camera
      └─ Bus Voltage, Core Temperatures       │   └─ Visual ID & Classification
                                              │
                                              └─ Passive: SDR / Acoustic
                                                  └─ RF Links, Motor Harmonics

                          │               │
                          └───────┬───────┘
                                  │ (Raw Data via SPI / I2C / UART)
                                  ↓

    =======================================================================
      PHASE 2: EDGE COMPUTE & FUSION (NVIDIA JETSON ORIN NX)
    =======================================================================

                            [ ORIN NX CORE ]
                                    │
    ┌───────────────────────────────┼───────────────────────────────┐
    │                               │                               │
    ↓                               ↓                               ↓
    [ Thermal Manager ]       [ Fusion Engine ]             [ Flight Controller ]
    │                               │                               │
    ├─ Monitors CPU/GPU temp        ├─ Extended Kalman Filter       ├─ Analyzes IMU vibration
    │                               │                               │
    └─ Triggers Stego PTC Heater    ├─ Bayesian Covariance Check    └─ Adaptive PID Scheduler
     if temp drops < thresholds   │  (Deprioritizes camera if     │  (Calculates new Kp, Ki, 
                                  │   whiteout/fog detected)      │   Kd gains for wind load)
                                  │                               │
                                  └─ YOLOv8 Target Classifier     │
                                     (Identifies drone frame)     │

                          │               │
                          └───────┬───────┘
                                  │ (Processed Commands & Telemetry)
                                  ↓

    =======================================================================
        PHASE 3: ACTUATION, C2, & MITIGATION (OUTPUT LAYER)
    =======================================================================

    ┌───────────────────────────────┼───────────────────────────────┐
    │                               │                               │
    ↓                               ↓                               ↓
    [ Physical Tracking ]       [ C2 Dashboard ]              [ Neutralization ]
    │                               │                               │
    ├─ SPI to SimpleFOC Drivers     ├─ Encrypted via LoRa Mesh      ├─ Target locked in UI
    │                               │                               │
    └─ Brushless Motors engage      ├─ Renders 2D Polar Plot        ├─ Operator Authorization
    │                               │                               │
    └─ Gimbal slews to target       └─ Displays System Health       └─ RF Jammer Deployed
     (Maintains <0.5° error)                                         (2.4/5.8GHz & GNSS)
                                                                  │
                                                                  └─ Threat reaches Fail-Safe
## 7. Youtube Video Link 
https://youtu.be/b6JOycQUBH8?si=EvOCV6wmlhk4iRvX
## 8. PPT Drive Link
https://drive.google.com/file/d/1l5IjpKKgdHX2Ls4RDt3SS1CB12fKvXPa/view?usp=drivesdk

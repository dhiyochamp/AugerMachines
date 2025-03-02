# 🚀 Advanced Dynamic Auger Filling Machine  
*Integrated Samkoon HMI (SK Series) + PLC | 200KHz Stepper Control | GT Macro Automation*  
**Version 2.1.5** | *Last Updated: Oct 2023*

---

## 📜 Table of Contents
1. [System Overview](#-system-overview)
2. [Hardware Configuration](#-hardware-configuration)
3. [I/O Mapping](#-io-mapping)
4. [Ladder Logic Design](#-ladder-logic-design)
5. [GT Macro Automation](#-gt-macro-automation)
6. [HMI Screen Design](#-hmi-screen-design)
7. [Installation & Testing](#-installation--testing)
8. [Troubleshooting](#-troubleshooting)
9. [License & Support](#-license--support)

---

## 🧩 System Overview
![System Diagram](https://via.placeholder.com/1024x400.png?text=HMI+PLC+Auger+Architecture)  
*Integrates high-speed pulse control (200KHz), real-time weight feedback, and multi-screen HMI for precision filling.*

### Key Features
- **Core Control**  
  ✔️ Samkoon SK HMI-PLC Hybrid ✔️ Ladder Logic + GT Macros  
- **Precision**  
  ✔️ 200KHz Stepper Motor ✔️ HX711 Load Cell Auto-Stop  
- **Safety**  
  ✔️ Emergency Stop Circuit ✔️ Proximity Sensor Positioning  
- **UI/UX**  
  ✔️ 4 Dynamic Screens ✔️ Touch Controls ✔️ Live Data Visualization  

---

## 🔌 Hardware Configuration
| Component              | Specifications               | Interface Point |
|------------------------|------------------------------|-----------------|
| **HMI/PLC**            | Samkoon SK-070AE (7")        | X0-Y7, D Registers |
| **Stepper Motor**      | NEMA 23 | 200KHz Pulse    | PLS0 (Y7)       |
| **Weight Sensor**      | HX711 + 5kg Load Cell        | D100 (Analog)   |
| **Proximity Sensors**  | Omron E2B-M12 (24VDC)        | X4 (Position)   |
| **Emergency Stop**     | Schneider Electric XB4BS8445 | X8 (NC Circuit) |

---

## 🔧 I/O Mapping
| Function               | Input (X) | Output (Y) | Data Register |
|------------------------|-----------|------------|---------------|
| Start Machine          | X0        | Y0         | M100          |
| Emergency Stop         | X8        | All Y Reset| M120          |
| Weight Value (Analog)  | -         | -          | D100          |
| Stepper Pulse (200KHz) | -         | Y7 (PLS0)  | D8150 (Speed) |

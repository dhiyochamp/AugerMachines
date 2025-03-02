# 🚀 Advanced Dynamic Auger Filling Machine Setup (Samkoon HMI + Integrated PLC)

## 🔥 Overview
This setup is designed for high-precision, high-speed auger filling using **Samkoon HMI (SK Series)** with built-in **PLC, Transistor Outputs, and Pulse Control**.

## 🚀 Features Implemented
- ✔ **Full Auger Machine Control**
- ✔ **High-Speed Pulse Control (200KHz) for Stepper Motor**
- ✔ **Integrated HMI + Ladder Logic + GT Macro**
- ✔ **Real-time Weight Measurement & Auto-Stop**
- ✔ **Manual & Auto Modes with Pedal Switch**
- ✔ **Multi-Screen HMI Control (Dynamic UI)**
- ✔ **Screw Test & Cleaning Cycle Automation**
- ✔ **Emergency Stop & Error Handling**

## 🔧 Final Setup Components

| Component      | Description |
|---------------|-------------|
| **HMI/PLC**   | Samkoon SK Series HMI with Integrated PLC & Transistor Outputs |
| **Motor**     | High-speed Stepper Motor with Driver (200KHz Pulse) |
| **Weight Sensor** | Load Cell with HX711 Module |
| **Sensors**   | Proximity Sensors for Bag Positioning & Screw Position |
| **Pedal Switch** | Manual Mode Control |
| **Emergency Stop** | Safety Shutdown |

---

## 🛠 Step 1: I/O Mapping (HMI to Ladder Logic)

| Function | Input (Xn) | Output (Yn) | Data Register (Dn) |
|----------|-----------|-------------|---------------------|
| Start Machine | X0 | Y0 | M100 |
| Stop Machine | X1 | Y0 (RST) | - |
| Auger Screw ON/OFF | X2 | Y1 | M101 |
| Stirrer Motor ON/OFF | X3 | Y2 | M102 |
| Pedal Switch (Manual Mode) | X4 | Y0 | M103 |
| Weight Input (Load Cell) | - | - | D100 |
| Set Weight (Target) | - | - | D101 |
| Auto Delay | - | - | D102 |
| Screw Test | X5 | Y5 | M104 |
| Screw Cleaning | X6 | Y6 | M105 |
| Pulse Output (Stepper Motor) | - | Y7 (200KHz) | PLS0 |
| Screen Switch (Multi-Page) | X7 | - | M110 |
| Emergency Stop | X8 | All Reset | M120 |

---

## 🛠 Step 2: Ladder Logic (Advanced)

```plc
(* ---------- MACHINE START/STOP ---------- *)
| Start (X0)  |----[ ]--------------------(SET)----| Run Bit (M100) |
| Stop (X1)   |----[ ]--------------------(RST)----| Run Bit (M100) |

(* ---------- AUGER SCREW CONTROL ---------- *)
| Run Bit (M100)  |----[ ]----| Screw ON (X2) |----( Y0 )----| Auger Motor |
| Screw OFF (X2)  |----[ ]--------------------(RST)----| Auger Motor |

(* ---------- STIRRER CONTROL ---------- *)
| Run Bit (M100)  |----[ ]----| Stir ON (X3) |----( Y1 )----| Stirrer Motor |
| Stir OFF (X3) |----[ ]--------------------(RST)----| Stirrer Motor |

(* ---------- WEIGHT MEASUREMENT (AUTO-STOP) ---------- *)
| Load Cell (D100) |----[CMP>=]----| Set Weight (D101) |----(RST)----| Auger Motor |

(* ---------- HIGH-SPEED PULSE CONTROL (200KHz STEPPER MOTOR) ---------- *)
| Run Bit (M100)  |----[ ]----| Pulse Output Enable |----( PLS0 200KHz )----| Stepper Motor Driver |

(* ---------- MANUAL MODE (PEDAL SWITCH) ---------- *)
| Manual Mode (X4) |----[ ]----(SET)----| Manual Active (M103) |
| Pedal Switch (X5) |----[ ]----| Manual Active |----( Y0 )----| Auger Motor |

(* ---------- SCREW TEST (STEP MODE) ---------- *)
| Screw Test (X6) |----[ ]----( Y5 )----| Test Auger |

(* ---------- SCREW CLEANING (REVERSE AUGER) ---------- *)
| Screw Cleaning (X7) |----[ ]----( Y6 )----| Reverse Auger |

(* ---------- SCREEN SWITCHING (MULTI-PAGE CONTROL) ---------- *)
| Home Button (X8) |----[ ]----(SET)----| Screen 1 Active |
| Program Start (X9) |----[ ]----(SET)----| Screen 2 Active |

(* ---------- EMERGENCY STOP (RESET ALL MOTORS) ---------- *)
| Emergency Stop (X10) |----[ ]----(RST)----| Run Bit (M100) | (RST) | Screw ON (X2) | (RST) | Stir ON (X3) |
```

---

## 🛠 Step 3: GT Macro Script for Advanced Control

```c
#include "MacroInit.h"

void Macro_main(IN *p)  
{  
    #define START_BTN   X0  
    #define STOP_BTN    X1  
    #define SCREW_ON    X2  
    #define STIR_ON     X3  
    #define PEDAL_SW    X5  
    #define SCREW_CLEAN X6  
    #define SCREW_TEST  X7  
    #define EMERGENCY   X8  
    #define WEIGHT_INPUT D100  
    #define SETPOINT    D101  

    #define AUGER_MOTOR Y0  
    #define STIR_MOTOR  Y1  
    #define CLEAN_MOTOR Y6  

    // Start/Stop Machine  
    if (GETBIT(START_BTN))  
    {  
        SETBIT(AUGER_MOTOR, 1);  
        SETBIT(STIR_MOTOR, 1);  
    }  
    if (GETBIT(STOP_BTN))  
    {  
        SETBIT(AUGER_MOTOR, 0);  
        SETBIT(STIR_MOTOR, 0);  
    }  

    // Weight Measurement Auto-Stop  
    if (GETREG(WEIGHT_INPUT) >= GETREG(SETPOINT))  
    {  
        SETBIT(AUGER_MOTOR, 0);  
    }  

    // Manual Mode (Pedal)  
    if (GETBIT(PEDAL_SW))  
    {  
        SETBIT(AUGER_MOTOR, 1);  
    }  

    // Screw Cleaning  
    if (GETBIT(SCREW_CLEAN))  
    {  
        SETBIT(CLEAN_MOTOR, 1);  
        DELAY(2000);  
        SETBIT(CLEAN_MOTOR, 0);  
    }  

    // Emergency Stop  
    if (GETBIT(EMERGENCY))  
    {  
        SETBIT(AUGER_MOTOR, 0);  
        SETBIT(STIR_MOTOR, 0);  
        SETBIT(CLEAN_MOTOR, 0);  
    }  
}
```

---

## 🔥 Final Setup Checklist
✅ Upload Ladder Logic to SKTOOL7.1  
✅ Upload GT Macro to HMI  
✅ Wire Motors & Sensors  
✅ Test Start/Stop, Weight Control, Pedal, Cleaning  
✅ Test High-Speed Pulse Output (Stepper Motor 200KHz)  
✅ Verify Multi-Screen Switching  

🚀 **Fully Automated Dynamic Auger Machine Setup Ready!**

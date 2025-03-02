# Auger Filling Machine PLC Control System

## Overview
This document provides comprehensive documentation for the PLC ladder logic implementation of an automated auger filling machine. The system supports both automatic and manual operation modes with built-in safety features and specialized functions.

## Features

### Core Functions
- **Machine Control**: Start/stop the entire machine system
- **Auger Screw Control**: Run and stop the auger motor with dedicated controls
- **Stirrer Control**: Operate the stirrer motor with continuous operation option
- **Weight Measurement**: Automatically stop filling when target weight is reached
- **Dual-Mode Operation**: Switch between manual and automatic operation
- **Pedal Switch**: Manual override for auger operation in manual mode
- **Maintenance Functions**: Screw cleaning (reverse operation) and test functions
- **Emergency Stop**: Immediate shutdown of all operations

### Logic Implementation

```
(* ---------------- START/STOP MACHINE ---------------- *)
| Start Button (X0) |----[ ]--------------------(SET)----| Run Bit (M0) |
| Stop Button (X1) |----[ ]--------------------(RST)----| Run Bit (M0) |

(* ---------------- AUGER SCREW CONTROL ---------------- *)
| Run Bit (M0) |----[ ]----| Screw ON (M1) |----( Y0 )----| Auger Motor |
| Screw OFF (X2) |----[ ]--------------------(RST)----| Auger Motor |

(* ---------------- STIRRER CONTROL ---------------- *)
| Run Bit (M0) |----[ ]----| Stir ON (M2) |----( Y1 )----| Stirrer Motor |
| Stir OFF (X3) |----[ ]--------------------(RST)----| Stirrer Motor |
| Keep Stirring (X4) |----[ ]----(SET)----| Stirrer Motor |

(* ---------------- WEIGHT MEASUREMENT ---------------- *)
| Load Cell Input (D0) |----[CMP>=]----| Set Weight (D1) |----(RST)----| Auger Motor |

(* ---------------- MODE SELECTION ---------------- *)
| Manual Mode (X5) |----[ ]--------------------(SET)----| Manual Control Active (M4) |
| Auto Mode (X6) |----[ ]--------------------(RST)----| Manual Control Active (M4) |

(* ---------------- PEDAL SWITCH CONTROL (Manual Mode) ---------------- *)
| Pedal Switch (X7) |----[ ]----| Manual Mode (M4) |----( Y0 )----| Auger Motor |

(* ---------------- SCREW CLEANING PROCESS ---------------- *)
| Screw Clean Button (X8) |----[ ]----( Y3 )----| Reverse Auger Motor |

(* ---------------- SCREW TEST FUNCTION ---------------- *)
| Screw Test (X9) |----[ ]----( Y4 )----| Test Auger Motor |

(* ---------------- EMERGENCY STOP ---------------- *)
| Emergency Stop (X10) |----[ ]----(RST)----| Run Bit (M0) |
                                   (RST)----| Screw ON (M1) |
                                   (RST)----| Stir ON (M2) |
```

## I/O Configuration

### Inputs
| Input | Address | Description |
|-------|---------|-------------|
| Start Button | X0 | Starts the machine |
| Stop Button | X1 | Stops the machine |
| Screw OFF | X2 | Manually stops the auger |
| Stir OFF | X3 | Manually stops the stirrer |
| Keep Stirring | X4 | Maintains stirrer operation |
| Manual Mode | X5 | Activates manual mode |
| Auto Mode | X6 | Activates automatic mode |
| Pedal Switch | X7 | Activates auger in manual mode |
| Screw Clean | X8 | Activates reverse auger operation |
| Screw Test | X9 | Tests auger motor |
| Emergency Stop | X10 | Immediately stops all operations |

### Outputs
| Output | Address | Description |
|--------|---------|-------------|
| Auger Motor | Y0 | Controls the auger motor |
| Stirrer Motor | Y1 | Controls the stirrer motor |
| Reverse Auger | Y3 | Runs auger in reverse |
| Test Auger | Y4 | Test function for auger |

### Memory Addresses
| Memory | Address | Description |
|--------|---------|-------------|
| Run Bit | M0 | Machine running status |
| Screw ON | M1 | Auger control flag |
| Stir ON | M2 | Stirrer control flag |
| Manual Control | M4 | Manual mode active flag |
| Weight Value | D0 | Current weight from load cell |
| Target Weight | D1 | Preset target weight |

## Installation Guide

1. Program this ladder logic into your PLC (compatible with Samkoon, Delta, Mitsubishi, and most standard PLCs)
2. Connect the HMI buttons to the PLC memory addresses (M, X, Y, D)
3. Configure the weight sensor (Load Cell) to provide input to D0
4. Set the target weight in D1 register
5. Test all functions systematically:
   - Start/stop operation
   - Auger and stirrer control
   - Weight measurement auto-stop
   - Manual mode with pedal operation
   - Maintenance functions
   - Emergency stop system

## Operation Guide

### Automatic Mode
1. Ensure machine is connected to power
2. Press Start Button (X0) to initialize the system
3. Set target weight in register D1
4. Select Auto Mode (X6)
5. The system will:
   - Run the auger to dispense product
   - Automatically stop when target weight is reached
   - Maintain stirrer operation if Keep Stirring (X4) is activated

### Manual Mode
1. Press Start Button (X0) to initialize the system
2. Select Manual Mode (X5)
3. Use the Pedal Switch (X7) to control the auger operation
4. Control the stirrer independently using the Stir ON/OFF buttons

### Maintenance
1. Use Screw Clean Button (X8) for reverse auger operation to clear blockages
2. Use Screw Test (X9) to check auger functionality

## Safety Features

- Emergency Stop (X10) immediately halts all operations
- Independent control of auger and stirrer
- Automatic weight-based stopping to prevent overfilling
- Manual override capabilities for troubleshooting

## Troubleshooting

| Issue | Possible Cause | Solution |
|-------|---------------|----------|
| Machine doesn't start | Emergency stop engaged | Reset emergency stop |
| | Power issue | Check power connections |
| Auger not running | Machine not started | Press Start Button |
| | Manual mode not active | Check mode selection |
| Weight not detected | Load cell configuration | Verify load cell settings |
| | Connection issue | Check load cell wiring |
| Automatic stop not working | Weight comparison | Verify D0 and D1 values |

## System Customization

The ladder logic can be modified to accommodate:
- Additional sensors
- Different motor types
- Multiple filling stations
- Advanced weight processing
- Custom HMI integration
- Data logging capabilities

---

For additional support or custom GT Script version, please contact technical support.

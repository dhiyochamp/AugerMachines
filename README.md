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
# Auger Filling Machine HMI Project

## Overview
This SKTOOLS (SK Series HMI Development Software) project provides a complete human-machine interface for controlling and monitoring an auger filling machine. The HMI is designed to work with the PLC ladder logic previously configured and provides intuitive controls for operators while maintaining industrial design standards.

## Project Structure

### Main Screens
1. **Home/Dashboard** - Main control panel with operational overview
2. **Operation Control** - Detailed machine control interface
3. **Settings** - Parameters configuration screen
4. **Alarms** - Active and historical alarms
5. **Data Logging** - Production data and reports

## Design Guidelines

### Recommended Theme: Industrial Blue
The Industrial Blue theme provides excellent contrast and visibility in factory environments while reducing eye strain during extended operation.

#### Color Palette
- Primary: #005b96 (Deep Blue)
- Secondary: #6497b1 (Medium Blue)
- Accent: #b3cde0 (Light Blue)
- Warning: #ffb347 (Amber)
- Alarm: #ff6b6b (Red)
- Background: #f0f0f0 (Light Gray)
- Text: #333333 (Dark Gray)

#### Typography
- Headers: Arial Bold, 16pt
- Labels: Arial, 12pt
- Values: Arial Bold, 14pt
- Units: Arial Italic, 10pt

### Header Design
Implement a consistent header across all screens with the following elements:

```
+-----------------------------------------------------------------------+
| [LOGO] AUGER FILLING MACHINE CONTROL                   [DATE] [TIME]  |
| [HOME] [OPERATION] [SETTINGS] [ALARMS] [DATA]     [USER] [LOGOUT]     |
+-----------------------------------------------------------------------+
```

#### Header Components
- **Company Logo**: Place in top-left corner (recommended size: 120×60 pixels)
- **Screen Title**: Centered, bold, 18pt
- **Date/Time**: Top-right corner with updating time display
- **Navigation Bar**: Button row for main screens
- **User Info**: Current logged-in user and logout button

### Footer Design
Implement a consistent footer with status indicators and common controls:

```
+-----------------------------------------------------------------------+
| [STATUS INDICATORS]         [MESSAGE DISPLAY]         [SYSTEM INFO]   |
+-----------------------------------------------------------------------+
```

#### Footer Components
- **Status Indicators**: 
  - System status light (Green/Yellow/Red)
  - Network connection status
  - PLC communication status
- **Message Display**: Scrolling text area for system messages
- **System Info**: PLC mode (RUN/STOP), HMI firmware version

## Animation Strips & Dynamic Elements

### Recommended Animations
1. **Process Visualization Strip**
   - Location: Upper section of Operation screen
   - Design: Animated horizontal strip showing material flow through the system
   - States: Different animations for running, stopped, and error states
   - Implementation: Use frame animation with 5-8 frames at 500ms intervals

2. **Loading Indicator**
   - Type: Circular progress bar
   - Usage: During filling operations to show percentage completion
   - Colors: Blue progress on gray background

3. **Weight Accumulation Animation**
   - Type: Vertical bar graph with numeric display
   - Features: Rising fill level with color changes at different thresholds
   - Thresholds: <80% (Green), 80-95% (Yellow), 95-100% (Green), >100% (Red)

4. **Motor Status Animation**
   - Type: Rotating icon for auger and stirrer motors
   - States: Static when off, rotating when on
   - Speed: Animation speed proportional to motor speed (if variable speed control implemented)

5. **Mode Transition Effects**
   - Subtle slide effects when switching between Manual/Auto modes
   - 300ms transition time with ease-in-out timing function

### Implementation in SKTOOLS
For each animation:
1. Create frame-by-frame images in your preferred image editor
2. Import all frames to SKTOOLS image library
3. Use the "Animation" widget and link to appropriate PLC addresses
4. Set the frame change conditions based on PLC values
5. Adjust timing for smooth visual feedback

## Screen-by-Screen Implementation

### 1. Home/Dashboard Screen
- **Top Section**: Process visualization animation strip
- **Center**: Large weight display and target weight input
- **Below Center**: Primary control buttons (Start, Stop, Emergency Stop)
- **Bottom Section**: Key status indicators and mode selection

### 2. Operation Control Screen
- **Left Panel**: All motor controls with status animations
- **Center**: Weight control and visualization
- **Right Panel**: Operational parameters and adjustments

### 3. Settings Screen
- **Access**: Password protected (supervisor level)
- **Calibration**: Weight sensor calibration interface
- **Parameters**: Timing, motor speeds, thresholds
- **System**: Communication settings, backups

### 4. Alarms Screen
- **Active Alarms**: List with timestamp, description, and acknowledgment
- **Historical**: Searchable log of past alarms
- **Filtering**: Options to filter by type and time period

### 5. Data Logging Screen
- **Production Data**: Batches, weights, timestamps
- **Charts**: Daily/weekly production visualization
- **Export**: Buttons to export data to USB

## PLC Address Mapping

| HMI Element | PLC Address | Description |
|-------------|-------------|-------------|
| Start Button | X0 | Start machine operation |
| Stop Button | X1 | Stop machine operation |
| Screw ON/OFF | M1/X2 | Control auger screw |
| Stir ON/OFF | M2/X3 | Control stirrer |
| Keep Stirring | X4 | Maintain stirrer operation |
| Manual Mode | X5 | Switch to manual mode |
| Auto Mode | X6 | Switch to automatic mode |
| Screw Clean | X8 | Clean auger screw |
| Screw Test | X9 | Test auger screw |
| Emergency Stop | X10 | Emergency stop button |
| Weight Display | D0 | Current weight value |
| Target Weight | D1 | Target weight setting |
| Machine Status | M0 | Running status indicator |
| Auger Motor | Y0 | Auger motor status |
| Stirrer Motor | Y1 | Stirrer motor status |
| Mode Status | M4 | Current operating mode |

## Testing & Deployment Checklist

### Simulation Testing
- [ ] All buttons trigger correct PLC addresses
- [ ] Animations respond correctly to state changes
- [ ] Numeric inputs accept valid ranges
- [ ] Screen transitions work as expected

### Physical HMI Testing
- [ ] Download project to HMI device
- [ ] Verify communication with PLC
- [ ] Test all functions systematically
- [ ] Verify animations and visual elements
- [ ] Check response times

### Operational Testing
- [ ] Test complete filling cycle in Auto mode
- [ ] Verify all manual controls
- [ ] Trigger test alarms to verify response
- [ ] Check data logging functionality

## User Access Levels

| Level | Name | Access |
|-------|------|--------|
| 0 | Operator | Basic operations, view data |
| 1 | Supervisor | Change parameters, acknowledge alarms |
| 2 | Maintenance | Calibration, diagnostics |
| 3 | Administrator | Full system access |

## Troubleshooting

Common issues and solutions:
- **Communication Errors**: Verify cable connections and COM settings
- **Slow Response**: Check animation frame rates and simplify if needed
- **Touch Calibration**: Access system menu for recalibration
- **Weight Discrepancies**: Run calibration procedure in Settings

## Future Enhancements
- Remote monitoring capability
- Production scheduling interface
- Preventative maintenance tracking
- OEE (Overall Equipment Effectiveness) calculations
- Multiple language support

---

© 2025 Your Company Name. All rights reserved.

# System Architecture  
### Smart Embedded Control & Sensing Platform

This document provides a detailed breakdown of the system architecture, functional blocks, signal flow, and PCB-level design considerations for the controller board.

---

# 1. Top-Level Architecture

The system is organized into the following major subsystems:

1. **Processing Core (STM32F407)**  
2. **Onboard Debugger (STM32F103, ST-Link equivalent)**  
3. **Communication Interfaces**  
4. **Precision Analog Measurement Front-End**  
5. **Dual Motor Control Subsystem**  
6. **Audio Subsystem**  
7. **Power Supply & Protection**  
8. **Mechanical & PCB Architecture**

---

# 2. Processing Core

### 2.1 STM32F407 MCU (Cortex-M4F)
- 168 MHz CPU  
- DSP + Floating Point Unit  
- 1 MB Flash, 192 KB SRAM  
- Native interfaces: USB OTG, Ethernet MAC, CAN, SPI, UART, I²C  

### 2.2 External Components
- 8 MHz crystal  
- Boot configuration resistors  
- SWD/JTAG header  
- Decoupling network (multi-capacitor: 100nF, 4.7µF, bulk)

---

# 3. On-Board Debugger (Programmer)

### 3.1 STM32F103-based ST-Link  
- USB-based debugging  
- Supports SWD programming  
- Connects to STM32F407 via SWD lines  
- ESD-protected USB port  

---

# 4. Communication Interfaces

## 4.1 Ethernet  
- PHY: DP838426IRHBR 
- RJ45 with integrated magnetics  
- Ethernet link/activity LEDs

## 4.2 USB 2.0  
- USB FS/HS depending on OTG configuration  
- ESD protection & common-mode filtering  
- Micro-USB or USB-C connector  

## 4.3 CAN Bus  
- MCU CAN peripheral  
- External CAN transceiver (SN65HVD230 / TJA1050)  
- Termination resistor option  

## 4.4 UARTs  
- 3.3V TTL UART  
- Expandable via header pins  

---

# 5. Precision Analog Measurement Subsystem

## 5.1 ADS122C04 24-bit ADC  
- Differential measurement: 0–10 mV  
- Common-mode voltage: 2.5V  
- PGA up to ×128  
- I²C communication  
- External precision reference (2.5V)  
- External crystal/clock input  

---

# 6. Motor Control Subsystem

## 6.1 DRV8701 Gate Driver  
- Provides FET gate control  
- Current regulation & protection  
- H-bridge configuration using N-channel MOSFETs  

## 6.2 MOSFET H-Bridge  
- Handles up to 12V / 3A per motor  
- Thermal pads + copper pour heatsinking  
- Flyback diode protection (integrated/external)

## 6.3 PWM Control  
- PWM input from STM32 timers  
- Supports 0–100% duty cycle  
- Recommended PWM frequency: 20–30 kHz  

---

# 7. Audio Subsystem

## 7.1 DAC + Amplifiers
- I²C-configurable DAC  
- Headphone amplifier  
- Speaker amplifier  
- Low-pass reconstruction filters  

## 7.2 Microphone Interface  
- Bias resistor for electret or MEMS mic  
- AC coupling  
- EMI filtering  
- Routed to codec input  

---

# 8. Power Architecture

## 8.1 Input Power  
- 12V DC input  
- Reverse polarity protection  
- TVS surge suppression  

## 8.2 Power Rails  
- Buck converters: 12V or 5V / 3.3V  
- Low-noise LDOs for analog & audio  
- Earth, DGND and AGND separation  

## 8.3 Power Distribution Strategy  
- Dedicated motor power domain  
- Analog reference isolation  
- Short, wide traces for high-current routing  

---

# 9. PCB Architecture

## 9.1 Layer Stack (4-layer)
- **L1:** High-speed & signals  
- **L2:** Continuous ground plane  
- **L3:** Power distribution  
- **L4:** Mixed I/O & slow signals  

## 9.2 Layout Guidelines
- Differential pair routing for USB & Ethernet  
- Keep AFE isolated from switching nodes  
- Thermal vias under MOSFETs & regulators  
- Ground stitching vias for noise reduction  

---

This architecture serves as a solid demonstration of modern embedded hardware design practices and system-level integration.


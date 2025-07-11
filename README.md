# Micro-no Board (µNO Board)- An Open Source Arduino-Compatible Development Platform

Welcome to the official documentation for the **µNO Board** (pronounced "Micro-no Board"). This open-source project presents a minimalistic, hardware-level implementation of the popular Arduino Uno, centered around the **ATmega328P-PU** microcontroller. Engineered and documented by **Abhishek Tomar**, this project is a comprehensive platform for electronics students and embedded systems learners seeking to understand the internal architecture and design of a microcontroller development board.

---

## 📚 Table of Contents

1. [Project Overview](#project-overview)
2. [Technical Features](#technical-features)
3. [Detailed Schematic Breakdown](#detailed-schematic-breakdown)

   * [Power Supply Circuit](#power-supply-circuit)
   * [Microcontroller Core](#microcontroller-core)
   * [Oscillator Configuration](#oscillator-configuration)
   * [Reset Mechanism](#reset-mechanism)
   * [FTDI Serial Programming Header](#ftdi-serial-programming-header)
   * [ISP Bootloader Interface](#isp-bootloader-interface)
   * [Status LED Indicator](#status-led-indicator)
4. [Arduino to ATmega328P Pin Mapping](#arduino-to-atmega328p-pin-mapping)
5. [Burning the Bootloader](#burning-the-bootloader)
6. [Uploading Programs via FTDI](#uploading-programs-via-ftdi)
7. [Understanding ISP and FTDI Protocols](#understanding-isp-and-ftdi-protocols)
8. [Accessing Design Files](#accessing-design-files)
9. [Contributions and Collaboration](#contributions-and-collaboration)
10. [License](#license)

---

## 🔍 Project Overview

The **µNO Board** replicates the functionality of the Arduino Uno but strips it to its essentials. Rather than relying on a premade Arduino module, this project demonstrates the discrete components necessary to build a fully functional development board from the ground up:

* Power regulation and filtering
* Stable clock generation
* GPIO and ADC decoupling
* Reset and auto-reset mechanisms
* Programming via serial (FTDI) or SPI (ISP)

It serves as both a learning tool and a production-ready embedded platform.

---

## ⚙️ Technical Features

* **Microcontroller**: ATmega328P-PU (8-bit AVR RISC architecture)
* **Voltage Regulation**: AMS1117-5.0V LDO regulator
* **System Clock**: External 16 MHz crystal
* **Programming Interfaces**: FTDI (UART) and ISP (SPI)
* **Manual Reset Button**: Physical switch for hardware resets
* **Input Voltage Range**: 6V to 15V (via VIN pin)
* **Regulated Output**: +5V for MCU and peripherals
* **GPIO Access**: Digital pins D0-D13, Analog pins A0-A5
* **Status Indicator**: Onboard LED that Indicates power ON/OFF

---

## 🔧 Detailed Schematic Breakdown

### 🔌 Power Supply Circuit

* **AMS1117-5.0V Regulator (U2)**

  * Converts input voltage (up to 15V) to 5V regulated output.
  * Essential for powering the MCU and peripherals.
* **Input/Output Capacitors**:

  * **C7, C8 (10uF)**: Stabilize input (VIN) and output (VOUT).
  * Filter transient fluctuations and reduce switching noise.

### 🧠 Microcontroller Core (ATmega328P)

* **Power Pins**:

  * **VCC (Pin 7)**: Main power input
  * **AVCC (Pin 20)**: Power supply for the ADC block
  * **GND (Pins 8 & 22)**: Common ground
* **Bypass Capacitors**:

  * **C3, C4, C6 (100nF)**: Placed near VCC and AVCC to suppress high-frequency noise.

### ⏱️ Oscillator Configuration

* **Crystal Oscillator (Y1, 16 MHz)**

  * External clock source for the MCU, ensures stable system timing
* **Load Capacitors (C1, C2 = 22pF)**

  * Ensure proper startup and oscillation of the crystal

### 🔁 Reset Mechanism

* **R1 (10kΩ)**: Pull-up resistor on RESET pin
* **SW1**: Push-button reset (pulls RESET to GND when pressed)
* **C5 (100nF)**: Coupled with FTDI DTR pin for auto-reset during code upload

### 🔄 FTDI Serial Programming Header (J5)

* **6-pin Header**: GND, VCC, RX, TX, DTR, CTS
* **Function**: Enables uploading sketches via UART after bootloader is flashed
* **Connection Tip**: Ensure TX (FTDI) connects to RX (MCU) and vice versa

### 💾 ISP Bootloader Interface (J1)

* **Standard 6-pin ISP Header**

  * MISO, MOSI, SCK, RESET, VCC, GND
* **Purpose**: Used for flashing the bootloader or burning hex files directly via SPI



---

## 🧩 Arduino to ATmega328P Pin Mapping

| Arduino | ATmega328P Pin | Port    | Description      |
| ------- | -------------- | ------- | ---------------- |
| D0      | 2              | PD0     | UART RX          |
| D1      | 3              | PD1     | UART TX          |
| D2-D7   | 4-9            | PD2-PD7 | Digital I/O      |
| D8-D13  | 14-19          | PB0-PB5 | SPI & I/O        |
| A0-A5   | 23-28          | PC0-PC5 | ADC & I2C        |
| RESET   | 1              | PC6     | System Reset     |
| AREF    | 21             | -       | Analog Reference |

---

## 🔥 Burning the Bootloader

### 💡 What is a Bootloader?

A bootloader is a small program that resides in the MCU’s flash memory, allowing firmware to be uploaded via UART rather than ISP.

### 🔧 Required Tools

* USBasp Programmer or Arduino-as-ISP

### 🧭 Step-by-Step Guide

1. Connect the ISP header on the board to your programmer.
2. Launch Arduino IDE.
3. Go to **Tools > Board > Arduino Uno**.
4. Under **Programmer**, select **USBasp** (or Arduino as ISP).
5. Click **Burn Bootloader**.

Once burned, your microcontroller can now be programmed through FTDI.

---

## ⬆️ Uploading Programs via FTDI

### 🛠️ Required Hardware

* FTDI USB-to-Serial Converter

### Wiring:

* FTDI TX → MCU RX
* FTDI RX → MCU TX
* DTR → RESET (via 100nF capacitor)
* VCC, GND connected properly

### Upload Steps:

1. Open Arduino IDE and select **Arduino Uno** as the board.
2. Choose the correct **COM Port**.
3. Click **Upload**.

The auto-reset circuit will handle entry into the bootloader automatically.

---

## 🧠 Understanding ISP and FTDI Protocols

### ISP (In-System Programming)

* **Protocol**: SPI-based (MOSI, MISO, SCK)
* **Use Case**: Burning bootloaders or uploading compiled firmware directly
* **Requires**: External programmer

### FTDI (USB-to-UART)

* **Protocol**: UART-based (TX/RX)
* **Use Case**: Uploading code to bootloaded MCUs
* **Advantage**: No extra hardware once bootloader is installed

---
## 🔮 Future Improvements and Expansion Ideas

As a learning platform and minimal Arduino-compatible board, the µNO Board has room for useful upgrades. Here are some potential enhancements that could improve its functionality, usability, and professional quality:

### 🔧 Hardware Improvements

- **⚡ Onboard USB-to-Serial (FTDI)**  
  Add a built-in CH340 or CP2102 IC for USB programming, eliminating the need for an external FTDI adapter.

- **🔋 3.3V Voltage Regulator**  
  Add an LDO regulator (e.g., AMS1117-3.3V) to support 3.3V sensors or modules on the same board.

- **🛡️ Reverse Polarity Protection**  
  Include a Schottky diode or P-MOSFET to protect the board from accidental reverse voltage input.

- **🧲 TVS Diodes or ESD Protection**  
  Protect sensitive GPIOs and the USB interface from electrostatic discharge or voltage spikes.

- **📦 Mounting Hole Grounding**  
  Connect metal mounting holes to GND for improved shielding and mechanical stability.

- **🧠 Additional GPIO Headers / Expansion Ports**  
  Break out I2C, SPI, ADC, and power lines to dedicated connectors for sensor/module expansion.


---

### 🖥️ Firmware & Bootloader Enhancements

- **⏱️ Optiboot Bootloader**  
  Use the smaller Optiboot variant to save flash memory and enable faster serial uploads (up to 115200 baud).

- **🧩 Arduino Board Package Support**  
  Create a custom board definition file (`boards.txt`) to add µNO as an official option in the Arduino IDE.

---

These enhancements can transform the µNO Board from a minimalist learning tool into a feature-rich, professional-grade embedded system. Explore, experiment, and evolve your design! 🚀

  
---

## 📁 Accessing Design Files

Your repository includes the following:

* `KiCad/`: Schematic and PCB design files
* `Gerber/`: Fabrication-ready output files
* `3D_Images/`: Visual renders of the board
* `README.md`: This documentation

📸 Board Images:

* ![Front View](3D_Images/Front.jpg)
* ![Back View](3D_Images/Back.jpg)
* ![PCB Traces](3D_Images/trace2.jpg)


---

## 🤝 Contributions and Collaboration

Contributions are welcome! Feel free to fork the repository, submit pull requests, or raise issues for bugs, suggestions, or improvements.

---

## 📜 License

This project is licensed under the **CERN-OHL-S v2** open hardware license. This ensures:

* Freedom to study, modify, manufacture, and distribute
* Requirement to share changes under the same license

---

❤️ Created with passion by Abhishek Tomar — Just a learner like you.

🌟 **Support This Project!**

If this project helped you learn, build, or get inspired, please ⭐ star the repository and share it!

Together, we make open hardware accessible and empowering for all. 💪✨

Happy Hacking 🚀

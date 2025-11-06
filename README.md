# Silent Digital Typewriter — HP45 Inkjet

A modern, silent typewriter that prints on plain paper using an HP45 inkjet cartridge.

## Features

- **Instant boot** (ESP32-S3 microcontroller)
- **USB keyboard input**
- **USB flash drive support** (save / load text files)
- **Plain text editor + print buffer**
- **Real ink-on-paper**, not thermal
- **Ultra-quiet motion** (NEMA-14 steppers + TMC drivers)
- **Dedicated HP45 inkjet controller** for microsecond firing pulses
- **Portable, battery powered**

This project explores a practical "digital typewriter" that behaves like a real appliance — no operating system, no laptop required.

## Motivation

Most modern "portable typing" is trapped behind screens. This device aims to be:

- Quiet enough for classrooms and libraries
- Lightweight and battery powered
- Compatible with standard plain paper
- Able to save work to a USB stick
- Not just a novelty — a real writing tool

## System Architecture

```
USB Keyboard → ESP32-S3 → Text Buffer → Rasterizer → HP45 Controller
                                                         ↓
                                               microSD / USB Flash (save files)

- Carriage Axis (NEMA-14 + TMC2209 + rail)
- Paper Feed Axis (NEMA-14 + TMC2209 + pinch rollers)
```

The ESP32-S3 provides instant-on firmware, a file system, and a small UI. The HP45 controller board manages all inkjet pulse timing and nozzle drive safely.

## Hardware Overview

- **MCU**: ESP32-S3 with USB-OTG (USB Host)
- **Inkjet**: HP45 cartridge + dedicated controller (serial protocol)
- **Carriage**: NEMA-14 stepper, GT2 belt, MGN7/MGN9 linear rail
- **Paper Feed**: NEMA-14 stepper + pinch rollers (optional encoder wheel)
- **Drivers**: 2× TMC2209 (silent / microstepping)
- **Storage**: USB flash + optional microSD
- **Power**: 2S Li-ion → buck 5V → 3.3V LDO, plus 18–24 V boost for HP45
- **UI**: Small OLED/TFT, a few buttons (PRINT/MODE/FEED)

## Firmware Features

- Reads USB keyboards (HID)
- Saves `.txt` files to USB flash
- Simple line buffer editing (typewriter mode / line mode)
- Prints lines as raster sweeps
- Silent stepper motion (StealthChop + low jerk)
- Autosave and document export

## Repository Structure (planned)

```
firmware/
├── esp32-s3/
│   ├── keyboard host
│   ├── filesystem (FatFs)
│   ├── rasterizer
│   ├── motion control
│   └── HP45 controller protocol

hardware/
├── main PCB (ESP32-S3, TMC drivers, power)
├── carriage module (CAD + BOM)
└── feed module (CAD + BOM)

docs/
├── theory of operation
├── serial protocol to HP45 controller
├── wiring diagrams
└── maintenance notes
```

## Status

- System architecture defined
- PCB + firmware development in progress
- Mechanical prototype work upcoming

Contributions, ideas, and experiments are welcome — pull requests encouraged!

## Goals (v1)

- Typewritten text printed cleanly on plain paper
- USB keyboard and flash drive working end-to-end
- Basic editor + print buffer
- Instant boot and safe power-off
- Battery powered, quiet operation

## Future Extensions

- BLE keyboard support
- Basic paragraph editor
- Font/size options
- Simple plotting / vector graphics
- Wireless export
- Encrypted notes mode

## License

TBD — MIT recommended for hardware + firmware openness.

## Contact / Discussion

Open an issue, start a discussion, or fork and experiment.

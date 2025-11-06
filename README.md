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
- **Serviceable and long-lived** design philosophy

This project explores a practical "digital typewriter" that behaves like a real appliance — no operating system, no laptop required.

## Motivation

**The Problem**: Traditional analog typewriters are too noisy and distracting to be used in a classroom. iPads and laptops are too distracting for both students and teachers/professors. Handwriting is sometimes not preferable.

**The Solution**: A silent, distraction-free, battery-powered typewriter with keyboard input for students who want to type in class or use a typewriter without the noise distraction.

Most modern "portable typing" is trapped behind screens. This device aims to be:

- Quiet enough for classrooms and libraries
- Lightweight and battery powered
- Compatible with standard plain paper
- Able to save work to a USB stick
- Serviceable and long-lived
- Not just a novelty — a real writing tool

### Why This Project?

**Existing Solutions & Limitations:**

- **Freewrite Smart, Alpha Typewriter**: Digital-only, expensive
- **Older thermal paper machines** (Canon Typestar, Brother Ep43, etc.):
  - Thermal paper is toxic and uncommon
  - Old hardware is hard to find and unreliable
  - No file export capability

**This Project's Advantages:**

- ✅ Real ink on paper (non-toxic, standard paper)
- ✅ Modern, open-source design
- ✅ File export capability
- ✅ Multiple modes and programs
- ✅ Interactive CLI interface
- ✅ Serviceable and long-lived design
- ✅ Potential Raspberry Pi version
- ✅ Educational tool and art piece
- ✅ All while maintaining: distraction-free, battery-operated operation

## Where This Device Excels

Beyond the classroom, this typewriter excels in various scenarios:

- **Libraries and quiet spaces**: Silent operation allows typing without disturbing others
- **Focus writing sessions**: No internet, notifications, or apps to distract from the task
- **Journaling and personal writing**: Physical output provides a tangible record
- **Field research and note-taking**: Battery-powered portability for on-site documentation
- **Creative writing retreats**: Distraction-free environment for deep work
- **Accessibility**: Alternative input method for those who prefer typing over handwriting
- **Offline documentation**: Create physical records without digital infrastructure
- **Artistic projects**: More than just typewritten documents — create visual art, graphics, patterns, and mixed-media works as physical artifacts
- **Workshops and conferences**: Quiet note-taking during presentations
- **Travel writing**: Portable, battery-powered typing without laptop bulk

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

- ✅ Idea and system architecture defined
- 🔄 PCB and firmware development in progress
- 🔄 CAD in progress
- 📋 Mechanical prototype upcoming

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
- Templating pages
- Raspberry Pi version

## Research Questions

User studies to understand preferences and effectiveness:

- I like physical/digital documents over [opposite] ones
- I can focus easily/get distracted on my computer when I'm typing a document
- I prefer to type/handwrite my drafts instead of [opposite] them
- I find digital/physical notes more effective than [opposite] notes

## License

TBD — MIT recommended for hardware + firmware openness.

## Contact / Discussion

Open an issue, start a discussion, or fork and experiment.

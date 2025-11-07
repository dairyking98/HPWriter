# Silent Digital Typewriter — Project Plan & Gantt Chart

## Overview

This document outlines the development stages for building the Silent Digital Typewriter (HP45 Inkjet) project. The plan is organized into phases with estimated timelines, dependencies, and milestones.

---

## Project Phases

### Phase 1: Research & Design Foundation
**Duration: 4-6 weeks**

| Task | Duration | Dependencies | Status |
|------|----------|--------------|--------|
| HP45 cartridge research & datasheet analysis | 1 week | None | 🔄 In Progress |
| ESP32-S3 USB Host capabilities study | 1 week | None | 📋 Planned |
| Stepper motor & driver selection (NEMA-14, TMC2209) | 1 week | None | 📋 Planned |
| Power system design (2S Li-ion, buck/boost) | 1 week | None | 📋 Planned |
| System architecture finalization | 1 week | All above | 📋 Planned |
| Component sourcing & BOM creation | 1 week | System architecture | 📋 Planned |

**Deliverables:**
- Complete BOM with part numbers
- System architecture diagram
- Power budget calculations
- HP45 firing pulse specifications

---

### Phase 2: Hardware Development — Core Electronics
**Duration: 8-10 weeks**

| Task | Duration | Dependencies | Status |
|------|----------|--------------|--------|
| HP45 controller PCB design | 3 weeks | Phase 1 complete | 📋 Planned |
| Main PCB design (ESP32-S3, TMC drivers, power, I²C headers for OLED + AS5600) | 4 weeks | Phase 1 complete | 📋 Planned |
| PCB prototyping & ordering | 1 week | PCB designs complete | 📋 Planned |
| PCB assembly & initial testing | 2 weeks | PCBs received | 📋 Planned |

**Deliverables:**
- HP45 controller PCB (Gerber files)
- Main control PCB (Gerber files) with I²C headers:
  - OLED header (4-pin: VCC, GND, SCL, SDA + optional RST)
  - AS5600 encoder header (I²C, shared bus with OLED)
- Assembled prototype boards
- Basic power-on tests
- I²C pull-up resistors (2.2–4.7 kΩ) installed

---

### Phase 3: Firmware Development — Core Functionality
**Duration: 11-13 weeks** (extended for encoder and virtual detent implementation)

| Task | Duration | Dependencies | Status |
|------|----------|--------------|--------|
| ESP32-S3 development environment setup | 1 week | Phase 2 (PCBs) | 📋 Planned |
| USB keyboard HID host implementation | 2 weeks | Dev environment | 📋 Planned |
| USB flash drive filesystem (FatFs) | 2 weeks | Dev environment | 📋 Planned |
| Text buffer & basic editor | 2 weeks | Keyboard input working | 📋 Planned |
| Display driver abstraction (SSD1306/SH1106) | 1 week | Dev environment | 📋 Planned |
| Basic UI framework (2-line layout) | 1 week | Display driver | 📋 Planned |
| AS5600 encoder driver (I²C) | 1 week | Dev environment | 📋 Planned |
| Virtual detent controller (state machine, control loop) | 2 weeks | Encoder driver | 📋 Planned |
| Feed axis integration (encoder + stepper) | 1 week | Virtual detent controller | 📋 Planned |
| Rasterizer (text to bitmap) | 3 weeks | Text buffer complete | 📋 Planned |
| HP45 controller serial protocol | 2 weeks | Phase 2 (HP45 PCB) | 📋 Planned |
| Motion control (stepper drivers) | 2 weeks | Phase 2 (main PCB) | 📋 Planned |
| Integration testing (keyboard → print) | 2 weeks | All modules complete | 📋 Planned |

**Deliverables:**
- Working firmware with all core features
- USB keyboard input functional
- File save/load working
- Display driver (SSD1306/SH1106) with I²C communication
- Basic 2-line UI framework
- AS5600 encoder driver with I²C communication
- Virtual detent controller (FREE_ROLL / DETENT_LOCKED / PRINT_ALIGN states)
- Feed axis encoder integration
- Basic printing capability

---

### Phase 4: Mechanical Design & Assembly
**Duration: 9-11 weeks** (extended for encoder mounting and lever mechanism)

| Task | Duration | Dependencies | Status |
|------|----------|--------------|--------|
| Carriage module CAD design | 3 weeks | Phase 1 (component specs) | 📋 Planned |
| Paper feed module CAD design (AS5600 mount, magnet positioning) | 2 weeks | Phase 1 (component specs) | 📋 Planned |
| Free-roll lever mechanism design | 1 week | Paper feed module | 📋 Planned |
| Main chassis/enclosure design | 2 weeks | Carriage & feed modules | 📋 Planned |
| 3D printing & machining (prototypes) | 2 weeks | CAD complete | 📋 Planned |
| Mechanical assembly & fit testing | 1 week | Parts manufactured | 📋 Planned |

**Deliverables:**
- Complete CAD models (STEP files)
- AS5600 encoder mount design
- Magnet mounting design (1-2 mm gap, centered on shaft)
- Free-roll lever mechanism
- 3D printed/machined prototype parts
- Assembled mechanical prototype
- Fit and clearance verification

---

### Phase 5: Integration & System Testing
**Duration: 8-10 weeks** (extended for encoder calibration and virtual detent tuning)

| Task | Duration | Dependencies | Status |
|------|----------|--------------|--------|
| Full system integration (electronics + mechanics) | 2 weeks | Phase 3 & 4 complete | 📋 Planned |
| Motion system calibration | 2 weeks | Integration complete | 📋 Planned |
| Encoder calibration routine (roller circumference, counts_per_detent) | 1 week | Motion system calibrated | 📋 Planned |
| Virtual detent tuning (gains, thresholds, currents) | 1 week | Encoder calibrated | 📋 Planned |
| Print quality optimization | 2 weeks | Motion calibrated | 📋 Planned |
| Battery system integration & testing | 1 week | Power system designed | 📋 Planned |
| End-to-end user testing | 1 week | All systems working | 📋 Planned |

**Deliverables:**
- Fully integrated prototype
- Calibrated motion system
- Encoder calibrated (roller circumference measured, counts_per_detent computed)
- Virtual detents tuned and functional
- Acceptable print quality
- Battery operation verified

---

### Phase 6: UI/UX & Polish
**Duration: 7-9 weeks** (extended for 2-line OLED + encoder UI implementation)

| Task | Duration | Dependencies | Status |
|------|----------|--------------|--------|
| OLED display hardware integration (SSD1306/SH1106 I²C) | 1 week | Phase 2 (main PCB) | 📋 Planned |
| Display driver implementation (oled_ssd1306.c) | 1 week | Display hardware ready | 📋 Planned |
| Text UI implementation (ui/text_ui.c, 2-line layout) | 2 weeks | Display driver complete | 📋 Planned |
| Font system (6×10 default, 8×12 hi-vis) | 1 week | Text UI framework | 📋 Planned |
| Button interface (PRINT/MODE/FEED/BKSP) | 1 week | Display working | 📋 Planned |
| UI integration (keystroke echo ≤10ms, status display) | 1 week | Text UI & buttons | 📋 Planned |
| LPI switching (6/8 LPI toggle, update counts_per_detent) | 1 week | Virtual detent controller | 📋 Planned |
| Free-roll lever integration (state machine trigger) | 1 week | Virtual detent controller | 📋 Planned |
| Screen invalidation & optimization (dirty regions) | 1 week | UI integration | 📋 Planned |
| Autosave & document management | 1 week | Filesystem working | 📋 Planned |

**Deliverables:**
- Functional 2-line OLED display interface
- Real-time keystroke echo (≤10 ms response)
- Status indicators (battery %, USB, microSD, mode, LPI)
- LPI switching (6/8 LPI) with live display update
- Free-roll lever functional (FREE_ROLL / PRINT_ALIGN state switching)
- Button controls working (PRINT/MODE/FEED/BKSP)
- Document management features

---

### Phase 7: Documentation & Refinement
**Duration: 4-6 weeks**

| Task | Duration | Dependencies | Status |
|------|----------|--------------|--------|
| Wiring diagrams & schematics | 1 week | Final hardware design | 📋 Planned |
| Firmware documentation | 2 weeks | Final firmware | 📋 Planned |
| Assembly instructions | 1 week | Final mechanical design | 📋 Planned |
| Maintenance & troubleshooting guide | 1 week | All documentation | 📋 Planned |
| Bug fixes & refinements | 2 weeks | User testing feedback | 📋 Planned |

**Deliverables:**
- Complete documentation package
- Assembly guide
- Maintenance manual
- Refined v1.0 release

---

## Gantt Chart Timeline

```
Phase 1: Research & Design Foundation
|████████████████████████████████████████| 4-6 weeks

Phase 2: Hardware Development — Core Electronics
        |████████████████████████████████████████████████| 8-10 weeks
        (starts after Phase 1)

Phase 3: Firmware Development — Core Functionality
                |████████████████████████████████████████████████████████| 11-13 weeks
                (starts after Phase 2 PCBs ready)

Phase 4: Mechanical Design & Assembly
        |████████████████████████████████████████████████████| 9-11 weeks
        (can start in parallel with Phase 2/3)

Phase 5: Integration & System Testing
                                        |████████████████████████████████████| 8-10 weeks
                                        (starts after Phase 3 & 4)

Phase 6: UI/UX & Polish
                                        |████████████████████████████████████| 7-9 weeks
                                        (can overlap with Phase 5)

Phase 7: Documentation & Refinement
                                                |████████████████████████████| 4-6 weeks
                                                (starts near end of Phase 5)
```

---

## Critical Path

The longest path through the project (critical path) is:

**Phase 1 → Phase 2 → Phase 3 → Phase 5 → Phase 6 → Phase 7**

**Total Estimated Duration: 40-51 weeks (10-12.75 months)**

---

## Parallel Work Opportunities

To accelerate development, these tasks can run in parallel:

- **Phase 2 (Hardware)** and **Phase 4 (Mechanical)** can overlap significantly
- **Phase 3 (Firmware)** modules can be developed in parallel once dependencies are met
- **Phase 6 (UI/UX)** can begin while Phase 5 integration testing is ongoing
- **Phase 7 (Documentation)** can start as soon as designs are finalized

**Optimized Timeline: 32-40 weeks (8-10 months)** with parallel execution

---

## Milestones

| Milestone | Target Phase | Success Criteria |
|-----------|--------------|------------------|
| **M1: Design Complete** | End of Phase 1 | All specifications finalized, BOM ready |
| **M2: Electronics Prototype** | End of Phase 2 | PCBs assembled and power-on successful |
| **M3: Firmware Alpha** | End of Phase 3 | Keyboard input → print output working |
| **M4: Mechanical Prototype** | End of Phase 4 | All mechanical parts assembled and tested |
| **M5: Integrated System** | End of Phase 5 | Full system working end-to-end |
| **M6: User-Ready Prototype** | End of Phase 6 | UI complete, all features functional |
| **M7: v1.0 Release** | End of Phase 7 | Documentation complete, ready for use |

---

## Risk Management

### High-Risk Areas

1. **HP45 Cartridge Control**
   - Risk: Complex firing pulse timing, potential cartridge damage
   - Mitigation: Extensive research, low-power testing, protection circuits

2. **USB Host on ESP32-S3**
   - Risk: Limited examples, driver compatibility issues
   - Mitigation: Early prototyping, fallback to USB-to-serial if needed

3. **Print Quality**
   - Risk: Alignment, ink flow, paper feed accuracy
   - Mitigation: Iterative testing, calibration routines, adjustable mechanisms

4. **Power Management**
   - Risk: Battery life, power efficiency, heat management
   - Mitigation: Power profiling, efficient drivers, thermal design

5. **Mechanical Precision**
   - Risk: Print alignment, paper feed consistency
   - Mitigation: Quality components, calibration software, adjustable mounts

---

## Resource Requirements

### Hardware Components
- ESP32-S3 development boards (multiple for testing)
- HP45 cartridges (for testing and development)
- NEMA-14 stepper motors (2×)
- TMC2209 drivers (2×)
- Linear rails (MGN7/MGN9)
- Power management components
- USB keyboards and flash drives (for testing)
- OLED displays (1.3" 128×32, SSD1306/SH1106, I²C)
- AS5600 encoder boards (I²C breakout)
- 6 mm diametric magnets (N52 recommended)
- I²C pull-up resistors (2.2–4.7 kΩ)
- Buttons (PRINT/MODE/FEED/BKSP)
- Free-roll lever switch
- Silicone/urethane rollers (≈16 mm Ø for 2.000″ circumference)
- 3D printing/machining services

### Software Tools
- PCB design software (KiCad, Altium, etc.)
- CAD software (Fusion 360, SolidWorks, etc.)
- ESP-IDF development environment
- Version control (Git)
- Documentation tools

### Skills Required
- Embedded systems programming (ESP32)
- PCB design and layout
- Mechanical design (CAD)
- Stepper motor control
- USB protocol knowledge
- Power electronics design

---

## Success Metrics

### v1.0 Goals (from README)
- ✅ Typewritten text printed cleanly on plain paper
- ✅ USB keyboard and flash drive working end-to-end
- ✅ Basic editor + print buffer
- ✅ Instant boot and safe power-off
- ✅ Battery powered, quiet operation

### Quality Targets
- Print resolution: ≥ 300 DPI equivalent
- Boot time: < 2 seconds
- Battery life: ≥ 4 hours continuous use
- Noise level: < 40 dB at 1 meter
- Print speed: ≥ 1 character per second

---

## Next Steps

1. **Immediate (Week 1)**
   - Complete HP45 cartridge research
   - Set up development environment
   - Order initial components for testing

2. **Short-term (Weeks 2-4)**
   - Finalize system architecture
   - Begin PCB schematic design
   - Start firmware framework

3. **Medium-term (Months 2-3)**
   - Complete PCB designs and order prototypes
   - Develop core firmware modules
   - Begin mechanical CAD work

---

## Notes

- This plan is a living document and should be updated as the project progresses
- Timelines are estimates and may vary based on component availability, testing results, and design iterations
- Some phases may require multiple iterations before moving to the next phase
- User feedback and testing may reveal additional requirements or refinements needed

---

**Last Updated:** 2025-01-XX (Display specs added)  
**Project Status:** Early Development Phase


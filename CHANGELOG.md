# Changelog

All notable changes to the Silent Digital Typewriter project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

**Last Updated:** 2025-11-06

## [Unreleased]

### Added
- Display specifications section (OLED 1.3" 128×32, SSD1306/SH1106, 2-line layout)
  - Screen technology details (monochrome OLED, I²C interface)
  - Display layout specifications (Line 1: edit buffer, Line 2: status/context)
  - Font options (6×10 default, 8×12 hi-visibility)
  - Hardware implementation (I²C wiring, power requirements)
  - Firmware requirements (UI rendering functions, keystroke echo ≤10 ms)
  
- Paper feed encoder specifications (AS5600 magnetic encoder)
  - Encoder choice rationale (AS5600 vs AS5048A)
  - Virtual detent system (3-state machine: FREE_ROLL / DETENT_LOCKED / PRINT_ALIGN)
  - Mechanics and calculations (roller circumference, line heights, detents per revolution)
  - Hardware implementation (I²C wiring, magnet mounting)
  - Control loop specifications (~1 kHz update rate)
  - Calibration routine documentation
  
- Carriage end stop sensor specifications (A3144 Hall effect sensor)
  - Sensor selection (A3144 recommended, alternatives documented)
  - Hardware implementation (5 V power, GPIO wiring, magnet mounting)
  - Homing sequence specifications
  - Calibration and troubleshooting procedures
  
- Print specifications
  - Character spacing (CPI): 10 CPI and 12 CPI options
  - Line spacing (LPI): 6 LPI and 8 LPI options (reorganized documentation)
  - Print resolution details (HP45 cartridge capabilities)
  - Carriage motion calculations (steps per character)
  - Paper feed motion calculations (steps per line)
  - Font design and rendering specifications
  - Configuration examples and firmware settings

### Changed
- Updated Hardware Overview to include:
  - OLED display specifications
  - AS5600 encoder for paper feed axis
  - A3144 end stop sensor for carriage axis
  
- Updated Firmware Features to include:
  - Real-time 2-line OLED display with keystroke echo
  - Virtual detent paper feed system
  - Carriage homing functionality
  - Character spacing selection (10/12 CPI)
  
- Updated Repository Structure to include:
  - Display driver module
  - Text UI module
  - Encoder driver module
  - Virtual detent controller module
  - End stop sensor module
  
- Renamed "LPI Switching" section to "Spacing Selection (CPI & LPI)" to include both character and line spacing options

### Documentation
- Added comprehensive Display Specifications section
- Added Paper Feed Encoder & Virtual Detent System section
- Added Carriage End Stop Sensor section
- Added Print Specifications section
- Updated PROJECT_PLAN.md with encoder-related tasks across phases 2-6

## [0.1.0] - 2025-11-06

### Added
- Initial project documentation
- Project plan and Gantt chart
- System architecture overview
- Hardware specifications (ESP32-S3, HP45 cartridge, stepper motors, TMC2209 drivers)
- Basic firmware feature list
- Repository structure outline

---

## Types of Changes

- **Added** for new features
- **Changed** for changes in existing functionality
- **Deprecated** for soon-to-be removed features
- **Removed** for now removed features
- **Fixed** for any bug fixes
- **Security** in case of vulnerabilities


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
- **Carriage**: NEMA-14 stepper, GT2 belt, MGN7/MGN9 linear rail + end stop sensor (home position)
- **Paper Feed**: NEMA-14 stepper + pinch rollers + AS5600 magnetic encoder (virtual detents)
- **Drivers**: 2× TMC2209 (silent / microstepping)
- **Storage**: USB flash + optional microSD
- **Power**: 2S Li-ion → buck 5V → 3.3V LDO, plus 18–24 V boost for HP45
- **UI**: Monochrome OLED 1.3" 128×32 (SSD1306/SH1106 via I²C), 2-line layout, buttons (PRINT/MODE/FEED/BKSP)

## Firmware Features

- Reads USB keyboards (HID)
- Saves `.txt` files to USB flash
- Simple line buffer editing (typewriter mode / line mode)
- Prints lines as raster sweeps
- Silent stepper motion (StealthChop + low jerk)
- Autosave and document export
- Real-time 2-line OLED display with keystroke echo (≤10 ms response)
- Status indicators (battery %, USB, microSD, mode)
- Virtual detent paper feed (AS5600 encoder, 6/8 LPI switching)
- Free-roll lever with soft spring detent alignment
- Carriage homing (end stop sensor for repeatable print alignment)
- Character spacing: 10 or 12 CPI (characters per inch) selectable

## Repository Structure (planned)

```
firmware/
├── esp32-s3/
│   ├── keyboard host
│   ├── filesystem (FatFs)
│   ├── rasterizer
│   ├── motion control
│   ├── HP45 controller protocol
│   ├── display driver (SSD1306/SH1106)
│   ├── text UI (2-line layout)
│   ├── encoder driver (AS5600)
│   ├── virtual detent controller
│   └── end stop sensor (carriage homing)

hardware/
├── main PCB (ESP32-S3, TMC drivers, power, I²C headers, GPIO inputs)
├── carriage module (CAD + BOM, end stop sensor mount)
├── feed module (CAD + BOM, AS5600 encoder)
├── OLED display module (I²C, 4-pin header)
└── free-roll lever mechanism

docs/
├── theory of operation
├── serial protocol to HP45 controller
├── wiring diagrams
├── display specifications
├── encoder & virtual detent system
├── end stop sensor specifications
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
- Additional font options (10/12 CPI already implemented)
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

---

## Display Specifications

### Screen Technology

- **Type**: Monochrome OLED
- **Size**: 1.3″ 128×32 pixels
- **Controller**: SSD1306/SH1106
- **Interface**: I²C (preferred), SPI optional
- **Response Time**: ~1–2 ms (no ghosting)
- **Power Consumption**: ~20–25 mA at full brightness (typical much lower)

### Display Layout

- **Lines**: 2-line layout (typewriter aesthetic, fast edits)
- **Line 1**: Live edit buffer (monospace 6×10 or 8×12 font)
  - ~16–21 characters visible depending on font/margins
- **Line 2**: Context/status display
  - Shows: Mode, USB status, Battery %, microSD status, Quiet/Draft icon
  - Alternative: Previous line in Line-Buffer mode

### Font Options

- **Default**: 6×10 monospace (~21 chars per line)
- **High-visibility**: 8×12 monospace (~16 chars per line)

### User Interface

**Soft Keys** (mapped to physical buttons):
- `PRINT` - Print current buffer
- `MODE` - Switch between Typewriter/Line-Buffer modes
- `FEED` - Feed paper line
- `BKSP` - Backspace/delete

**Status Indicators**:
- Battery percentage
- USB present (✓)
- microSD present
- Quiet/Draft mode icon

### Hardware Implementation

- **I²C Configuration**: SCL/SDA with 2.2–4.7 kΩ pull-ups to 3.3 V
- **Connector**: 4-pin header (VCC, GND, SCL, SDA) + optional RST
- **Power**: 3.3 V supply, ~20–25 mA budget for bright pages
- **Future**: SPI pads reserved if faster refresh needed

### Firmware Requirements

- Display driver abstraction (SSD1306/SH1106 via I²C)
- UI rendering functions:
  - `ui_render_line(text)` - Update line 1 (edit buffer)
  - `ui_render_status(mode, usb, batt, sd)` - Update line 2 (status)
- Keystroke echo at ≤10 ms end-to-end
- Screen invalidation (dirty regions) to minimize I²C traffic
- Font tables: 6×10 (default), 8×12 (hi-vis)

**Configuration**:
- `DISPLAY_LINES=2`
- `FONT=6x10` (or `8x12`)
- `MARGIN_COLS=1`

### Rationale

- **Two lines** maintain minimalism while improving editing and status visibility vs. single line
- **OLED** chosen over LCD for instant response and legibility of small monospace fonts
- **I²C** preferred for simplicity; SPI reserved if bus gets crowded
- **Fast response** (≤10 ms) ensures real-time typing feedback

---

## Paper Feed Encoder & Virtual Detent System

### Encoder Choice

**AS5600** (selected)
- **Interface**: I²C
- **Resolution**: 12-bit (4096 counts/revolution)
- **Wiring**: Simple (2 wires + power)
- **Rationale**: Plenty of resolution for virtual detents, simplest wiring, excellent for tactile feedback

**AS5048A** (alternative)
- **Interface**: SPI
- **Resolution**: 14-bit (16384 counts/revolution)
- **Wiring**: More complex (4 wires + power)
- **Use case**: Ultra-fine detent feel if needed

### Mechanics (Paper Feed)

- **Roller Circumference (C)**: 50.8 mm (2.000″) recommended (clean LPI math)
- **Line Height (H)**:
  - 6 LPI → 4.233 mm
  - 8 LPI → 3.175 mm
- **Detents per Revolution**:
  - 6 LPI: 12 detents
  - 8 LPI: 16 detents

### Counts per Detent (Virtual Notch)

For **AS5600** (4096 CPR):
- **6 LPI**: ~341 counts/detent (4096 × 4.233 / 50.8)
- **8 LPI**: ~256 counts/detent (4096 × 3.175 / 50.8) — power-of-two-friendly

For **AS5048A** (16384 CPR): multiply by ~4
- **6 LPI**: ~1364 counts/detent
- **8 LPI**: ~1024 counts/detent

### Hardware Implementation

**Electrical (ESP32-S3 + AS5600)**:
- **Power**: 3.3 V to AS5600 VDD
- **I²C**: SDA → GPIO (e.g., 8), SCL → GPIO (e.g., 9)
- **Pull-ups**: 2.2–4.7 kΩ to 3.3 V (shared with OLED)
- **ADDR**: Leave default unless multiple encoders
- **Magnet**: 6 mm diameter diametric magnet on feed shaft end
  - Centered on shaft
  - 1–2 mm gap from encoder IC
  - N52 recommended for strongest signal

**Mechanical**:
- Silicone/urethane roller (≈16 mm Ø for 2.000″ circumference with jacket)
- Free-roll lever switch (detects lever position)
- Magnet mounting on feed shaft end

### Firmware Model (Virtual Detents)

**State Machine**:

1. **FREE_ROLL** (lever up)
   - User can spin paper freely
   - Motor applies soft virtual spring to nearest detent
   - Low hold current (80–120 mA)
   - Provides tactile feedback without resistance

2. **DETENT_LOCKED** (within ±window of detent center)
   - Motor stiffens slightly to give click/hold feel
   - Medium hold current (180–240 mA)
   - Holds paper at exact line position

3. **PRINT_ALIGN** (lever down)
   - Nudges to exact detent center
   - Normal line-advance stepping
   - Print current (300–500 mA RMS) for traction
   - Open-loop motion during printing; encoder used for alignment only

**Core Math**:

```
counts_per_detent = CPR × (H / C)
detent_index = round(angle_counts / counts_per_detent)
detent_center = detent_index × counts_per_detent
d = angle_counts - detent_center  (signed error)
```

**Control Loop** (~1 kHz update rate):
- **Outside snap window**: Set low spring → `target = angle - k_soft × d`; low hold current
- **Inside detent window**: Lock → `target = detent_center`; medium hold current
- **On resume print**: Step tiny correction to `detent_center`; set print current

**Suggested Gains & Thresholds**:
- `snap_window`: ~±(0.35 × counts_per_detent)
- `k_soft`: 0.15–0.30 (dimensionless; tune by feel)
- **Currents** (NEMA-14 via TMC2209):
  - `low_hold_current`: 80–120 mA
  - `med_hold_current`: 180–240 mA
  - `print_current`: 300–500 mA RMS (tune for traction)

### Stepper Integration

**Feed Axis**:
- TMC2209 in StealthChop mode
- Hold current varies by state (FREE_ROLL / DETENT_LOCKED / PRINT_ALIGN)
- During printing: open-loop motion (encoder ignored)
- After each line advance: recenter to detent if residual error appears

**Carriage Axis**:
- Remains open-loop stepper (no encoder)
- TMC2209 StealthChop
- End stop sensor for homing (mechanical microswitch or optical)
- Home on startup (seeks end stop, establishes reference position)
- Constant-velocity sweeps for HP45 raster printing

### Spacing Selection (CPI & LPI)

**CPI Switching (10/12)**:
- 10/12 CPI toggle in UI changes:
  - `steps_per_character` calculation
  - Carriage motion timing during print sweeps
  - Display shows "CPI: 10" or "CPI: 12" on second OLED line

**LPI Switching (6/8)**:
- 6/8 LPI toggle in UI changes:
  - `counts_per_detent` calculation
  - Firmware's steps-per-line during motor-driven advance
  - Display shows "LPI: 6" or "LPI: 8" on second OLED line

**Settings**:
- CPI and LPI settings are independent (can be combined in any way)
- Both settings stored in NVS and persist across reboots
- Display format: "CPI: 10 | LPI: 6" (abbreviated as space allows)

### Calibration Routine (~1 minute)

1. Enter Cal mode in UI
2. Prompt: "Install magnet; rotate 1 rev"
3. Read AS5600 raw from 0→4095; verify clean wrap (no dropouts)
4. Measure actual roller circumference `C_meas`:
   - Mark paper/roller
   - Roll one revolution against ruler
   - Store in NVS (Non-Volatile Storage)
5. Compute `counts_per_detent` from measured `C_meas`
6. Test: Show live `detent_index`; rotate by hand and verify predictable "clicks"

### Parts Shortlist

- AS5600 board (I²C breakout)
- 6 mm diametric magnet (N52 recommended)
- NEMA-14 stepper (feed axis)
- TMC2209 driver (feed axis)
- Silicone/urethane roller (≈16 mm Ø for 2.000″ circumference)
- Lever switch (free-roll detect)
- ESP32-S3 DevKit (USB-OTG)

### Benefits

- **Natural feel**: Virtual detents provide tactile feedback like mechanical typewriters
- **Precise alignment**: Encoder ensures perfect line spacing
- **Flexible**: Easy LPI switching without mechanical changes
- **Quiet**: Soft spring in free-roll mode; no mechanical clicks
- **Reliable**: Magnetic encoder has no mechanical wear
- **Calibratable**: Software calibration compensates for manufacturing tolerances

---

## Carriage End Stop Sensor

### Purpose

The carriage end stop sensor establishes a repeatable home (reference) position for the carriage axis. Since the carriage uses open-loop stepper control (no encoder), the end stop provides the absolute position reference needed for accurate print alignment.

### Sensor Options

**A3144 Hall Effect Sensor** (recommended)
- **Type**: Unipolar digital Hall effect sensor (Allegro A3144)
- **Operating Voltage**: 4.5–24 V (5 V recommended)
- **Output**: Open-collector, active LOW when magnet detected
- **Sensitivity**: Switches at ~75–150 G (Gauss)
- **Response Time**: < 5 μs
- **Advantages**: Non-contact, reliable, no mechanical wear, long life, immune to dust/contamination
- **Mounting**: Fixed to chassis at carriage home position
- **Actuator**: Small diametric magnet (e.g., 3×3 mm, N42/N52) on carriage
- **Operating Distance**: 1–5 mm from magnet (tune for reliable triggering)

**Mechanical Microswitch** (alternative)
- **Type**: SPDT or SPST microswitch (e.g., Omron D2F-01F, Cherry D44X)
- **Actuation Force**: 0.25–0.5 N
- **Travel**: 0.5–1.0 mm
- **Advantages**: Simple, low cost, passive (no power)
- **Mounting**: Fixed to chassis at carriage home position
- **Actuator**: Cam or flag on carriage that depresses switch at home
- **Considerations**: Mechanical wear over time, contact bounce

**Optical End Stop** (alternative)
- **Type**: Phototransistor/photodiode pair or reflective sensor (e.g., TCST2103)
- **Response Time**: < 1 ms
- **Advantages**: Non-contact, high precision, long life
- **Mounting**: Fixed to chassis with reflector/flag on carriage
- **Considerations**: Requires LED current (~5–20 mA), sensitive to ambient light

### Hardware Implementation

**A3144 Hall Effect Sensor (Recommended)**:

**Electrical**:
- **Power**: 5 V (VCC) from ESP32-S3 5 V rail or LDO
- **Ground**: GND connection
- **Signal**: GPIO input (e.g., GPIO 10) with pull-up resistor
- **Connection**: 
  - VCC → 5 V
  - GND → GND
  - OUT → GPIO (with 10 kΩ pull-up to 3.3 V or 5 V)
- **Logic**: Active LOW when magnet detected (carriage at home)
- **Current Draw**: ~5–10 mA typical
- **Note**: A3144 is 5 V device; use level shifter or pull-up to 3.3 V with 10 kΩ resistor

**Mechanical**:
- **Sensor Mounting**: Fixed to chassis at leftmost position (home)
  - Sensor face should be perpendicular to magnet travel
  - Mount with small adjustment capability for fine-tuning
- **Magnet Mounting**: Small diametric magnet on carriage
  - 3×3 mm or 4×4 mm neodymium magnet (N42 or N52 grade)
  - Mounted with pole axis perpendicular to sensor
  - Distance: 2–4 mm from sensor face when at home
  - Use epoxy or mechanical retention
- **Sensing Distance**: 1–5 mm (adjust magnet distance for reliable triggering)
- **Repeatability**: ±0.05 mm typical with proper alignment
- **Hysteresis**: A3144 has built-in hysteresis (~50 G) to prevent oscillation

**PCB Requirements**:
- 5 V power supply (from main 5 V rail)
- GPIO input pin with 10 kΩ pull-up resistor (to 3.3 V or 5 V)
- Optional: 100 nF bypass capacitor near sensor VCC
- Optional: LED indicator for visual feedback during homing
- Optional: Level shifter if using 3.3 V GPIO with 5 V sensor

**Alternative Wiring (3.3 V GPIO)**:
- If GPIO is 3.3 V logic: Pull-up A3144 OUT to 3.3 V (10 kΩ)
- A3144 open-collector output is safe with 3.3 V pull-up
- No level shifter needed if pull-up is to 3.3 V

### Firmware Implementation

**Homing Sequence**:

1. **Startup Home** (on boot):
   ```
   - Move carriage slowly toward home (negative direction)
   - Monitor end stop GPIO (active LOW when magnet detected)
   - When triggered: stop immediately
   - Back off slightly (~0.5–1 mm) to exit hysteresis zone
   - Set position = 0 (home established)
   ```

2. **Homing Speed**:
   - Slow approach: ~10–20 mm/s (Hall sensor responds quickly, less overshoot risk)
   - Fast retry: If sensor not found in expected range, move faster to find it
   - Back-off: ~5 mm/s reverse to exit hysteresis zone

3. **Position Tracking**:
   - After homing: Track position in steps/mm
   - Steps per mm = (motor_steps × microsteps) / (belt_pitch × pulley_teeth)
   - Example: 200 steps/rev × 16 microsteps / (2 mm × 20 teeth) = 80 steps/mm

4. **Safety Limits**:
   - Maximum travel: Hard limit (software) prevents over-travel
   - Emergency stop: If end stop not found within limits, halt and error

**Configuration**:
```c
#define CARRIAGE_HOME_GPIO       10
#define CARRIAGE_HOME_POLARITY   ACTIVE_LOW  // A3144 outputs LOW when magnet detected
#define CARRIAGE_HOMING_SPEED    15.0  // mm/s (Hall sensor allows faster)
#define CARRIAGE_BACKOFF_DISTANCE 0.5  // mm (smaller due to hysteresis)
#define CARRIAGE_MAX_TRAVEL      200.0 // mm (example)
#define CARRIAGE_DEBOUNCE_MS     2     // Minimal debounce needed (Hall is fast)
```

### Benefits

- **Repeatable Alignment**: Ensures print always starts from same position
- **Absolute Reference**: Provides known position without encoder
- **Non-Contact**: No mechanical wear, long life (millions of cycles)
- **Fast Response**: < 5 μs response time allows faster homing speeds
- **Reliable**: Immune to dust, contamination, and mechanical failure
- **Hysteresis**: Built-in hysteresis prevents oscillation at trigger point
- **Low Cost**: A3144 sensor < $1, small magnet < $0.50
- **Simple Integration**: Open-collector output easy to interface with GPIO

### Calibration

**Initial Setup**:
1. Install A3144 sensor at desired home position (fixed to chassis)
2. Install small magnet on carriage (diametric, pole perpendicular to sensor)
3. Adjust magnet distance (2–4 mm typical) for reliable triggering:
   - Too close: Sensor may always be triggered
   - Too far: May not trigger reliably
   - Test by moving carriage manually and observing GPIO
4. Verify homing sequence works reliably:
   - Carriage approaches, sensor triggers, backs off slightly
   - Position resets to 0
5. Test print alignment repeatability (should be ±0.1 mm or better)

**Magnet Orientation**:
- Use diametric magnet (magnetized across diameter)
- Mount with pole axis perpendicular to sensor face
- If not triggering: Rotate magnet 90° (may need opposite pole)
- Test both poles to find which triggers reliably

**Troubleshooting**:
- Sensor not triggering: Move magnet closer, check polarity
- Sensor always triggered: Move magnet farther, check for stray fields
- Intermittent operation: Check wiring, verify magnet is secure
- False triggers: Shield sensor from other magnetic sources (motors, speakers)

**Maintenance**:
- Check sensor operation periodically (unlikely to fail)
- Verify magnet hasn't shifted or fallen off
- Clean sensor face if contaminated (rarely needed)
- Check for loose wiring connections

### Parts Shortlist

- A3144 Hall effect sensor (Allegro A3144, or compatible SS41)
- 3×3 mm or 4×4 mm diametric neodymium magnet (N42 or N52 grade)
- 10 kΩ pull-up resistor (for GPIO)
- 100 nF bypass capacitor (optional, for sensor VCC)
- Mounting hardware (screws, standoffs, small bracket for sensor)
- Magnet mounting adhesive (epoxy) or mechanical retention
- Optional: LED indicator for visual feedback
- Optional: Level shifter (only if needed for 3.3 V GPIO compatibility)

### Integration Notes

- End stop sensor is independent of encoder system (feed axis only)
- Carriage uses open-loop control after homing
- Position is tracked in firmware based on step count from home
- Homing required on every boot for accurate positioning
- Optional: Periodic re-homing during long print jobs to correct drift
- **Magnetic Interference**: Keep A3144 sensor away from:
  - Stepper motors (may require 50+ mm distance)
  - Power transformers
  - Other strong magnetic sources
- **Power Supply**: A3144 requires 5 V; can share with other 5 V peripherals
- **Sensing Distance**: Test and document magnet distance for reproducible setup

---

## Print Specifications

### Character Spacing (CPI)

**10 CPI** (recommended)
- **Character Width**: 0.100 inch (2.54 mm) per character
- **Characters per Line**: ~80 characters on 8.5" wide paper (with margins)
- **Typical Use**: Standard typewriter spacing, similar to pica type
- **Advantages**: More readable, easier to scan, traditional typewriter feel

**12 CPI** (alternative)
- **Character Width**: 0.0833 inch (2.117 mm) per character
- **Characters per Line**: ~96 characters on 8.5" wide paper (with margins)
- **Typical Use**: Elite type spacing, more compact, higher information density
- **Advantages**: More text per page, professional document appearance

**Selection**:
- Toggle between 10 CPI and 12 CPI via UI (MODE button or menu)
- Display shows "CPI: 10" or "CPI: 12" on second OLED line
- Selection stored in NVS and persists across reboots

### Line Spacing (LPI)

**6 LPI** (recommended)
- **Line Height**: 0.1667 inch (4.233 mm) per line
- **Lines per Page**: ~66 lines on 11" tall paper (with margins)
- **Typical Use**: Double-spaced equivalent, comfortable reading
- **Advantages**: More readable, easier to edit, professional appearance

**8 LPI** (alternative)
- **Line Height**: 0.1250 inch (3.175 mm) per line
- **Lines per Page**: ~88 lines on 11" tall paper (with margins)
- **Typical Use**: Single-spaced equivalent, compact documents
- **Advantages**: More text per page, efficient use of paper

**Selection**:
- Toggle between 6 LPI and 8 LPI via UI (separate from CPI setting)
- Display shows "LPI: 6" or "LPI: 8" on second OLED line
- Selection stored in NVS and persists across reboots

### Print Resolution

**HP45 Cartridge Capabilities**:
- **Nozzle Resolution**: ~300 DPI (dots per inch) native
- **Nozzle Spacing**: ~85 μm (0.0033 inch) between nozzles
- **Print Head Width**: ~0.5 inch (12.7 mm) with 50 nozzles
- **Ink Drop Size**: Variable (can adjust for different print qualities)

**Character Rendering**:
- **Monospace Font**: Fixed-width characters (all characters same width)
- **Rasterization**: Characters converted to bitmap patterns
- **Character Height**: ~2.5–3.0 mm (depending on font design)
- **Character Width**: Matches CPI setting (2.54 mm @ 10 CPI, 2.117 mm @ 12 CPI)

### Carriage Motion (Character Spacing)

**10 CPI Motion**:
- **Steps per Character**: Depends on belt pitch and pulley
- **Example Calculation**: 
  - Belt pitch: 2 mm (GT2)
  - Pulley: 20 teeth
  - Steps/mm: 80 (200 steps/rev × 16 microsteps / 40 mm/rev)
  - Steps per character @ 10 CPI: 80 steps/mm × 2.54 mm = ~203 steps
- **Carriage Speed**: Constant velocity during print sweep
- **Acceleration**: Smooth start/stop to prevent ink smearing

**12 CPI Motion**:
- **Steps per Character**: 
  - Steps per character @ 12 CPI: 80 steps/mm × 2.117 mm = ~169 steps
- **Faster Printing**: More characters per second at same carriage speed
- **Same mechanical setup**: Only firmware timing changes

### Paper Feed Motion (Line Spacing)

**6 LPI Feed**:
- **Steps per Line**: 
  - Roller circumference: 50.8 mm (2.000")
  - Steps/mm: 80 (example)
  - Steps per line @ 6 LPI: 80 steps/mm × 4.233 mm = ~339 steps
- **Feed Speed**: Smooth acceleration, controlled deceleration
- **Alignment**: Virtual detent ensures perfect line spacing

**8 LPI Feed**:
- **Steps per Line**:
  - Steps per line @ 8 LPI: 80 steps/mm × 3.175 mm = ~254 steps
- **Faster Feed**: Shorter distance per line
- **Same mechanical setup**: Only firmware step count changes

### Font Design

**Monospace Character Set**:
- **ASCII Printable**: 95 characters (space through ~)
- **Character Bitmaps**: Stored in ROM/Flash
- **Character Height**: ~10–12 pixels (2.5–3.0 mm @ 300 DPI)
- **Character Width**: Fixed per CPI setting
  - 10 CPI: ~30 pixels wide (2.54 mm @ 300 DPI)
  - 12 CPI: ~25 pixels wide (2.117 mm @ 300 DPI)

**Font Rendering**:
- **Rasterizer**: Converts text buffer to bitmap rows
- **Print Sweep**: Carriage moves at constant velocity
- **Nozzle Firing**: Triggered at precise positions based on character bitmaps
- **Ink Drops**: Fired as carriage passes over paper

### Configuration

**Firmware Settings**:
```c
// Character spacing (CPI)
#define CPI_10_STEPS_PER_CHAR  203  // Steps per character @ 10 CPI
#define CPI_12_STEPS_PER_CHAR  169  // Steps per character @ 12 CPI

// Line spacing (LPI)
#define LPI_6_STEPS_PER_LINE   339  // Steps per line @ 6 LPI
#define LPI_8_STEPS_PER_LINE   254  // Steps per line @ 8 LPI

// Character dimensions (mm)
#define CPI_10_CHAR_WIDTH_MM   2.54
#define CPI_12_CHAR_WIDTH_MM   2.117
#define LPI_6_LINE_HEIGHT_MM   4.233
#define LPI_8_LINE_HEIGHT_MM   3.175
```

**User Settings**:
- CPI selection: 10 or 12 (stored in NVS)
- LPI selection: 6 or 8 (stored in NVS)
- Settings independent (can have 10 CPI + 6 LPI, or 12 CPI + 8 LPI, etc.)

### Print Quality

**Resolution**: 300 DPI equivalent (HP45 native resolution)
**Character Clarity**: Sharp, clear characters suitable for documents
**Ink**: HP45 compatible ink cartridges
**Paper**: Standard plain paper (20–24 lb bond recommended)

### Benefits

- **Flexible Spacing**: User-selectable CPI and LPI for different document needs
- **Professional Output**: Clean, readable typewritten documents
- **Traditional Feel**: Mimics classic typewriter spacing options
- **Efficient**: More text per page with 12 CPI / 8 LPI settings
- **Readable**: More spacing with 10 CPI / 6 LPI settings
- **Software Configurable**: No mechanical changes needed to switch spacing

---

**Last Updated:** 2025-01-XX (Display specs + Encoder specs + End stop sensor + Print specs added)
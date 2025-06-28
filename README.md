![title](https://github.com/TilmanGriesel/WindScape/blob/main/docs/title.png?raw=true)

WindScape is a Home Assistant ESPHome project that transforms ordinary PC fans into natural wind simulators.
The main goal is to create realistic, location-inspired wind patterns that mimic the feeling of natural outdoor breezes - from gentle Mediterranean coastal winds to dramatic Alpine gusts - bringing a more organic and soothing airflow experience to your indoor workspace.

## Features

- **Wind Simulation Engine** with layered sine wave oscillations
- **6 Location Presets** inspired by real-world locations
- **100-level speed control**
- **Real-time RPM monitoring** (optional sensor)
- **Wireless control** via Home Assistant and WiFI
- **Automation-Ready** via Home Assistant, combine sensors to set presets and parameters
- **OTA firmware updates** via ESPHome
- **Activity-based wind cycles** (quiet/normal/active periods)

## Wind Mode Presets

Transform the atmosphere of your room with realistic wind profiles inspired by beautiful locations around the world. Each preset is designed to mimic the **natural wind behavior** you might feel in these places from gentle seaside breezes to dramatic mountain gusts.

#### 1. Ocean (Atlantic Coast)
- **Feel:** Strong, steady seaside wind with rolling gusts
- **Experience:** Like standing on a breezy beach where ocean winds roll in with energy. Great for a refreshing, invigorating airflow that feels alive and ever-changing.

<img src="https://github.com/TilmanGriesel/WindScape/blob/main/docs/plage.png?raw=true" width="100">

#### 2. Mediterranean (Italian Coast)
- **Feel:** Soft and relaxed coastal breeze
- **Experience:** Gentle wind like what you'd feel while lounging on a terrace overlooking the Mediterranean. Ideal for calm afternoons, reading, or light background airflow.

<img src="https://github.com/TilmanGriesel/WindScape/blob/main/docs/capri.png?raw=true" width="100">

#### 3. Countryside (French Fields)

- **Feel:** Peaceful, barely-there wind
- **Experience:** Imagine a warm summer day in a quiet lavender field. The air moves softly, almost like a whisper. Perfect for unwinding or sleeping.

<img src="https://github.com/TilmanGriesel/WindScape/blob/main/docs/valensole.png?raw=true" width="100">

#### 4. Mountains (Alpine Range)

- **Feel:** Crisp, lively mountain air with frequent gusts
- **Experience:** Like opening a window in the Alps — cool, brisk, and full of natural movement. Great for simulating fresh, outdoor mountain air indoors.

<img src="https://github.com/TilmanGriesel/WindScape/blob/main/docs/alpine.png?raw=true" width="100">

#### 5. Plains (Patagonian Steppes)

- **Feel:** Bold and intense wind with powerful surges
- **Experience:** Feels like you're standing in one of the windiest places on Earth. Ideal for a dramatic atmosphere, energizing effect, or maximum airflow performance.

<img src="https://github.com/TilmanGriesel/WindScape/blob/main/docs/patagonia.png?raw=true" width="100">

#### 6. Fjord (Norwegian Fjords)

- **Feel:** Channeled, dramatic airflow with pressure shifts
- **Experience:** Like the wind funnelling between tall cliffs, unpredictable and thrilling. Excellent for a unique, immersive airflow experience.

<img src="https://github.com/TilmanGriesel/WindScape/blob/main/docs/fjord.png?raw=true" width="100">

#### 7. Manual Mode

- **Feel:** Whatever you choose
- **Experience:** Full control. No simulation, just direct manual speed adjustment, great for testing, automations or when you want a consistent fan output without variation.

## Quick Setup

### Hardware
- ESP32 development board (Lolin32 Lite recommended)
- Fan controller (like the Noctua NA-FC1) https://noctua.at/en/na-fc1
- 4-pin PWM fan (like the Noctua NV-FS1) https://noctua.at/en/nv-fs1
- 12V power supply
- Optional: buck converter for 5V ESP32 power, the ESP32 can be powered by USB too.

**Alternative DIY NV-FS1:**
- https://www.printables.com/model/1324299-pc-desk-fan
- https://www.printables.com/model/889331-noctua-inspired-desk-fan-mount

### Wiring Diagram
```
        +-----------------------------+
        |      12V Power Supply       |
        |                             |
        |   +12V ─────┬────────────┐  |
        |   GND  ─────┴────┐       │  |
        +------------------│-------│--+
                           │       │
                      +----▼-------▼----+
                      |   Lolin32 Lite  |
                      |     (ESP32)     |
                      |                 |
                      |  GPIO14 ─────┐  |
                      |              └──► PWM to NA-FC1 Pin 3
                      |  GPIO27 ◄──────── TACH from NA-FC1 Pin 4
                      |  GND ───────────► shared GND
                      |  VIN ◄── 5V from buck converter (optional)
                      +-----------------+
                                 │
                                 ▼
        +-------------------------------------------+
        |                NA-FC1                     |
        |                                           |
        |  Pin 1: GND ◄────── shared GND ───────────┘
        |  Pin 2: +12V ◄───── from 12V power supply
        |  Pin 3: PWM ◄────── from ESP32 GPIO14
        |  Pin 4: TACH ──────► to ESP32 GPIO27
        +-------------------------------------------+
                                 │
                                 ▼
                          +-------------+
                          |  Noctua Fan |
                          |   (4-pin)   |
                          +-------------+

```

**References:**
- WEMOS LOLIN32 Lite Pinout: https://www.espboards.dev/esp32/lolin32/
- Wiring example: https://raw.githubusercontent.com/wiki/KlausMu/esp32-fan-controller/images/fritzingESP32_BME280_fan.png

![breadboard](https://github.com/TilmanGriesel/ha_esphome_desk_fan/blob/main/docs/img1.png?raw=true)

![assembled](https://github.com/TilmanGriesel/ha_esphome_desk_fan/blob/main/docs/img2.png?raw=true)

### Software
1. Flash the ESPHome config to your ESP32
2. Add WiFi credentials to `secrets.yaml`
3. Device appears automatically in Home Assistant

## Configuration

**Sliders:**
- **Wind Intensity** (30–150%) - Global power multiplier for all wind effects
- **Gust Frequency** (10–90%) - How often natural gusts occur
- **Minimum Oscillation** (5–50%) - Baseline wind variation to prevent static airflow
- **Minimum Fan Speed** (15–100%) - Safety limit to prevent fan stalling
- **Maximum Fan Speed** (15–100%) - Upper safety limit for fan operation

**Select:**
- **Wind Mode** - Choose from 6 location presets or Manual mode

**System Controls:**
- **Restart** - Reboots the ESP32 device

## How It Works

**WindScape** creates realistic wind behavior using a sophisticated atmospheric simulation engine that translates natural wind patterns into PWM signals for PC fan control.

### Core Wind Engine

**Layered Sine Wave System:**
- **Primary Layer:** Slow breathing pattern that mimics natural atmospheric pressure changes
- **Secondary Layer:** Medium-frequency micro-turbulence for realistic variation  
- **Tertiary Layer:** High-frequency fine detail that adds natural texture
- **Mathematical Harmony:** Uses carefully chosen frequency ratios (3.7x, 11.3x) to create non-repeating, organic patterns

**Activity-Based Cycles:**
- **Quiet Periods:** Gentle, minimal variation with extended breathing cycles
- **Normal Conditions:** Standard wind behavior with balanced activity
- **Active Periods:** Increased turbulence and more frequent, dramatic gusts
- **Automatic Transitions:** System naturally cycles between states with weighted probabilities

### Location-Specific Behavior

Each preset adjusts multiple atmospheric parameters:
- **Base wind speed** and **breathing patterns**
- **Micro-turbulence intensity** and **gust characteristics**
- **Update frequencies** and **activity modifiers**
- **Location-specific adaptations** for quiet and active periods

### Gust System

**Natural Gust Distribution:**
- **Probability-based timing** with minimum 5-second intervals
- **Moderate-bias strength** (favors realistic over extreme gusts)
- **Three-phase progression:** Buildup → Sustain → Decay
- **Activity-aware behavior:** Quiet gusts are gentle and slow, active gusts are sharp and dramatic

### Fan Speed Calculation

1. **Base Calculation:** `base_speed × breathing_modifier × (1 + oscillation)`
2. **Gust Application:** Result multiplied by current gust intensity
3. **Global Scaling:** Applied wind intensity multiplier (30-150%)
4. **Safety Limits:** Enforced minimum/maximum fan speed boundaries
5. **PWM Output:** Final percentage converted to PWM signal

### Real-Time Monitoring

**Wind Sensors:**
- **Wind Speed** (MPH) - Current simulated wind velocity
- **Gust Strength** (%) - Active gust intensity above baseline
- **Wind Activity** (%) - Current activity mode indicator
- **Wind Condition** - Human-readable description (e.g., "Gentle breeze with gusts (coastal)")

**System Diagnostics:**
- **Physics Loop Frequency** (Hz) - Simulation performance monitoring
- **Fan Running** - Boolean status based on actual PWM output level
- **Standard ESP32 metrics** - Uptime, WiFi signal, temperature, memory usage

## Troubleshooting

**Fan not responding?**
- Check PWM connections (GPIO14) and ensure 25kHz compatibility
- Verify 12V power supply to fan controller
- Check minimum fan speed setting isn't too low

**No RPM reading?**
- Verify tachometer wiring (GPIO27) and internal pullup
- Confirm fan controller provides tach signal output
- Adjust pulse counter multiplier for your specific fan

**Wind simulation not working?**
- Ensure you're not in Manual mode
- Check Wind Intensity isn't set too low (minimum 30%)
- Verify Minimum Oscillation setting provides enough variation

**Erratic behavior?**
- Monitor system logs for parameter clamping warnings
- Check WiFi signal strength and connection stability
- Ensure power supply provides adequate current

## Tips for Best Experience

- **Start with defaults** - All presets are pre-tuned for natural feel
- **Adjust gradually** - Small changes in oscillation and intensity have big effects
- **Location matters** - Choose presets that match your desired atmosphere
- **Monitor sensors** - Use the wind condition sensor to understand current behavior
- **Activity awareness** - The system naturally varies intensity over time
- **Power scaling** - Use Wind Intensity as a master volume control

## Technical Notes

- **Update intervals** adapt dynamically based on selected location
- **Safety monitoring** prevents parameter drift and system instability  
- **Minimum oscillation** ensures wind never becomes completely static
- **Activity cycles** create long-term variation without user intervention
- **PWM-based status** provides accurate fan operation feedback

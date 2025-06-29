![title](https://github.com/TilmanGriesel/WindScape/blob/main/docs/title.png?raw=true)

WindScape is a Home Assistant ESPHome project that transforms ordinary PC fans into natural wind simulators.
The main goal is to create realistic, location-inspired wind patterns that mimic the feeling of natural outdoor breezes - from gentle Mediterranean coastal winds to dramatic Alpine gusts - bringing a more organic and soothing airflow experience to your indoor workspace.

## Features

- **Randomized Wind Simulation Engine** with natural weather phases
- **6 Location Presets** inspired by real-world locations
- **100-level speed control**
- **Real-time RPM monitoring** (optional sensor)
- **Wireless control** via Home Assistant and WiFI
- **Automation-Ready** via Home Assistant, combine sensors to set presets and parameters
- **OTA firmware updates** via ESPHome
- **Weather phase system** (quiet air/medium conditions/high activity)

## Wind Mode Presets

Transform the atmosphere of your room with realistic wind profiles inspired by beautiful locations around the world. Each preset configures different wind speed ranges, gust characteristics, and weather phase behaviors to create unique atmospheric experiences.

#### 1. Ocean (Atlantic Coast)
- **Wind Range:** 8-16 mph (moderate to fresh breeze)
- **Gust Behavior:** Frequent rolling gusts (3% probability, 2.0x intensity, 4s duration)
- **Weather Phases:** Balanced transitions with sustained medium and high activity periods
- **Experience:** Like standing on a breezy beach with consistent ocean winds and rolling wave-like gusts. Great for energizing, consistent airflow.

<img src="https://github.com/TilmanGriesel/WindScape/blob/main/docs/plage.png?raw=true" width="100">

#### 2. Mediterranean (Italian Coast)
- **Wind Range:** 4-10 mph (light to gentle breeze)
- **Gust Behavior:** Gentle, infrequent gusts (1.5% probability, 1.6x intensity, 2.5s duration)
- **Weather Phases:** Longer quiet periods, shorter high activity phases
- **Experience:** Soft coastal breeze perfect for relaxation, reading, or background airflow. Gentle enough for sleep yet alive enough to feel natural.

<img src="https://github.com/TilmanGriesel/WindScape/blob/main/docs/capri.png?raw=true" width="100">

#### 3. Countryside (French Fields)
- **Wind Range:** 2-8 mph (calm to light breeze)
- **Gust Behavior:** Rare, subtle gusts (0.8% probability, 1.4x intensity, 5s duration)
- **Weather Phases:** Extended quiet periods, very gentle high activity
- **Experience:** Whisper-quiet rural air with minimal variation. Perfect for sleeping, meditation, or when you want barely-noticeable natural airflow.

<img src="https://github.com/TilmanGriesel/WindScape/blob/main/docs/valensole.png?raw=true" width="100">

#### 4. Mountains (Alpine Range)
- **Wind Range:** 6-18 mph (moderate to strong breeze)
- **Gust Behavior:** Sharp, frequent gusts (5% probability, 2.3x intensity, 2s duration)
- **Weather Phases:** Quick transitions, shorter phases, dramatic weather changes
- **Experience:** Crisp mountain air with sudden wind shifts and sharp gusts. Ideal for creating an invigorating alpine atmosphere indoors.

<img src="https://github.com/TilmanGriesel/WindScape/blob/main/docs/alpine.png?raw=true" width="100">

#### 5. Plains (Patagonian Steppes)
- **Wind Range:** 10-22 mph (fresh to strong breeze)
- **Gust Behavior:** Powerful, sustained gusts (8% probability, 2.5x intensity, 6s duration)
- **Weather Phases:** High activity periods are intense and frequent
- **Experience:** The windiest preset - constant strong airflow with powerful gusts. Maximum performance for hot days or when you want dramatic atmospheric effects.

<img src="https://github.com/TilmanGriesel/WindScape/blob/main/docs/patagonia.png?raw=true" width="100">

#### 6. Fjord (Norwegian Fjords)
- **Wind Range:** 8-20 mph (moderate to strong breeze)
- **Gust Behavior:** Channeled, dramatic gusts (6% probability, 2.4x intensity, 3.5s duration)
- **Weather Phases:** Unpredictable transitions mimicking fjord wind patterns
- **Experience:** Dynamic airflow that feels like wind funneling between cliffs. Unique, immersive experience with surprising intensity changes.

<img src="https://github.com/TilmanGriesel/WindScape/blob/main/docs/fjord.png?raw=true" width="100">

#### 7. Manual Mode

- **Feel:** Whatever you choose
- **Experience:** Full control. No simulation, just direct manual speed adjustment, great for testing, automations or when you want a consistent fan output without variation.

## Build Instructions & Hardware Setup

### Hardware
- ESP32 development board (Lolin32 Lite recommended) around 6 Euros
- 12V power supply
- Optional: buck converter for 5V ESP32 power, the ESP32 can be powered by USB too.

### FAN
- 4-pin PWM desk and room fan (like the Noctua NV-FS1) https://noctua.at/en/nv-fs1

#### DIY NV-FS1 Desk and Room Fan
You can safe a bit of money and just print and build your own NV-FS1 with the NA-AA1-12 airflow amplifier. Here are some links:
- https://www.printables.com/model/1324299-pc-desk-fan
- https://www.printables.com/model/889331-noctua-inspired-desk-fan-mount
- https://noctua.at/en/nf-a12x25-pwm
- https://noctua.at/en/nv-aa1-12

#### BOM
Two simplified BOM options with one KIT version and a DIY build variant:

| Item                            | Setup A: NV‑FS1 Kit Route (€)                      | Setup B: DIY Build (€)    |
| ------------------------------- | -------------------------------------------------- | ------------------------- |
| NV‑FS1 Desk/Room Fan Kit        | **99.90** (MSRP)                                   | –                         |
| ESP32 (Lolin32 Lite)            | 8.21                                               | 8.21                      |
| Buck Converter (12 → 5 V)       | 4.00 (Mini‑Buck) approximate                       | 4.00                      |
| 4‑pin PWM fan cable (extension) | 2.95 (estimated)                                   | 2.95                      |
| Pull‑up & filter components     | 1.00 (estimated)                                   | 1.00                      |
| **Noctua NF‑A12x25 PWM fan**    | Included in kit                                    | 35.50 (average EU price)  |
| **NV‑AA1‑12 airflow amplifier** | Included in kit                                    | 14.90                     |
| 3D‑printed mount & hardware     | –                                                  | 5.00                      |
| 12 V Power Supply               | Included in kit                                    | 7.00                      |
| **Total Estimated Cost**        | **€116.06**                                        | **€78.56**                |
| **Savings with DIY Build**      | **–**                                              | **€37.50 less**           |

### Wiring Diagram

#### USB Powered

```
+-----------------------------+
|      12V Power Supply       |
|                             |
|   +12V ─────┬────────────┐  |
|             │            │
|             ▼            │
|   [Buck Converter]       │
|    In: 12V   Out: 5V     │
|        │         │       │
|        ▼         ▼       │
|      GND       +5V       │
|        │         │       │
+--------┴─────────┴───────┘

           +------------------------+
           |      Lolin32 Lite      |
           |        (ESP32)         |
           |                        |
           | USB ◄─────── USB 5V from host (PC/power)
           | GND ◄─────── Shared GND with PSU & fan
           |                        |
           | GPIO14 ─────┐          |
           |             └─────► PWM (Fan Pin 4, Blue)
           |                        |
           | GPIO27 ◄────┬──────── TACH (Fan Pin 3, Yellow)
           |             │
           |        [10kΩ pull-up to 3.3V]
           |             │
           |         [3.3kΩ] in series
           |             │
           |        [0.1nF cap to GND] ◄──(RC filter)
           +------------------------+
                     │
                     ▼
           +------------------------+
           |       Noctua Fan       |
           |         4-pin          |
           |                        |
           | Pin 1 (Black): GND ◄────── Shared GND (PSU + ESP32)
           | Pin 2 (Red):  +12V ◄────── +12V from PSU
           | Pin 3 (Yellow): TACH ─────► GPIO27 (via RC & pull-up)
           | Pin 4 (Blue):   PWM ◄───── GPIO14
           +------------------------+
```

#### With Buck Converter

```
+-----------------------------+
|      12V Power Supply       |
|                             |
|   +12V ─────┬────────────┐  |
|             │            │
|             ▼            │
|   [Buck Converter]       │
|    In: 12V   Out: 5V     │
|        │         │       │
|        ▼         ▼       │
|      GND       +5V       │
|        │         │       │
+--------┴────┬────┘       │
              ▼            ▼
       +------------------------+
       |      Lolin32 Lite      |
       |        (ESP32)         |
       |                        |
       | VIN ◄────────────── 5V from buck converter
       | GND ◄────────────── GND from buck converter
       |                        |
       | GPIO14 ─────┐          |
       |             └─────► PWM (Fan Pin 4, Blue)
       |                        |
       | GPIO27 ◄────┬──────── TACH (Fan Pin 3, Yellow)
       |             │
       |        [10kΩ pull-up to 3.3V]
       |             │
       |         [3.3kΩ] in series
       |             │
       |        [0.1nF cap to GND] ◄──(RC filter)
       +------------------------+
                     │
                     ▼
           +------------------------+
           |       Noctua Fan       |
           |         4-pin          |
           |                        |
           | Pin 1 (Black): GND ◄────── GND from PSU
           | Pin 2 (Red):  +12V ◄────── +12V from PSU
           | Pin 3 (Yellow): TACH ─────► GPIO27 (via RC & pull-up)
           | Pin 4 (Blue):   PWM ◄───── GPIO14
           +------------------------+
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
- **Wind Variability** (5–40%) - How much wind speed varies over time (replaces oscillation)
- **Minimum Fan Speed** (0–50%) - Safety limit to prevent fan stalling
- **Maximum Fan Speed** (40–100%) - Upper safety limit for fan operation

**Select:**
- **Wind Mode** - Choose from 6 location presets or Manual mode

**System Controls:**
- **Restart** - Reboots the ESP32 device

## How It Works

**WindScape** creates realistic wind behavior using a sophisticated randomized atmospheric simulation engine that mimics natural weather patterns with distinct phases and unpredictable variation.

### Core Wind Engine

**Randomized Target System:**
- **Target-Based Movement:** Wind smoothly moves toward random targets within location-specific ranges
- **Phase-Specific Behavior:** Each weather phase has its own wind speed ranges and characteristics
- **Micro-Turbulence:** Constant small random fluctuations add natural texture and prevent static air
- **Anti-Stagnation:** Multiple overlapping systems ensure continuous movement and variation

**Weather Phase System:**
WindScape now features three distinct weather phases that create natural atmospheric cycles:

#### 1. Quiet Air Phase (90-210 seconds)
- **Wind Range:** 30% to 40% of location's base range
- **Turbulence:** Minimal (0.2x intensity)
- **Gusts:** 40% less frequent, gentler (1.1-1.5x intensity, 2-6s duration)
- **Behavior:** Slow, gentle changes for peaceful, barely-there wind
- **Experience:** Like a calm summer evening or early morning stillness

#### 2. Medium Conditions Phase (120-300 seconds)
- **Wind Range:** 70% to 80% of location's base range
- **Turbulence:** Normal (0.6x intensity)
- **Gusts:** Standard frequency and intensity (1.2-1.8x, 1.5-6.5s duration)
- **Behavior:** Moderate variation with regular activity
- **Experience:** Typical outdoor breeze with natural movement

#### 3. High Activity Phase (60-150 seconds)
- **Wind Range:** 40% to 120% of location's base range (can exceed normal maximum!)
- **Turbulence:** Strong (1.2x intensity)
- **Gusts:** 2.5x more frequent, powerful (1.4-2.4x intensity, 1-4s duration)
- **Behavior:** Rapid, dramatic changes with frequent strong gusts
- **Experience:** Storm-like conditions with intense, energizing airflow

### Natural Phase Transitions

The system automatically transitions between phases using realistic probabilities:
- **Quiet → Medium** (60%) or **High** (40%) - sudden storms are possible!
- **Medium → Quiet** (30%), **Stay Medium** (40%), or **High** (30%)
- **High → Medium** (40%), **Quiet** (30%) - sudden calm, or **Stay High** (30%)

### Location-Specific Behavior

Each preset adjusts multiple atmospheric parameters for each phase:
- **Base wind speed ranges** and **phase-specific modifications**
- **Gust probability** and **intensity characteristics**
- **Turbulence levels** and **change rates**
- **Phase duration preferences** and **transition behaviors**

### Enhanced Gust System

**Realistic Gust Distribution:**
- **Phase-Appropriate Timing:** Quiet phases have fewer, gentler gusts; High phases have frequent, intense ones
- **Variable Duration:** 1.5 to 8 seconds with natural distribution (shorter gusts more common)
- **Dynamic Intensity:** Each gust has random strength within phase-appropriate limits
- **Natural Shape:** Three-phase progression (buildup → sustain with variation → decay)

### Fan Speed Calculation

1. **Target Generation:** Random targets within current phase's wind range
2. **Smooth Transition:** Current wind speed moves toward target at phase-appropriate rate
3. **Micro-Turbulence:** Phase-specific random fluctuations added continuously
4. **Gust Application:** Active gust multiplier applied if gust is in progress
5. **Global Scaling:** Wind intensity multiplier (30-150%) applied
6. **Safety Limits:** Minimum/maximum fan speed boundaries enforced
7. **PWM Output:** Final percentage converted to PWM signal

### Real-Time Monitoring

**Wind Sensors:**
- **Wind Speed** (MPH) - Current simulated wind velocity including gusts
- **Gust Active** (%) - Current gust intensity (0% when no gust active)
- **Current Weather Phase** - Shows "Quiet Air", "Medium Conditions", or "High Activity"
- **Wind Condition** - Human-readable description with phase information
- **Current Wind State** - Shows current behavior ("Wind building", "Gust active", "Phase: 2m remaining")

**System Diagnostics:**
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
- Verify Wind Variability setting provides enough movement

**Wind feels too static?**
- Increase Wind Variability setting (15-25% recommended)
- Check that weather phases are transitioning (watch "Current Weather Phase" sensor)
- Monitor "Current Wind State" to understand system behavior

**Too much variation?**
- Reduce Wind Variability setting
- Lower Gust Frequency setting
- Choose a calmer location preset (Countryside, Mediterranean)

## Tips for Best Experience

- **Start with defaults** - All presets are pre-tuned for natural feel
- **Adjust gradually** - Small changes in variability and intensity have big effects
- **Location matters** - Choose presets that match your desired atmosphere
- **Monitor phases** - Watch the weather phase sensor to understand long-term patterns
- **Phase awareness** - The system naturally creates quiet periods, normal conditions, and stormy phases
- **Power scaling** - Use Wind Intensity as a master volume control for all effects
- **Time of day** - Different phases work well for different activities (Quiet for sleeping, High for energizing)

## Technical Notes

- **Randomized timing** - All update intervals include random variation for natural unpredictability
- **Phase-based ranges** - Each weather phase operates within different wind speed ranges
- **Anti-cyclical design** - Eliminates predictable sine wave patterns in favor of target-based randomness
- **Multi-layered variation** - Combines phase changes, target movement, micro-turbulence, and gusts
- **Natural distribution** - Favors moderate conditions but allows for dramatic weather events
- **Maintenance systems** - Automatic nudging prevents the system from getting stuck in static states
- **Real-time adaptation** - Phase transitions happen organically based on elapsed time and probabilities

## Roadmap
- Explore connection to https://github.com/remvze/moodist

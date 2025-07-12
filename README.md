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

Which build you choose depends on which fan you're building this project with. Generally speaking, a 5V fan is a simpler build which can run off of any USB 3.0 or USB-C port on a computer, power bank, or low power wall connector.

### 5V Case Fan (simpler)
<details>

<summary>Expand to see Bill of Materials (5V)</summary>

#### Hardware
- Any ESP32 development board with onboard wifi. Easily bought for around €3.00
- Any 5V PC case fan that supports PWM (pulse width modulation)

#### FAN
- There's no set fan you should use, so long as the fan supports PWM speed control.
  - An ideal option would be a a [Noctua NF-A12x25 5V](https://noctua.at/en/products/fan/nf-a12x25-5v), which is extremeley quiet, and can operate on as little as 5% duty cycle
  - When comparing each fan's noise floor, remember that [db is logarithmic!](https://en.wikipedia.org/wiki/Decibel)

##### DIY NV-FS1 Desk and Room Fan
You can safe a bit of money and just print and build your own NV-FS1 with the NA-AA1-12 airflow amplifier. Here are some links:
- https://www.printables.com/model/554226-120mm-computer-fan-desk-mount
- https://www.printables.com/model/1324299-pc-desk-fan
- https://www.printables.com/model/889331-noctua-inspired-desk-fan-mount
- https://noctua.at/en/nf-a12x25-pwm
- https://noctua.at/en/nv-aa1-12

#### BOM
Two simplified BOM options with one KIT version and a DIY build variant:

| Item                            | 5V DIY Build (€)   |
| ------------------------------- | ------------------ |
| Noctua NF-A12x25 5V             | 33.00              |
| Generic ESP32                   | 3.00               |
| General hardware (screws, nuts) | 4.00               |
| **Total Estimated Cost**        | €40.00             |

#### Wiring Diagram

```
+----------------------------+
|     USB Power (5V)         |
|                            |
|   +5V ─────────────┐       |
|                    ▼       |
|           +----------------------+
|           |   ESP32 WROOM Board  |
|           |  (AliExpress-style)  |
|           |                      |
|           | 5V/VIN ◄─── 5V from USB
|           | GND    ◄─── GND from USB
|           |                      |
|           | GPIO14 ───┐          |
|           |           └────► PWM (Fan Pin 4, Blue)
|           |                      |
|           | GPIO27 ◄──┬────── TACH (Fan Pin 3, Green)
|           |           │
|           |   [10kΩ pull-up to 3.3V]
|           |           │
|           |     [3.3kΩ] in series
|           |           │
|           |   [0.1nF cap to GND] ◄──(RC filter)
|           +----------------------+
|                     │
|                     ▼
|           +----------------------+
|           |     Noctua Fan       |
|           |    5V PWM (4-pin)    |
|           |                      |
|           | Pin 1 (Black): GND ◄────── GND
|           | Pin 2 (Red or Yellow):  +5V ◄─── 5V/VIN
|           | Pin 3 (Green): TACH ───► GPIO27 (filtered)
|           | Pin 4 (Blue):   PWM ◄─── GPIO14
|           +----------------------+
```
           
</details>

### 12V Case Fan
<details>

<summary>Expand to see Bill of Materials (12V)</summary>

#### Hardware
- ESP32 development board (Lolin32 Lite recommended) around 6 Euros
- 12V power supply
- Optional: buck converter for 5V ESP32 power, the ESP32 can be powered by USB too.

#### FAN
- 4-pin PWM desk and room fan (like the Noctua NV-FS1) https://noctua.at/en/nv-fs1

##### DIY NV-FS1 Desk and Room Fan
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

#### Wiring Diagram

##### USB Powered

```
+-----------------------------+
|      12V Power Supply       |
|                             |
|   +12V ─────────────┐       |
|                     │       |
|                     ▼       |
|                 +12V to Fan |
|                             |
|                 GND ────────┘
|                             |
+-----------------------------+

           +------------------------+
           |      Lolin32 Lite      |
           |        (ESP32)         |
           |                        |
           | USB ◄──── USB from host (PC/power)
           | GND ◄──── Shared GND with PSU & fan
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

##### With Buck Converter

```
+-----------------------------+
|      12V Power Supply       |
|                             |
|   +12V ─────┬────────────┐  |
|             │            │
|             ▼            │
|   [Buck Conv. [LM2596]   │
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


![setup](https://raw.githubusercontent.com/TilmanGriesel/WindScape/843b6eca3a42019fdb35a68ddca5e0dcae5bd2b5/docs/title.png?raw=true)

![breadboard](https://github.com/TilmanGriesel/ha_esphome_desk_fan/blob/main/docs/img1.png?raw=true)

![assembled](https://github.com/TilmanGriesel/ha_esphome_desk_fan/blob/main/docs/img2.png?raw=true)

</details>

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

## Troubleshooting

**Fan not responding?**
- Verify PWM signal is correctly connected to the appropriate GPIO pin and supports 25 kHz operation; adjust configuration if necessary.
- Ensure the fan controller is receiving a stable 12 V power supply.
- Confirm the minimum fan speed setting is not set too low.
- Check the fan’s minimum duty cycle—some models require at least 20%, or even up to 70%, to start operating.
- Some fans may require an initial higher speed (“kick-start”) before they can be controlled at lower speeds.

**No RPM reading?**
- Verify tachometer wiring GPIO pin and internal pullup
- Confirm fan provides tach signal output (Yellow wire)
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
- **Monitor phases** - Watch the weather phase sensor to understand long-term patterns, test every pattern for 10 minutes to get a feel.
- **Phase awareness** - WindScape naturally creates quiet periods, normal conditions, and stormy phases
- **Power scaling** - Use Wind Intensity as a master volume control for all effects

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

---

# How WindScape Works

## Core Wind Engine

### Dynamic Target System
- **Smooth Movement Patterns:** Wind flows naturally toward randomized targets within location-appropriate ranges
- **Dynamic Phase Behavior:** Each weather phase features unique wind characteristics and speed ranges
- **Natural Micro-Turbulence:** Continuous subtle variations create realistic air texture and prevent artificial stillness
- **Continuous Motion Guarantee:** Multiple overlapping systems ensure the air never feels static or mechanical

### Three-Phase Weather System
WindScape cycles through three distinct atmospheric phases that mirror real-world weather patterns:

#### 1. Quiet Air Phase (90-210 seconds)
- **Wind Intensity:** 30-40% of location's base range
- **Turbulence:** Minimal (20% intensity)
- **Gusts:** 40% less frequent with gentle intensity (1.1-1.5x strength, 2-6 second duration)
- **Character:** Gradual, peaceful changes creating barely perceptible airflow
- **Feel:** Like a tranquil summer evening or the stillness of early dawn

#### 2. Medium Conditions Phase (120-300 seconds)
- **Wind Intensity:** 70-80% of location's base range
- **Turbulence:** Standard (60% intensity)
- **Gusts:** Normal frequency and strength (1.2-1.8x intensity, 1.5-6.5 second duration)
- **Character:** Moderate variation with steady, predictable activity
- **Feel:** A comfortable outdoor breeze with natural, rhythmic movement

#### 3. High Activity Phase (60-150 seconds)
- **Wind Intensity:** 40-120% of location's base range (can exceed normal limits!)
- **Turbulence:** Intense (120% intensity)
- **Gusts:** 2.5x more frequent with powerful bursts (1.4-2.4x intensity, 1-4 second duration)
- **Character:** Rapid, dramatic shifts with frequent strong gusts
- **Feel:** Storm-like conditions delivering energizing, dynamic airflow

## Natural Phase Transitions

The system transitions between phases using realistic weather probabilities:
- **From Quiet Air:** 60% chance to Medium Conditions, 40% chance to High Activity (sudden storms happen!)
- **From Medium Conditions:** 30% to Quiet Air, 40% stay Medium, 30% to High Activity
- **From High Activity:** 40% to Medium, 30% to Quiet Air (sudden calm), 30% stay High

## Location-Specific Customization

Each environment preset fine-tunes atmospheric parameters across all phases:
- **Base wind ranges** with **phase-specific adjustments**
- **Gust frequency** and **intensity profiles**
- **Turbulence characteristics** and **transition speeds**
- **Phase duration preferences** and **weather progression patterns**

## Advanced Gust System

### Realistic Wind Burst Patterns
- **Phase-Matched Timing:** Quiet phases deliver infrequent, gentle gusts; High Activity phases produce frequent, intense bursts
- **Natural Duration Range:** 1.5-8 seconds with realistic distribution (shorter gusts more common)
- **Variable Intensity:** Each gust features randomized strength within phase-appropriate limits
- **Organic Shape:** Three-stage progression (gradual buildup → sustained peak with variation → smooth decay)

## Wind Speed Calculation Process

1. **Target Generation:** Creates random wind targets within the current phase's intensity range
2. **Smooth Transitions:** Current wind speed flows toward targets at phase-appropriate rates
3. **Micro-Turbulence:** Applies phase-specific random fluctuations continuously
4. **Gust Integration:** Adds active gust multipliers when wind bursts are in progress
5. **Global Scaling:** Applies user wind intensity setting (30-150%)
6. **Safety Enforcement:** Ensures output stays within minimum/maximum fan speed limits
7. **PWM Conversion:** Translates final percentage to precise PWM control signals

## Real-Time Wind Monitoring

### Wind Conditions Display
- **Current Wind Speed** (MPH) - Live simulated velocity including active gusts
- **Gust Status** (%) - Real-time gust intensity (shows 0% when no gust is active)
- **Active Weather Phase** - Displays "Quiet Air," "Medium Conditions," or "High Activity"
- **Wind Description** - Human-friendly summary with current phase information
- **System Status** - Shows current behavior ("Wind building," "Gust active," "Phase: 2m remaining")

### System Health Diagnostics
- **Standard ESP32 Metrics** - System uptime, WiFi signal strength, processor temperature, memory usage

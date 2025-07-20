![WindScape Title](https://github.com/TilmanGriesel/WindScape/blob/main/docs/title.png?raw=true)

# WindScape

*A Home Assistant / ESPHome project that turns everyday PWM‑controlled PC fans into remarkably natural indoor wind simulators.*

WindScape delivers authentic, location‑inspired airflow, from a gentle Mediterranean whisper to a brisk Alpine surge, creating a natural, immersive atmosphere for your workspace, gaming setup, or sim‑racing cockpit. It’s a versatile wind simulator ready to elevate any project you come up with.

![noctua_nv_fs1_1](https://noctua.at/pub/media/catalog/product/cache/74c1057f7991b4edb2bc7bdaa94de933/n/o/noctua_nv_fs1_5.jpg)

## Table of Contents

1. [Features](#features)
1. [Operating Modes](#operating-modes)
1. [Demo](#demo)
1. [Build Guides](#build-guides)
1. [Software Setup](#software-setup)
1. [Preset Library](#preset-library)
1. [Troubleshooting](#troubleshooting)
1. [Tips for Best Experience](#tips-for-best-experience)
1. [Technical Notes](#technical-notes)
1. [Roadmap](#roadmap)
1. [How WindScape Works](#how-windscape-works)
1. [Links](#links)

---

## Features

* **Realistic Wind Physics**
  Smart turbulence modeling and thermal effects that create natural-feeling wind patterns, just like you'd experience outdoors.

* **Nature-Inspired Environment Presets**
  Five carefully crafted profiles that capture the feel of different outdoor locations – from gentle countryside breezes to powerful ocean winds.

* **Steady or Dynamic Modes**
  Choose between constant airflow or ever-changing wind patterns that respond naturally to the environment you select.

* **External Sensor Support**
  Connect live data feeds (like racing sim speed) and WindScape will automatically adjust airflow in real-time, with smart fallback when the signal drops.

* **Home Assistant Integration**
  Control, monitor, and automate entirely over Wi‑Fi.

* **Comprehensive Real-Time Monitoring**
  Deep insight into the simulation engine – weather phases, thermal events, turbulence levels, target tracking, and all physics calculations exposed as sensors.

* **OTA Firmware Updates**
  Built on ESPHome for one‑click wireless upgrades.

---

## Operating Modes

| Mode                              | Description                                                                                           |
| --------------------------------- | ----------------------------------------------------------------------------------------------------- |
| **Oscillating (Wind Simulation)** | Natural, ever‑changing airflow pattern.                                                               |
| **Steady (Constant Breeze)**      | Fixed speed, no variability.                                                                          |
| **External Sensor**               | Maps a Home Assistant sensor (e.g. car speed) to live fan speed; auto‑fallback after 60 s of silence. |

> **Global Power Control** in Home Assistant overrides every mode.

---

## Demo

![WindScape configuration](https://github.com/TilmanGriesel/WindScape/blob/main/docs/windscape_demo_01.gif?raw=true)
![WindScape dashboard](https://github.com/TilmanGriesel/WindScape/blob/main/docs/windscape_demo_02.gif?raw=true)
![External sensor example](https://github.com/TilmanGriesel/WindScape/blob/main/docs/ha_iracing_01.png?raw=true)
![Breezer9000](https://raw.githubusercontent.com/TilmanGriesel/WindScape/843b6eca3a42019fdb35a68ddca5e0dcae5bd2b5/docs/title.png?raw=true)

---

## Build Guides

Which build you choose depends on the fan you pair with the project. In general, a 5 V fan makes things simpler because it can be powered from any USB‑A 3.0 or USB‑C port, a power bank, or a low‑wattage wall adapter.

### Hardware

* **Microcontroller** – Any ESP32 development board with built‑in Wi‑Fi (widely available for about €3).
* **Fan** – Any 5 V PC case fan that supports PWM (pulse‑width modulation).

### Fan Selection

There’s no single “required” fan, just make sure it supports PWM speed control. A great premium option is the **Noctua NF‑A12x25 5 V**, which is exceptionally quiet and can run at duty cycles as low as 5 %.

When comparing noise levels, remember that the decibel (dB) scale is logarithmic: even small changes in the number can mean a significant audible difference.

### 5 V Fan (DIY USB‑Powered & Simple)

<details>
<summary>Bill of Materials & Wiring</summary>

| Item                            | 5V DIY Build (€)   |
| ------------------------------- | ------------------ |
| Noctua NF-A12x25 5V             | 33.00              |
| Generic ESP32                   | 3.00               |
| General hardware (screws, nuts) | 4.00               |
| 3‑D printed mount & hardware    | 5.00               |
| **Total Estimated Cost**        | **45.00**          |

<details>
<summary>Wiring Diagram</summary>

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

</details>

_Thanks and credit to @mwood77_

---

### 12 V Fan (Noctua NV‑FS1 Fan Kit or DIY)

<details>
<summary>Bill of Materials & Wiring</summary>

| Item                         | Kit Route (€) | DIY Route (€) |
| ---------------------------- | ------------- | ------------- |
| NV‑FS1 fan kit (inc. PSU)    | **99.90**     | —             |
| ESP32 (Lolin32 Lite)         | 8.21          | 8.21          |
| Buck converter 12 → 5 V      | 4.00          | 4.00          |
| 4‑pin fan cable              | 2.95          | 2.95          |
| Noctua NF‑A12x25 PWM         | —             | 35.50         |
| NV‑AA1‑12 amplifier          | —             | 14.90         |
| 3‑D printed mount & hardware | —             | 5.00          |
| 12 V PSU                     | Included      | 7.00          |
| **Total**                    | **116.06**    | **78.56**     |

<details>
<summary>Wiring Diagram</summary>

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

</details>
</details>

---

## Software Setup

1. Flash the WindScape ESPHome YAML to your ESP32.
2. Add Wi‑Fi credentials in `secrets.yaml`.
3. Reboot—the device will auto‑discover in Home Assistant.


---

## Preset Library

| # | Preset                            | Base Wind Range | Gust Style               | Character                      |
| - | --------------------------------- | --------------- | ------------------------ | ------------------------------ |
| 1 | **Ocean (Atlantic Coast)**        | 8–16 mph        | Rolling, 3 % @ 2 × speed | Energising, constant swell     |
| 2 | **Mediterranean (Italian Coast)** | 4–10 mph        | Gentle, 1.5 % @ 1.6 ×    | Relaxed, great for reading     |
| 3 | **Countryside (French Fields)**   | 2–8 mph         | Rare, 0.8 % @ 1.4 ×      | Whisper‑quiet, ideal for sleep |
| 4 | **Mountains (Alpine Range)**      | 6–18 mph        | Sharp, 5 % @ 2.3 ×       | Crisp, invigorating shifts     |
| 5 | **Plains (Patagonian Steppes)**   | 10–22 mph       | Sustained, 8 % @ 2.5 ×   | Strongest airflow, hot days    |
| 6 | **Fjord (Norwegian Fjords)**      | 8–20 mph        | Channeled, 6 % @ 2.4 ×   | Dramatic, funnel‑like gusts    |
| 7 | **Manual**                        | —               | —                        | Direct user control            |

---

## Configuration Reference

| Control              | Range / Options    | Purpose                   |
| -------------------- | ------------------ | ------------------------- |
| **Wind Intensity**   | 30 – 150 %         | Master power scaler       |
| **Gust Frequency**   | 10 – 90 %          | Probability of gust start |
| **Wind Variability** | 5 – 40 %           | Overall speed fluctuation |
| **Min Fan Speed**    | 0 – 50 %           | Prevent stall             |
| **Max Fan Speed**    | 40 – 100 %         | Safety ceiling            |
| **Wind Mode**        | 6 presets + Manual | Select ambience           |
| **Restart**          | —                  | Reboot ESP32              |

**Advanced Tuning Parameters** (for power users via config):
- Turbulence Detail Level: 30m (how fine-grained the wind texture is)
- Turbulence Strength: 0.3 (how pronounced the micro-variations are)  
- Thermal Event Rate: 0.025Hz (how often warm air bubbles occur)
- Thermal Intensity: 1.8× (how strong thermal effects feel)
- Thermal Coverage: 15m (how wide the thermal influence spreads)

---

## Monitoring & Sensors

WindScape exposes comprehensive real-time data about the wind simulation engine:

### **Core Wind Metrics**
| Sensor | Unit | Description |
|--------|------|-------------|
| **Wind Speed** | m/s | Current output wind speed |
| **Target Wind Speed** | m/s | Where the simulation is heading |
| **Wind Change Rate** | m/s² | How fast wind is accelerating/decelerating |
| **Fan RPM** | RPM | Actual fan rotation speed |

### **Weather System Status**
| Sensor | Type | Description |
|--------|------|-------------|
| **Current Weather Phase** | 0-2 | Quiet (0), Medium (1), High (2) activity |
| **Weather Phase Description** | Text | Human-readable phase status |
| **Phase Duration Remaining** | seconds | Time until next phase change |

### **Physics Engine Monitoring**
| Sensor | Unit | Description |
|--------|------|-------------|
| **Turbulence Energy** | J | Von Kármán spectral energy level |
| **Spectral Energy Buffer** | m/s | Real-time turbulence contribution |
| **Gust Active** | % | Current gust progression (0-100%) |
| **Gust Intensity** | × | Active gust strength multiplier |
| **Thermal Bubble Active** | % | Thermal event progression (0-100%) |
| **Current Thermal Contribution** | m/s | Thermal effect on wind speed |

### **System Status Indicators**
| Sensor | Type | Description |
|--------|------|-------------|
| **Wind Simulation Active** | Binary | Physics engine running status |
| **Gust Currently Active** | Binary | Gust event in progress |
| **Thermal Bubble Active** | Binary | Thermal event in progress |
| **External Sensor Mode** | Binary | Using external data feed |
| **Wind Speed Converging** | Binary | Current speed near target |
| **Simulation Status** | Text | Overall engine activity summary |
| **Physics Debug Info** | Text | Technical diagnostic data |

These sensors enable:
- **Real-time dashboards** showing wind physics in action
- **Advanced automations** based on weather phases or thermal events  
- **Performance monitoring** and troubleshooting
- **Educational visualization** of atmospheric modeling

---

## Troubleshooting

| Symptom                  | Checks                                                                                                |
| ------------------------ | ----------------------------------------------------------------------------------------------------- |
| **Fan not spinning**     | Verify PWM pin & 25 kHz capability · Confirm 5 V/12 V supply · Raise *Min Fan Speed* above stall duty |
| **No RPM feedback**      | Check tach wiring/pull‑up · Confirm fan outputs tach · Adjust pulse multiplier                        |
| **No wind simulation**   | Ensure **Manual Mode** is *off* · *Wind Intensity* ≥ 30 % · *Wind Variability* not 0 %                |
| **Airflow feels static** | Increase *Wind Variability*                                        |
| **Too chaotic**          | Lower *Gust Frequency* or choose a calmer preset                                                      |

---

## Tips for Best Experience

* **Begin with Defaults** – Presets are tuned for realism out‑of‑the‑box.
* **Tweak Slowly** – Small slider changes have large perceptual impact.
* **Pick the Right Preset** – Match airflow to activity: calming *Countryside* for sleep, energising *Plains* for heat.

---

## Technical Notes

**How the Wind Physics Work:**
WindScape creates realistic wind by combining several clever techniques:

* **Turbulence Modeling** - Uses proven atmospheric science to generate the micro-variations that make wind feel natural instead of robotic
* **Thermal Effects** - Simulates warm air rising (like over hot pavement) that creates those gentle gusts you feel in real life
* **Weather Moods** - Automatically shifts between calm, moderate, and active periods, just like real weather patterns
* **Smart Gusting** - Adds realistic wind bursts that build up naturally, peak, and fade away organically
* **Smooth Transitions** - Prevents jarring speed changes by using physics-based momentum

**Behind the Scenes:**
- Updates 5 times per second with intelligent randomization
- Tracks multiple wind frequencies simultaneously for authentic texture
- Models thermal events that happen every few seconds to minutes
- Adapts intensity based on the current weather "mood"
- Uses logarithmic scaling so fan speed changes feel natural to humans

---

## Roadmap

* Native integration with [moodist](https://github.com/remvze/moodist) ambience engine.

---

## How WindScape Works

**The Wind Simulation System:**
WindScape layers multiple systems to create convincing natural wind:

1. **Micro-Turbulence** - Constantly adds tiny speed variations (like leaves rustling) by mixing different "frequencies" of wind movement together

2. **Thermal Bubbles** - Occasionally creates rising warm air effects that start gentle, build to a peak, then fade away naturally over 8-15 seconds

3. **Weather Moods** - Automatically cycles between calm periods (1.5-3.5 min), normal activity (2-5 min), and windy periods (1-2.5 min) with natural transitions

4. **Realistic Movement** - All speed changes use momentum and inertia, so the fan never makes sudden jarring jumps

5. **Multiple Time Scales** - Fast micro-changes (5x/sec), medium gusts (seconds), and slow mood shifts (minutes) all work together

**External Sensor Mode** skips all the simulation and directly maps your data (like car speed) to fan speed, with a 60-second safety timeout that switches back to your last preset if the signal is lost.

---

## Links

* **Home Assistant** – [https://www.home-assistant.io/](https://www.home-assistant.io/)
* **ESPHome** – [https://esphome.io/](https://esphome.io/)
* **Noctua Fans & Accessories** – [https://noctua.at/](https://noctua.at/)
  * https://noctua.at/en/nf-a12x25-pwm
  * https://noctua.at/en/nv-aa1-12
  * Noctua NV-FS1 YT Review: https://youtu.be/9PvWBuDTGDo
* **DIY Fan Mount STL files**
  * https://www.printables.com/model/554226-120mm-computer-fan-desk-mount
  * https://www.printables.com/model/1324299-pc-desk-fan
  * https://www.printables.com/model/889331-noctua-inspired-desk-fan-mount
* **ir2mqtt** (iRacing telemetry → MQTT) – [https://github.com/jmlt/ir2mqtt](https://github.com/jmlt/ir2mqtt)
* **Moodist** (ambient engine) – [https://github.com/remvze/moodist](https://github.com/remvze/moodist)
---

*© 2025  WindScape. Licensed under the MIT License unless noted otherwise.*

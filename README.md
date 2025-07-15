![WindScape Title](https://github.com/TilmanGriesel/WindScape/blob/main/docs/title.png?raw=true)

# WindScape

*A Home Assistant / ESPHome project that turns everyday PWM‑controlled PC fans into remarkably natural indoor wind simulators.*

WindScape delivers authentic, location‑inspired airflow—from a gentle Mediterranean whisper to a brisk Alpine surge—creating a natural, immersive atmosphere for your workspace, gaming setup, or sim‑racing cockpit. It’s a versatile wind simulator ready to elevate any project you dream up.

## Table of Contents

1. [Features](#features)
1. [Operating Modes](#operating-modes)
1. [Demo](#demo)
1. [Preset Library](#preset-library)
1. [Build Guides](#build-guides)
1. [Software Setup](#software-setup)
1. [Troubleshooting](#troubleshooting)
1. [Tips for Best Experience](#tips-for-best-experience)
1. [Technical Notes](#technical-notes)
1. [Roadmap](#roadmap)
1. [How WindScape Works](#how-windscape-works)
1. [Links](#links)

---

## Features

* **Dynamic Wind‑Simulation Engine**
  Smooth, randomized airflow with realistic transitions between quiet, moderate, and high‑activity “weather” phases.

* **Six Location Presets**
  Pre‑tuned profiles that capture the feel of coastal, mountain, rural, and other outdoor environments.

* **Manual or Automated Speed Control**
  Adjust fan speed directly or let automations handle it.

* **External Wind‑Sensor Support**
  Reacts to live telemetry (e.g. racing‑sim car speed over MQTT) and safely reverts to the last preset if the sensor falls silent.

* **Home Assistant Integration**
  Control, monitor, and automate entirely over Wi‑Fi.

* **Real‑Time RPM Monitoring**
  Optional tachometer feedback for diagnostics.

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

## Build Guides

### 5 V Fan (USB‑Powered)

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

---

### 12 V Fan

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

WindScape layers multiple randomised systems (phase engine, micro‑turbulence, gusts) to avoid predictable sine‑wave patterns and ensure continuous, organic motion. All timings are jittered; phase progression is probability‑based; and user intensity scaling is applied last.

---

## Roadmap

* Native integration with [moodist](https://github.com/remvze/moodist) ambience engine.

---

## How WindScape Works

WindScape cycles through **Quiet**, **Medium**, and **High‑Activity** weather phases, each with its own speed ranges and gust probabilities. A dynamic target algorithm steers the fan toward randomly selected speeds within those ranges, while micro‑turbulence and gust overlays add texture. External sensor mode bypasses the simulation and maps real‑time data directly to fan speed, with automatic safety fallback.

---

## Links

* **Home Assistant** – [https://www.home-assistant.io/](https://www.home-assistant.io/)
* **ESPHome** – [https://esphome.io/](https://esphome.io/)
* **Noctua Fans & Accessories** – [https://noctua.at/](https://noctua.at/)
* **DIY Fan Mount STL files** – [https://www.printables.com/](https://www.printables.com/)
* **ir2mqtt** (iRacing telemetry → MQTT) – [https://github.com/jmlt/ir2mqtt](https://github.com/jmlt/ir2mqtt)
* **Moodist** (ambient engine) – [https://github.com/remvze/moodist](https://github.com/remvze/moodist)

---

*© 2025  WindScape. Licensed under the MIT License unless noted otherwise.*

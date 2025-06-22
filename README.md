![title](https://github.com/TilmanGriesel/WindScape/blob/main/docs/title.png?raw=true)

About a year ago, I picked up the Noctua NV-FS1. Sure, it's arguably overpriced for a desk fan, but as a self-proclaimed Noctua fanboy, I have no regrets. It’s whisper-quiet, has a unique aesthetic, and fits perfectly into my workspace.

That said, one thing always felt missing: a natural breeze. The constant airflow just doesn't replicate the soothing randomness of a gentle seaside wind.

So I built a solution, The Breezer 9000. It’s a PWM controller powered by ESPHome, fully integrated with Home Assistant. It randomizes fan speed to create a calming, natural airflow pattern right at my desk. I loved the result, kept working on it and WindScape was born. If you like this project, leaving a star would be lovely.


## Features

- Wind Simulation Engine
- 6 Location Presets
- 100-level speed control
- Real-time RPM monitoring
- Wireless control via Home Assistant
- OTA firmware updates

## Wind Mode Presets

Transform the atmosphere of your room with realistic wind profiles inspired by beautiful locations around the world. Each preset is designed to mimic the **natural wind behavior** you might feel in these places from gentle seaside breezes to dramatic mountain gusts.


#### 1. Plage du Truc Vert (Atlantic Coast, France)

<img src="https://github.com/TilmanGriesel/WindScape/blob/main/docs/plage.png?raw=true" width="100">

- **Feel:** Strong, steady seaside wind with playful gusts
- **Experience:** Like standing on a breezy beach where ocean winds roll in with energy. Great for a refreshing, invigorating airflow that feels alive and ever-changing.

#### 2. Capri (Mediterranean Island, Italy)

<img src="https://github.com/TilmanGriesel/WindScape/blob/main/docs/capri.png?raw=true" width="100">

- **Feel:** Soft and relaxed coastal breeze
- **Experience:** Gentle wind like what you’d feel while lounging on a terrace overlooking the Mediterranean. Ideal for calm afternoons, reading, or light background airflow.

#### 3. Plateau de Valensole (French Countryside)

<img src="https://github.com/TilmanGriesel/WindScape/blob/main/docs/valensole.png?raw=true" width="100">

- **Feel:** Peaceful, barely-there wind
- **Experience:** Imagine a warm summer day in a quiet lavender field. The air moves softly, almost like a whisper. Perfect for unwinding or sleeping.

#### 4. Fellhorn (Alpine Mountain Range)

<img src="https://github.com/TilmanGriesel/WindScape/blob/main/docs/alpine.png?raw=true" width="100">

- **Feel:** Crisp, lively mountain air with frequent gusts
- **Experience:** Like opening a window in the Alps — cool, brisk, and full of natural movement. Great for simulating fresh, outdoor mountain air indoors.

#### 5. Meseta de Somuncurá (Patagonia, Argentina)

<img src="https://github.com/TilmanGriesel/WindScape/blob/main/docs/patagonia.png?raw=true" width="100">

- **Feel:** Bold and intense wind with powerful surges
- **Experience:** Feels like you’re standing in one of the windiest places on Earth. Ideal for a dramatic atmosphere, energizing effect, or maximum airflow performance.

#### 6. Nærøyfjord (Norwegian Fjords)

<img src="https://github.com/TilmanGriesel/WindScape/blob/main/docs/fjord.png?raw=true" width="100">

- **Feel:** Channeled, dramatic airflow with noticeable shifts
- **Experience:** Like the wind funnelling between tall cliffs, unpredictable and thrilling. Excellent for a unique, immersive airflow experience.

#### 7. Manual Mode

- **Feel:** Whatever you choose
- **Experience:** Full control. No simulation, just direct manual speed adjustment, great for testing, automations or when you want a consistent fan output without variation.

## Quick Setup

### Hardware
- ESP32 development board (Lolin32 Lite recommended)
- fan controller (like the Noctua NA-FC1) https://noctua.at/en/na-fc1
- 4-pin PWM fan (like the Noctua NV-FS1) https://noctua.at/en/nv-fs1
- 12V power supply
- Optional: buck converter for 5V ESP32 power, the ES32 can be powered by USB too.


- Selfmade NV-FS1:
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

- WEMOS LOLIN32 Lite Pinout:
  - https://www.espboards.dev/img/9zjDDWk5sl-1000.png
  - https://www.espboards.dev/esp32/lolin32/
- Useful wiring diagram from KlausMu/esp32-fan-controller: https://raw.githubusercontent.com/wiki/KlausMu/esp32-fan-controller/images/fritzingESP32_BME280_fan.png



![breadboard](https://github.com/TilmanGriesel/ha_esphome_desk_fan/blob/main/docs/img1.png?raw=true)

![assembled](https://github.com/TilmanGriesel/ha_esphome_desk_fan/blob/main/docs/img2.png?raw=true)

### Software
1. Flash the ESPHome config to your ESP32
2. Add WiFi credentials to `secrets.yaml`
3. Device appears automatically in Home Assistant

## Configuration

* **Sliders**:
  * **Wind Intensity** (30–150%)
  * **Gust Frequency** (10–90%)
  * **Fan Speed Limit** (15–100%)
  * **Minimum Fan Speed** (0–50%)
* **Select**:
  * **Wind Mode Presets**: Includes real-world-inspired locations like *Capri*, *Fellhorn*, and *Nærøyfjord*
* **Buttons**:
  * *Quick Gust* – Injects a manual gust event
  * *Calm Wind* – Temporarily suppresses wind
  * *Regenerate Wind Pattern* – Reseeds the Perlin noise engine
  * *Restart* – Reboots the device

## How It Works

**WindScape** simulates wind behavior using a physics-based noise engine driven by real-time parameters and user presets. The airflow is translated into PWM signals to control PC fans.

### Core Principles

* **Base Wind Speed**: Each location preset defines a baseline wind speed (in MPH), mapped to fan duty cycle.
* **Perlin Noise**: 1D multi-octave Perlin noise generates smooth, pseudo-random fluctuations to mimic real-world turbulence.
* **Gust Simulation**: A separate high-frequency noise layer introduces intermittent gusts, whose intensity and frequency are user-configurable.
* **Environmental Profiles**: Presets like *Plage du Truc Vert* or *Fellhorn* adjust parameters like turbulence, gust chance, and noise scaling for different geographic wind behaviors.

### Fan Speed Calculation

1. The total wind strength = `base_speed × (1 + noise layers) × gust_multiplier`
2. This result is constrained (2–25 mph) and linearly mapped to fan speed (15–95%)
3. Safety limits (min/max speed) and intensity scaling are enforced before applying PWM output

### Update Loops

* **Wind Physics**: Runs every `base_update_interval` (e.g. 800ms), updates wind speed and descriptive conditions.
* **Gust Engine**: Runs faster (e.g. 200ms), injecting gusts when noise exceeds a dynamic threshold.

### Diagnostics

Real-time sensors report:

* Simulated wind speed (MPH)
* Gust strength (%)
* Loop timing and physics frequency (Hz)

## Troubleshooting

**Fan not responding?** Check PWM connections and 21kHz compatibility  
**No RPM reading?** Verify tachometer wiring and pullup resistor  
**Breeze mode issues?** Manual speed changes disable breeze mode automatically

## Tips

- Start with defaults, then customize to preference
- Longer cycles feel more natural
- Monitor logs to see breeze events in real-time
- Lower settings work better for focused work

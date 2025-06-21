# Breezer 9000
aka _ha_esphome_desk_fan_

About a year ago, I picked up the Noctua NV-FS1. Sure, it's arguably overpriced for a desk fan, but as a self-proclaimed Noctua fanboy, I have no regrets. It’s whisper-quiet, has a unique aesthetic, and fits perfectly into my workspace.

That said, one thing always felt missing: a natural breeze. The constant airflow just doesn't replicate the soothing randomness of a gentle seaside wind.

So I built a solution, The Breezer 9000. It’s a PWM controller powered by ESPHome, fully integrated with Home Assistant. It randomizes fan speed to create a calming, natural airflow pattern right at my desk. I love the result. Maybe you will too. If you like this project, leaving a star would be lovely.

![title](https://github.com/TilmanGriesel/ha_esphome_desk_fan/blob/main/docs/title.png?raw=true)

## Features

**Seaside Breeze Mode** - Realistic wind patterns with:
- Natural wave-like speed variations
- Random gusts and peaceful lulls
- Fully customizable intensity and timing

**Smart Controls**
- 100-level speed control
- Real-time RPM monitoring
- Wireless control via Home Assistant
- OTA firmware updates

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

All settings adjust live through Home Assistant:

| Setting | Purpose | Default |
|---------|---------|---------|
| **Min/Max Speed** | Base breeze intensity | 20-60% |
| **Gust Max Speed** | Peak wind burst intensity | 80% |
| **Wave Duration** | Cycle length | 5 seconds |
| **Gust Probability** | Frequency of wind bursts | 8% |
| **Lull Probability** | Calm moment frequency | 5% |

![ha_config](https://github.com/TilmanGriesel/ha_esphome_desk_fan/blob/main/docs/ha1.png?raw=true)

![breeze_variation](https://github.com/TilmanGriesel/ha_esphome_desk_fan/blob/main/docs/ha2.png?raw=true)

## Breeze Profiles

**Gentle Ocean**
```
Min: 15%, Max: 40%, Gusts: 60%, Cycle: 10s
```

**Tropical Breeze**
```
Min: 25%, Max: 70%, Gusts: 90%, Cycle: 5s
```

**Night Mode**
```
Min: 10%, Max: 30%, Gusts: 40%, Low probability
```

## How It Works

The wind simulation combines multiple layers:
- Base wave pattern for natural rhythm
- Random variations for unpredictability
- Probabilistic gust and lull events
- Smooth speed transitions

## Troubleshooting

**Fan not responding?** Check PWM connections and 21kHz compatibility  
**No RPM reading?** Verify tachometer wiring and pullup resistor  
**Breeze mode issues?** Manual speed changes disable breeze mode automatically

## Tips

- Start with defaults, then customize to preference
- Longer cycles feel more natural
- Monitor logs to see breeze events in real-time
- Lower settings work better for focused work

# Home Automation — ESPHome + Home Assistant

![ESPHome](https://img.shields.io/badge/ESPHome-devices-000000) ![Home Assistant](https://img.shields.io/badge/Home%20Assistant-automations-41BDF5) ![ESP32](https://img.shields.io/badge/ESP32-esp32dev-blue)

My ESPHome device firmware and Home Assistant automations. Secrets are referenced with `!secret`
and live in a `secrets.yaml` that is not part of this repo (see `esphome/secrets.example.yaml`).

## ESPHome devices

### [GoNiki Air Monitor](esphome/goniki-air-monitor.yaml)
Custom ESPHome firmware for an ESP32 air-quality monitor with a built-in display.
- **SHT3x** temperature/humidity, **SGP4x** VOC + NOx index (temperature/humidity compensated), **PMSx003** particle counts (PM0.3–PM10)
- 240×135 **ST7789** colour display drawn with a C++ lambda: two-column layout, colour-coded values, 4-step battery gauge read from four ADC inputs with moving-average filters
- Backlight controlled by a wake button and a restartable 30 s script; SNTP time

### [Air Conditioner](esphome/air-conditioner.yaml)
Local control of a **Midea** split AC through an ESP32 on the UART service port (SMLIGHT SLWF-01Pro design, ported to ESP32).
- `midea` climate platform: modes, fan speeds, presets, swing, outdoor temperature, power usage
- Template buttons/switches for display toggle, swing step and beeper; Follow-Me service; IR transmitter
- Local web server with authentication

### Filament RFID reader
Lives in the [bambu-ams-rfid](https://github.com/Aleitao2/bambu-ams-rfid) repo.

## Home Assistant automations
| Automation | What it does |
|---|---|
| `filters_runtime_tracker` | Every 15 min adds 0.25 h to the AC filter counter if the AC is running, and to the PC filter counter if the PC is on |
| `filters_cleaning_reminder` | Daily phone notification when AC filter ≥ 250 h / 30 days or PC filters ≥ 500 h / 90 days, with an actionable *Mark as cleaned* button |
| `filters_reset_after_cleaning` | Resets the counter and last-cleaned date from the dashboard button or the notification action, and clears the notification |
| `ac_toggle_button`, `ac_temp_up/down_button` | Dashboard / phone-widget buttons for the AC (toggle cool/off, ±1 °C) |
| `printer_light_follows_chamber_light` | Room light follows the 3D printer's chamber light |

The 3D-printer / AMS / Spoolman automations are documented in [bambu-ams-rfid](https://github.com/Aleitao2/bambu-ams-rfid).

## Credits
Air-conditioner config based on [smlight-dev/slwf-01pro](https://github.com/smlight-dev/slwf-01pro).

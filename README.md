# Pico W Waveshare Touch Display

ESPHome configuration for a Raspberry Pi Pico W with a 240 × 320 Waveshare-style
ST7789V display, CST816 capacitive touchscreen, and a GPIO-connected mmWave
presence sensor.

The display shows motion state, Wi-Fi strength, and uptime. Touch events are
written to the ESPHome log.

Hardware documentation and interface setup instructions are available on the
[Waveshare 2inch Capacitive Touch LCD wiki](https://www.waveshare.com/wiki/2inch_Capacitive_Touch_LCD#Enable_SPI_and_I2C_Interfaces).

## Hardware connections

| Function | Pico W pin |
| --- | --- |
| I²C SDA | GPIO6 |
| I²C SCL | GPIO7 |
| Touch interrupt | GPIO8 |
| Display CS | GPIO9 |
| SPI clock | GPIO10 |
| SPI MOSI | GPIO11 |
| SPI MISO | GPIO12 |
| Display reset | GPIO13 |
| Display DC | GPIO14 |
| Display backlight | GPIO15 |
| mmWave sensor | GPIO26 |

Verify these connections against your particular display board before applying
power.

## Setup

1. Install ESPHome 2025.11.0 or newer.
2. Clone this repository.
3. Copy `secrets.example.yaml` to `secrets.yaml` and enter your Wi-Fi details.
4. Add the two required files described in [`fonts/README.md`](fonts/README.md).
5. Validate the configuration:

   ```sh
   esphome config pico-display.yaml
   ```

6. Connect the Pico W by USB for the first installation:

   ```sh
   esphome run pico-display.yaml
   ```

Later updates can use ESPHome OTA.

## Notes

- `secrets.yaml` is ignored so Wi-Fi credentials are not committed.
- The mmWave input uses GPIO26 with its internal pull-up enabled.
- The touchscreen probe is skipped to match the tested hardware setup.
- The Wi-Fi strength display uses a 15-sample moving average.

## License

Released under the MIT License. See [`LICENSE`](LICENSE).

# Pico W Waveshare Touch Display

ESPHome configuration for a Raspberry Pi Pico W with a portrait 240 × 320
Waveshare-style ST7789V display, CST816 capacitive touchscreen, and a
GPIO-connected mmWave presence sensor.

An optimized configuration for the **Seeed Studio XIAO ESP32-C6** is also
available in
[`door-control-xiao-esp32c6.yaml`](door-control-xiao-esp32c6.yaml). It uses a
40 MHz display SPI rate and a five-second scheduled refresh to keep touch
handling responsive.

The recommended target for the future LVGL interface is the **Seeed Studio
XIAO ESP32-S3**. Its configuration is in
[`door-control-xiao-esp32s3.yaml`](door-control-xiao-esp32s3.yaml). This variant
enables its 8 MB octal PSRAM and uses the native USB Serial/JTAG logger so the
UART pins remain available to the project.

The home screen shows the date and time and provides two large touch controls:

- **Open Door** runs the Home Assistant `script.open_door` action.
- **Leave House** runs the Home Assistant `script.leave_house` action.

The display configuration uses a named `home_page`, and the lower navigation
area is intentionally reserved so additional screens can be added later.

Hardware documentation and interface setup instructions are available on the
[Waveshare 2inch Capacitive Touch LCD wiki](https://www.waveshare.com/wiki/2inch_Capacitive_Touch_LCD#Enable_SPI_and_I2C_Interfaces).

## Hardware connections

### Raspberry Pi Pico W

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

### Seeed Studio XIAO ESP32-C6

| Function | XIAO pin | ESP32-C6 GPIO |
| --- | --- | --- |
| I²C SDA | D4 | GPIO22 |
| I²C SCL | D5 | GPIO23 |
| Touch interrupt | D3 | GPIO21 |
| Display CS | D2 | GPIO2 |
| SPI clock | D8 | GPIO19 |
| SPI MOSI | D10 | GPIO18 |
| SPI MISO | D9 | GPIO20 |
| Display reset | D0 | GPIO0 |
| Display DC | D1 | GPIO1 |
| Display backlight | D6 | GPIO16 |
| mmWave sensor | D7 | GPIO17 |

### Seeed Studio XIAO ESP32-S3

| Function | XIAO pin | ESP32-S3 GPIO |
| --- | --- | --- |
| I²C SDA | D4 | GPIO5 |
| I²C SCL | D5 | GPIO6 |
| Touch interrupt | D3 | GPIO4 |
| Display CS | D2 | GPIO3 |
| SPI clock | D8 | GPIO7 |
| SPI MOSI | D10 | GPIO9 |
| SPI MISO | D9 | GPIO8 |
| Display reset | D0 | GPIO1 |
| Display DC | D1 | GPIO2 |
| Display backlight | D6 | GPIO43 |
| mmWave sensor | D7 | GPIO44 |

Verify these connections against your particular display board before applying
power.

## Setup

1. Install ESPHome 2025.11.0 or newer.
2. Clone this repository.
3. Copy `secrets.example.yaml` to `secrets.yaml` and enter your Wi-Fi details.
4. Add the two required files described in [`fonts/README.md`](fonts/README.md).
5. In Home Assistant, open **Settings → Devices & services → ESPHome**, select
   this device, choose **Configure**, and enable **Allow the device to perform
   Home Assistant actions**.
6. Confirm that these Home Assistant entities exist:

   - `script.open_door`
   - `script.leave_house`

7. Validate the configuration for your board:

   ```sh
   esphome config pico-display.yaml
   ```

   For the XIAO ESP32-C6, use:

   ```sh
   esphome config door-control-xiao-esp32c6.yaml
   ```

   For the XIAO ESP32-S3, use:

   ```sh
   esphome config door-control-xiao-esp32s3.yaml
   ```

8. Connect the controller by USB for the first installation:

   ```sh
   esphome run pico-display.yaml
   ```

   For the XIAO ESP32-C6, use:

   ```sh
   esphome run door-control-xiao-esp32c6.yaml
   ```

   For the XIAO ESP32-S3, use:

   ```sh
   esphome run door-control-xiao-esp32s3.yaml
   ```

Later updates can use ESPHome OTA.

## Notes

- `secrets.yaml` is ignored so Wi-Fi credentials are not committed.
- The mmWave input uses GPIO26 with its internal pull-up enabled.
- The touchscreen probe is skipped to match the tested hardware setup.
- The Wi-Fi strength display uses a 15-sample moving average.
- Touch buttons are scoped to `home_page`, preventing them from firing when a
  future screen is visible.
- If the XIAO display shows corruption with long jumper wires, reduce
  `data_rate` from `40MHz` to `20MHz`.
- GPIO3 (`D2`) is a strapping pin on the ESP32-S3. It is used only as the
  display chip-select output here; do not add an external pull-up or pull-down
  to that signal.

## License

Released under the MIT License. See [`LICENSE`](LICENSE).

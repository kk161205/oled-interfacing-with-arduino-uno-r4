# OLED I2C with Arduino UNO R4

A minimal example that demonstrates how to drive an SSD1306 I2C OLED display from an Arduino UNO R4 using the Adafruit SSD1306 and Adafruit GFX libraries. The project prints a few lines on the OLED from `src/main.cpp`.

## What you get
- Example code that initializes the SSD1306 display and prints a short message.
- PlatformIO configuration that targets `uno_r4_wifi` (see `platformio.ini`).

## Hardware Required
- Arduino UNO R4 (Uno R4 WiFi board is used in `platformio.ini`)
- 0.96" or similar SSD1306 I2C OLED module (128x64)
- Jumper wires
- USB cable to connect UNO R4 to your PC

Important: Verify whether your OLED module requires 3.3V or 5V on VCC. Many modules accept 3.3–5V, but check the module documentation to avoid damage.

## Wiring
Use the UNO R4's dedicated I2C pins (SDA / SCL). If your board doesn't expose labeled SDA/SCL pins, the legacy A4 (SDA) / A5 (SCL) pins are often wired to the same bus — confirm with your board documentation.

Typical connections:
- OLED VCC  -> 3.3V (or 5V if module supports it)
- OLED GND  -> GND
- OLED SDA  -> SDA (UNO R4 SDA pin)
- OLED SCL  -> SCL (UNO R4 SCL pin)

OLED I2C address used in the code: 0x3C (if your module is 0x3D, update the address in `src/main.cpp`).

## Software / Build (PlatformIO)
This project uses PlatformIO. The active environment in `platformio.ini` is `[env:uno_r4_wifi]`.

Open a terminal in the project root and run:

```powershell
# Build
platformio run -e uno_r4_wifi

# Upload to the board (uses upload_port from platformio.ini if set)
platformio run -e uno_r4_wifi -t upload

# Open the serial monitor (alternative: explicitly specify COM port & baud)
platformio device monitor -e uno_r4_wifi
# or
platformio device monitor -p COM15 -b 9600
```

Notes:
- `platformio` can also be called as `pio` depending on your installation.
- `platformio.ini` currently sets `monitor_speed = 9600` and `monitor_port = COM15`.

## Code overview (`src/main.cpp`)
The provided `main.cpp` does the following:
- Includes Adafruit libraries and Wire for I2C.
- Defines `SCREEN_WIDTH`, `SCREEN_HEIGHT`, and `OLED_ADDR` (0x3C).
- Instantiates `Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT);`.
- In `setup()` the code begins serial at 9600 and calls `display.begin(SSD1306_SWITCHCAPVCC, OLED_ADDR)` to initialize the OLED.
- Then it clears the display, sets text size/color, prints three lines and calls `display.display()` to update the screen.
- `loop()` is empty — this is a static demo showing how to initialize and print to the display.

If you want to modify the displayed text, edit the `println()` lines in `setup()` or move display code into `loop()` for dynamic updates.

## Troubleshooting
- OLED shows nothing:
  - Double-check power (VCC) and GND.
  - Confirm SDA/SCL wiring to the correct pins.
  - Run an I2C scanner sketch (or use a helper) to confirm the OLED address (0x3C or 0x3D).
  - If `display.begin()` fails, the code prints "OLED not found" to Serial. Open the serial monitor at 9600 to see this.

- Wrong/garbled characters:
  - Confirm `SCREEN_WIDTH` / `SCREEN_HEIGHT` match your module (128x64 expected).
  - Try `setTextSize()` values and make sure `setTextColor(SSD1306_WHITE)` is used for monochrome displays.

- PlatformIO upload or monitor issues:
  - Verify the correct COM port (change `monitor_port` / `upload_port` in `platformio.ini` or pass `-p COMxx`).
  - Make sure no other program (e.g., Arduino IDE Serial Monitor) is holding the port.

## Dependencies
Declared in `platformio.ini`:
- adafruit/Adafruit GFX Library@^1.12.4
- adafruit/Adafruit SSD1306@^2.5.16

PlatformIO will fetch these automatically when building.

## License & Author
- License: MIT (copy or replace with your preferred license)
- Author: Replace with your name

---
If you'd like, I can:
- Add an I2C scanner sketch into `src/` as a helper file.
- Update the README to include a small wiring diagram image (if you provide one).
- Change the license/author fields to your preferred values.

# Arduino development setup
See the [blog post](https://www.airgradient.com/blog/install-arduino-c3-mini/) for full instructions on how to set up the Arduino IDE with the ESP32 build chain.

The quick version is this:

- File -> Preferences
- In "Additional boards manager URLs" enter: https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_dev_index.json
- Tools -> Board menu -> Boards Manager...
- Search for "ESP32"
- Install the library by Espressif
- Tools -> Board -> esp32 -> Lolin C3 Mini

## Additional settings
The AirGradient One requires a number of additional settings in the Arduino IDE. These can be found in comments at the top of examples/OneOpenAir/OneOpenAir.ino.

- Tools -> Enable USB CDC on boot -> Enabled
- Tools -> Flash frequench -> 80Mhz
- Tools -> Flash mode -> QIO
- Tools -> Flash size -> 4MB (32Mb)
- Tools -> JTAG adapter -> Disabled
- Tools -> Partition scheme -> Minimal SPIFFS (1.9MB APP with OTA/190KB SPIFFS)

# Installing dependencies
In order to use the ESP toolchain, you'll need the Python serial library. On Debian based distros, this is as simple as:

```
sudo apt install python3-serial
```

After getting the system dependencies, you'll also need to get the required Arduino libraries.

- Open the Arduino IDE
- Tools -> Manage Libraries
- Search for and install the AirGradient library

# Identifying your hardware
To know which firmware you should be compiling, see the
[builds overview page](https://www.airgradient.com/documentation/overview/).

This will give you the filename of the Arduino sketch you should be using.

# Compiling
Open the appropriate .ino file and click the verify button (check mark button in the upper left of the Arduino IDE).

# Modifying firmware to be a local server (optional)
By default, your AirGradient air quality monitor will immediately send data to the AirGradient cloud. With the stock firmware, this happens before you are given an opportunity to configure the device to prevent this from happening. However, changing the following line in ./src/AgConfigure.cpp, you can prevent any posts to the server.

Change `#define jprop_postDataToAirGradient_default true' to `#define jprop_postDataToAirGradient_default false'.

This will ensure that your device ID (wifi MAC address) is never sent to a remote server unless you explicitly request this using configuration parameters (see the [local-server documentation](local-server.md) for details).

Making this change makes your air quality monitor more private by default, but also means you can not use the AirGradient Cloud Dashboard by default.

# Flashing to your device
Click the upload button (right arrow next to the verify button in the upper left of the Arduino IDE).

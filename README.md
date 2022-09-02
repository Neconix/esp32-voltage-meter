# ESP32 voltage meter

ADC voltage meter based on ESP32 IDF. Connect the GPIO34 to a potentiometer contact or voltage divider with resistors to measure a voltage level.
I used this project to measure an external lion battery (18650) voltage level with a voltage divider built on two 10 KOhm resistors.

## Hardware Required

* A development board with ESP32 SoC (e.g., ESP32-DevKitC, ESP-WROVER-KIT, etc.);
* A USB cable for power supply and programming.

In this example, we use `ADC_UNIT_1` by default, we need to connect a voltage source (0 ~ 3.3v) to GPIO34. If another ADC unit is selected in your application, you need to change the GPIO pin.

# Build and flash

## ESP-IDF from command line

```shell
git clone https://github.com/Neconix/esp32-voltage-meter.git
cd esp32-voltage-meter
idf.py set-target esp32
idf.py menuconfig
idf.py build
idf.py -p /dev/ttyUSB0 flash # where /dev/ttyUSB0 is a port to connected ESP32 board
idf.py -p /dev/ttyUSB0 monitor # used to see output from ESP_LOG
```
With some Linux distributions, you may get the `Failed to open port /dev/ttyUSB0` error message when flashing the ESP32. Run something like this to add current user to `dialout` group:

```shell
sudo usermod -a -G dialout $USER
```
## VSCode

In VSCode with installed [ESP-IDF extension](https://github.com/espressif/vscode-esp-idf-extension/blob/master/docs/tutorial/install.md):

- git clone https://github.com/Neconix/esp32-voltage-meter.git
- open project folder in VSCode and run from command pallete (`Ctrl + Shift + P`):
- ESP-IDF: Add vscode configuration folder
- ESP-IDF: SDK configuration editor (menuconfig)
- ESP-IDF: Select port to use
- ESP-IDF: Set espressif device target
- ESP-IDF: Build your project
- ESP-IDF: Flash (UART) your project
- ESP-IDF: Monitor your device

Or run ESP-IDF: Build, Flash and start a monitor on your device.

# Example Output

Running this example, you will see the following log output on the serial monitor:

```
Raw: 486	Voltage: 189mV
Raw: 435	Voltage: 177mV
Raw: 225	Voltage: 128mV
Raw: 18	    Voltage: 79mV
```
## Notes about ADC Attenuation

Vref is the reference voltage used internally by ESP32 ADCs for measuring the input voltage. The ESP32 ADCs can measure analog voltages from 0 V to Vref. Among different chips, the Vref varies, the median is 1.1 V. In order to convert voltages larger than Vref, input voltages can be attenuated before being input to the ADCs. There are 4 available attenuation options, the higher the attenuation is, the higher the measurable input voltage could be. See more about ESP32 ADC [here](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/peripherals/adc.html).

### Attenuation
	
Measurable input voltage range:

| Attenuation      | Measurable input voltage range |
| ---------------- |:----------------:|
| ADC_ATTEN_DB_0   | 100 mV ~ 950 mV  |
| ADC_ATTEN_DB_2_5 | 100 mV ~ 1250 mV |
| ADC_ATTEN_DB_6   | 150 mV ~ 1750 mV |
| ADC_ATTEN_DB_11  | 150 mV ~ 2450 mV |

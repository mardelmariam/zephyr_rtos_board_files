.. zephyr:board:: esp32_gen_devkit

Overview
********

The ESP32 Generic Devkit board definition targets an ESP32-D0WDQ6 with two
Xtensa LX6 CPU cores, Wi-Fi, Bluetooth and a 40 MHz crystal. The board's
PROCPU devicetree records a revision 1.0 chip and 4 MiB of SPI flash. It uses
Zephyr's ESP32-WROOM-32UE N4 module definition. Confirm the actual module and
board wiring before applying this configuration to another device.

The board provides separate PROCPU and APPCPU targets. Both use the same
4 MiB AMP flash partition layout, with separate image partitions. The CPU
images share hardware resources and must coordinate their use.

Pin assignments
***************

The PROCPU board definition uses the following pin assignments. The enabled
status refers to the board devicetree; an application must also enable the
appropriate drivers and configure any attached devices.

.. list-table:: PROCPU peripheral pins
   :header-rows: 1
   :widths: 16 16 38 30

   * - Peripheral
     - Default status
     - GPIO assignments
     - Notes
   * - UART0
     - Enabled
     - TX: 1; RX: 3
     - Console and shell, 115200 baud
   * - UART1
     - Disabled
     - TX: 10; RX: 9
     - These pins are connected to SPI flash; do not enable with these assignments
   * - UART2
     - Disabled
     - TX: 17; RX: 16
     -
   * - I2C0
     - Enabled
     - SDA: 21; SCL: 22
     - Standard-mode, 100 kHz
   * - SPI2
     - Enabled
     - MISO: 12; MOSI: 13; clock: 14; chip select: 15
     - Shares GPIO12--15 with JTAG
   * - SPI3
     - Enabled
     - MISO: 19; MOSI: 23; clock: 18; chip select: 5
     -
   * - I2S0
     - Disabled
     - WS: 33; BCK: 32; data out: 25; data in: 34
     -
   * - I2S1
     - Disabled
     - WS: 4; BCK: 26; data out: 27; data in: 35
     -

The BOOT button is assigned to GPIO0, active low. GPIO34, GPIO35, GPIO36 and 
GPIO39 are input-only. GPIO32 and GPIO33 also have 32 kHz crystal functions, 
so check the board wiring before using them for another purpose.

GPIO0, GPIO2, GPIO5, GPIO12 and GPIO15 are boot-strapping pins. External
circuits must allow the required levels during reset. In particular, GPIO12
controls the flash supply voltage at boot on configurations that use the
strapping pin; an inappropriate level can prevent booting.

The module's SPI flash uses GPIO6-11. These pins must not be assigned to
other peripherals in this board configuration. The current UART1 defaults
use GPIO9 and GPIO10, so UART1 must remain disabled unless it is assigned
other suitable pins.

ADC1 and ADC2 are disabled by default. An application can enable the required
controller and configure its channels in an overlay. In Zephyr's ESP32
devicetree, ``adc0`` refers to ESP32 ADC1 and ``adc1`` to ESP32 ADC2. ADC
channels correspond to fixed GPIOs.

Programming and debugging
*************************

The board configuration supports serial programming through the ESP32
bootloader. Select the PROCPU or APPCPU board target as appropriate for the
application and its multicore setup. UART0 is the PROCPU console by default.

JTAG debugging requires a compatible external adapter. The supplied
``support/openocd.cfg`` selects an FTDI interface and single-core debugging;
adjust the interface configuration for the adapter in use. JTAG uses
GPIO12-15, which are also assigned to SPI2. Disable SPI2 when using these
pins for JTAG debugging and ensure that connected hardware does not interfere
with the debug signals or boot strapping levels.

References
**********

* `ESP32-WROOM-32 datasheet`_
* `ESP32-WROOM-32E and ESP32-WROOM-32UE datasheet`_
* `ESP32 GPIO documentation`_
* `ESP32 JTAG debugging`_

.. _ESP32-WROOM-32 datasheet: https://documentation.espressif.com/esp32-wroom-32_datasheet_en.html
.. _ESP32-WROOM-32E and ESP32-WROOM-32UE datasheet: https://documentation.espressif.com/esp32-wroom-32e_esp32-wroom-32ue_datasheet_en.html
.. _ESP32 GPIO documentation: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/peripherals/gpio.html
.. _ESP32 JTAG debugging: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/jtag-debugging/index.html

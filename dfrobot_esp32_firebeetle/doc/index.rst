.. zephyr:board:: dfrobot_esp32_firebeetle

Overview
********

The DFRobot FireBeetle ESP32 is a compact development board based on the
ESP32, with two Xtensa LX6 CPU cores, 2.4 GHz Wi-Fi and Bluetooth. It can be
powered over USB or from a 3.7 V lithium battery and includes battery charging
circuitry. See the `DFRobot FireBeetle ESP32 documentation`_ for the board
pinout and power connections.

This board definition describes an ESP32-D0WDQ6 with a 40 MHz crystal. The
PROCPU devicetree uses the ESP32-WROOM-32UE N8 module definition with 8 MiB
of flash, matching the flash size recorded in the board DTS comments. Verify
the module and flash capacity on your own board; FireBeetle variants can
differ.

The board provides ``dfrobot_esp32_firebeetle/esp32/procpu`` and
``dfrobot_esp32_firebeetle/esp32/appcpu`` targets. Both use the AMP flash
partition layout defined for 4 MiB, with separate image partitions. The APPCPU
devicetree also describes 4 MiB of flash. The CPU images share hardware
resources and must coordinate their use.

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
     - Disabled
     - MISO: 12; MOSI: 13; clock: 14; chip select: 15
     - Shares GPIO12-15 with JTAG
   * - SPI3
     - Disabled
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

The board definition assigns a BOOT button to GPIO0, active low. The PROCPU
DTS comments identify GPIO2 as connected to an onboard LED. No LED alias is 
defined in the board devicetree. GPIO34, GPIO35, GPIO36 and GPIO39 are 
input-only. 
GPIO32 and GPIO33 also have 32 kHz crystal functions, so check the board
wiring before using them for another purpose.

GPIO0, GPIO5, GPIO12 and GPIO15 are boot-strapping pins. External
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

* `DFRobot FireBeetle ESP32 documentation`_
* `ESP32-WROOM-32 datasheet`_
* `ESP32-WROOM-32E and ESP32-WROOM-32UE datasheet`_
* `ESP32 GPIO documentation`_
* `ESP32 JTAG debugging`_

.. _DFRobot FireBeetle ESP32 documentation: https://wiki.dfrobot.com/dfr0478/
.. _ESP32-WROOM-32 datasheet: https://documentation.espressif.com/esp32-wroom-32_datasheet_en.html
.. _ESP32-WROOM-32E and ESP32-WROOM-32UE datasheet: https://documentation.espressif.com/esp32-wroom-32e_esp32-wroom-32ue_datasheet_en.html
.. _ESP32 GPIO documentation: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/peripherals/gpio.html
.. _ESP32 JTAG debugging: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/jtag-debugging/index.html

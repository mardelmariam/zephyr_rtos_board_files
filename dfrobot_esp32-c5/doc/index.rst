.. zephyr:board:: dfrobot_esp32-c5

Overview
********

The DFRobot FireBeetle 2 ESP32-C5 board uses an ESP32-C5 with a RISC-V
high-performance (HP) core and a low-power (LP) core. The HP-core DTS records
an ESP32-C5 revision 1.0, a 240 MHz CPU, a 48 MHz crystal and 4 MiB of SPI
flash. It also records dual-band Wi-Fi 6, Bluetooth LE 5 and IEEE 802.15.4
capabilities. These comments describe the inspected device; device-specific
values, such as the MAC address and security state, may differ on other boards.

The Zephyr board targets are ``dfrobot_esp32-c5/esp32c5/hpcore`` and
``dfrobot_esp32-c5/esp32c5/lpcore``. The HP-core YAML lists 384 KiB of RAM
and support for ADC, CAN, GPIO, watchdog, UART, DMA, PWM, SPI, counter,
entropy, I2C, I2S, OpenThread, Wi-Fi, cryptography, retained memory and
pulse I/O. The LP-core YAML lists CPU and serial/UART support. These lists
are capabilities, not a guarantee that every peripheral is enabled or wired
for an application.

In the HP-core DTS, GPIO, watchdog, hardware random number generator, core
temperature sensor, USB Serial/JTAG and I2C0 are enabled. Wi-Fi, Bluetooth
HCI, IEEE 802.15.4, SPI2, UART0 and PSRAM are explicitly disabled. The USB
serial interface is selected for the console and shell; the board defconfig
enables console, serial, logging and GPIO. Applications may need additional
driver configuration and devicetree overlays.

Pins
****

The following assignments come from the HP-core DTS and shared pin control
file. Pin control entries alone do not enable a peripheral.

.. list-table:: Board pin assignments
   :header-rows: 1
   :widths: 22 38 40

   * - Function
     - GPIOs
     - Notes
   * - BOOT button
     - 28
     - ``sw0``; active low, internal pull-up
   * - User LED
     - 15
     - ``led0``; active high
   * - USB Serial/JTAG
     - 13, 14
     - Console and shell; USB D- and D+, respectively
   * - UART0
     - TX 11, RX 12
     - Disabled on the HP core; configured for 115200 baud
   * - I2C0
     - SDA 9, SCL 10
     - Enabled, standard mode (100 kHz), open drain with pull-ups
   * - SPI2
     - MISO 25, clock 23, CS 26, MOSI 24
     - Disabled on the HP core
   * - I2S
     - MCLK 0, WS 3, BCK 4, data out 5, data in 8
     - Pin control is defined; check controller status and board wiring before use
   * - UART1
     - TX 5, RX 4
     - Pin control is defined; check controller status before use

The pinctrl file also defines an I2C sleep state on GPIO6 and GPIO7; the
HP-core I2C node selects only the default state on GPIO9 and GPIO10.

The HP-core DTS comments identify GPIO8, 9, 10, 23 and 24 as available,
subject to their configured functions. They flag GPIO6 and 7 for LP I2C,
GPIO2--5 for JTAG, and GPIO25--27 for SPI or strapping. They reserve GPIO0
for power control, GPIO1 for battery monitoring, GPIO11 and 12 for UART0,
GPIO13 and 14 for USB, and GPIO15 for the LED or optional PSRAM chip select.
The comments also associate GPIO19 with flash power and GPIO27 with VDD_SPI.
GPIO16, 17, 18, 20, 21 and 22 are connected to the integrated flash and must
not be reused while it is in use.

Some pinctrl assignments overlap these reservations: for example, I2S MCLK
uses GPIO0, I2S and UART1 use GPIO4 and 5, and SPI2 uses GPIO25 and 26.
Check the actual board wiring, boot strapping requirements, and peripheral
configuration before enabling or remapping a function. The SPI2 DTS comment
claims GPIO12--15 are SPI2 and JTAG pins, but its actual pinctrl assignments
are GPIO23--26. The JTAG notes in the same file also identify GPIO2--5; do not
rely on the conflicting SPI2 comment as a pin map.

Flash partitions
****************

The HP-core DTS includes the board-local ``partitions_0x2000_4M.dtsi`` and
selects ``image-0`` as its code partition. The file describes a 4 MiB flash
layout whose MCUboot partition begins at offset ``0x2000``. It provides a
4 MiB layout with that bootloader offset for this board; the files do not
record the author's original reason for creating it. The first 8 KiB are
outside the listed partitions, and the remaining space through ``0x3fffff``
is assigned as follows:

.. list-table:: HP-core flash layout
   :header-rows: 1
   :widths: 25 18 18 39

   * - Label
     - Offset
     - Size
     - Intended use
   * - ``mcuboot``
     - ``0x002000``
     - 56 KiB
     - MCUboot bootloader
   * - ``sys``
     - ``0x010000``
     - 64 KiB
     - System partition
   * - ``image-0``
     - ``0x020000``
     - 1792 KiB
     - Primary HP application slot
   * - ``image-1``
     - ``0x1e0000``
     - 1792 KiB
     - Secondary HP image slot
   * - ``image-0-lpcore``
     - ``0x3a0000``
     - 32 KiB
     - Primary LP image slot
   * - ``image-1-lpcore``
     - ``0x3a8000``
     - 32 KiB
     - Secondary LP image slot
   * - ``storage``
     - ``0x3b0000``
     - 192 KiB
     - Storage
   * - ``image-scratch``
     - ``0x3e0000``
     - 124 KiB
     - Scratch space for an applicable MCUboot upgrade mode
   * - ``coredump``
     - ``0x3ff000``
     - 4 KiB
     - Coredump storage

The partition definitions reserve space; they do not by themselves enable
firmware updates, storage or coredumps. Sysbuild defaults to MCUboot with no
signature type selected in ``Kconfig.sysbuild``. Check the bootloader and
application configuration before using updates or security features.

The LP-core DTS instead includes Zephyr's ``partitions_0x0_default_4M.dtsi``
and selects its ``slot0_lpcore_partition``. In the supplied Zephyr tree,
that layout has the same application and LP image offsets, but its MCUboot
partition starts at ``0x0`` and is 64 KiB long. Ensure that the bootloader
and both core images use compatible offsets when combining and flashing them.

Flashing and debugging
**********************

The board's ``board.cmake`` selects the ESP32 runner for flashing and OpenOCD
for debugging. Install the Zephyr Espressif toolchain and its compatible
Espressif OpenOCD, and connect the board's USB Serial/JTAG port to the host.
For example, from a Zephyr workspace, build and flash the HP-core hello-world
sample with sysbuild:

.. code-block:: console

   west build -b dfrobot_esp32-c5/esp32c5/hpcore --sysbuild zephyr/samples/hello_world
   west flash

Sysbuild builds the configured bootloader and application images. Select the
serial device if the runner cannot identify it automatically. The HP-core
console uses USB serial. If the chip remains in download mode after flashing,
try ``west flash --reset-type watchdog-reset`` or power-cycle the board.

The ESP32-C5 provides built-in JTAG through USB Serial/JTAG on GPIO13 (D-)
and GPIO14 (D+); these pins must remain available for USB debugging. The
supplied ``support/openocd.cfg``, however, selects an FTDI adapter and the
original ESP32 target, not the ESP32-C5. It must be replaced or overridden
with an ESP32-C5 configuration before using it for this board. For built-in
USB JTAG, the Zephyr ESP32-C5 DevKit configuration uses:

.. code-block:: tcl

   source [find board/esp32c5-builtin.cfg]

Use that configuration with Espressif's OpenOCD and its matching scripts.
After flashing the required images, start an OpenOCD debug session for the HP
application, for example with ``west debug`` from the application build
context. For a sysbuild, select the application domain using ``west debug
--domain <application-domain>``; its name is listed in ``build/domains.yaml``.
The board's default flash runner is serial, so ``west flash`` above does not
flash through JTAG. If JTAG flashing is required, configure a compatible
OpenOCD runner and verify that all required bootloader and application images
are programmed at the correct offsets.

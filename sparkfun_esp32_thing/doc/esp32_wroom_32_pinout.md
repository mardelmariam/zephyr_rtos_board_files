# ESP-WROOM-32 pinout

Pin labels transcribed from the ESP-WROOM-32 pinout image and verified by the user. The numbers are the gray **physical pin** numbers shown in the image. The diagram marks the GPIO entries with a PWM symbol.

## Left side, top to bottom

| Pin | Physical pin | Other labels |
|---|---:|---|
| GND | — | |
| 3.3V | — | |
| ChipPU | 9 | |
| GPIO36 | 5 | SensVP, ADC1_0, RTCIO0, ADCPA |
| GPIO39 | 8 | SensVN, ADC1_3, RTCIO3, ADCPA |
| GPIO34 | 10 | VDET1, ADC1_6, RTCIO4 |
| GPIO35 | 11 | VDET2, ADC1_7, RTCIO5 |
| GPIO32 | 12 | ADC1_4, RTCIO9, Touch9, XTAL32 |
| GPIO33 | 13 | ADC1_5, RTCIO8, Touch8, XTAL32 |
| GPIO25 | 14 | EMACRXD0, ADC2_8, RTCIO6, DAC_1 |
| GPIO26 | 15 | EMACRXD1, ADC2_9, RTCIO7, DAC_2 |
| GPIO27 | 16 | EMACRXDV, ADC2_7, RTCIO17, Touch7 |
| GPIO14 | 17 | EMACTXD2, ADC2_6, RTCIO16, Touch6, MTMS, HSPICLK, SPCLK, HS2CLK |
| GPIO12 | 18 | EMACTXD3, ADC2_5, RTCIO15, Touch5, MTDI, HSPIQ, SDData2, HS2DATA2 |

## Bottom side, left to right

| Pin | Physical pin | Other labels |
|---|---:|---|
| GND | — | |
| GPIO13 | 20 | EMACRXER, ADC2_4, RTCIO14, Touch4, MTCK, HSPID, SDData3, HS2DATA3 |
| GPIO9 | 28 | SPIHD, U1RXD, HS1DATA2, SDData2 |
| GPIO10 | 29 | SPIWP, U1TXD, HS1DATA3, SDData3 |
| GPIO11 | 30 | SPICS0, U1RTS, HS1CMD, SDCMD |
| GPIO6 | 31 | SPICLK, U1CTS, HS1CLK, SDCLK |
| GPIO7 | 32 | SPIQ, U2RTS, HS1DATA0, SDData0 |
| GPIO8 | 33 | SPID, U2CTS, HS1DATA1, SDData1 |
| GPIO15 | 21 | EMACRXD3, ADC2_3, RTCIO13, Touch3, MTDO, HSPICS0, SDCMD, HS2CMD |
| GPIO2 | 22 | HSPIWP, ADC2_2, RTCIO12, Touch2 |

## Right side, top to bottom

| Pin | Physical pin | Other labels |
|---|---:|---|
| GND | — | |
| GPIO23 | 36 | VSPID, HS1STROBE |
| GPIO22 | 39 | EMACTXD1, U0RTS, VSPIWP |
| GPIO1 | 41 | EMACTXD2, U0TXD, CLKOUT3; marked as program port |
| GPIO3 | 40 | U0RXD, CLKOUT2; marked as program port |
| GPIO21 | 42 | EMACTXEN, VSPIHD |
| NC | — | |
| GPIO19 | 38 | EMACTXD0, U0CTS, VSPIQ |
| GPIO18 | 35 | VSPICLK, HS1DATA7 |
| GPIO5 | 34 | EMACRXCLK, VSPICS0, HS1DATA6 |
| GPIO17 | 27 | EMACCOL, U2TXD, HS1DATA5 |
| GPIO16 | 25 | EMACCLKOUT, U2RXD, HS1DATA4 |
| GPIO4 | 24 | EMACTXER, ADC2_0, RTCIO10, Touch0, HSPIHD, SDData1, HS2DATA1 |
| GPIO0 | 23 | EMACTXCLK, ADC2_1, RTCIO11, Touch1, CLKOUT1 |

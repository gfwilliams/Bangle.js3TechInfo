# Bangle.js3 Tech Info

Initial pin mappings for Bangle.js 3

### Notes:

* I've named certain elements in caps, eg PIN/LIPOCHARGE/etc
* `TEST_POINT` means this should go to a test point on the PCB
* IO is very tight - I've flagged unused IO pins with `UNUSED`

## Power Supply

* ESP8684 states it needs 3.3v at no less than 500mA
* I think it makes sense to run nRF54L15/etc at 3.3v to avoid having to level-shift to ESP8684/others (although I'd love to use 1.8v if we think it's possible?)
* LCD VDD2 needs 5v (4.75-5.25v) but at ~200uA - it might be possible to capacitively voltage-double this off FRP which runs at 60Hz, since 0.7v diodes would produce 5.2v? Otherwise the RGB LEDs on the torch would prefer 30mA@5v to using VBAT so we could power them off the 5v rail rather than VBAT if it was powerful enough.
* If there were a PMIC that handled all this and *also* reported power usage, that would be amazing. If it reports battery voltage we could remove the need to a potential divider to measure voltage on the nRF54
* Is there a PMIC that does LiPo charging too?
* Depending on quiescent current of components (mic, RGB leds, vibration IC, etc) we might need an I2C power switch on I2CBUS to ensure low power consumption?

5v for LCD:

![circuit diagram](data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPHN2ZyB3aWR0aD0iMjAwIiBoZWlnaHQ9IjExMyIgdmVyc2lvbj0iMS4xIiB2aWV3Qm94PSIwIDAgMjAwIDExMyIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48ZGVzYz52b2x0YWdlX211bHQuZHhmIC0gc2NhbGUgPSAxLjAwMDAwMCwgb3JpZ2luID0gKDAuMDAwMDAwLCAwLjAwMDAwMCksIG1ldGhvZCA9IG1hbnVhbDwvZGVzYz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMTkgLTg1NikiPjxnIGZpbGw9Im5vbmUiPjxnIHN0cm9rZT0iIzAwOTgwMCI+PHBhdGggZD0ibTEzNCA5MTFoOS42Ii8+PHBhdGggZD0ibTE0NCA5MjF2LTkuNiIvPjxwYXRoIGQ9Im0xNDQgOTExaDM4LjQiLz48cGF0aCBkPSJtMTQ0IDk1MHY5LjYiLz48cGF0aCBkPSJtMTE1IDkxMWgtOS42Ii8+PHBhdGggZD0ibTEwNiA5MTFoLTkuNiIvPjxwYXRoIGQ9Im0xMDYgOTAydjkuNiIvPjxwYXRoIGQ9Im02Ny4yIDkxMWgtMTkuMiIvPjxwYXRoIGQ9Im0xMDYgODgzdi0xOS4yIi8+PC9nPjxnIHN0cm9rZT0iIzkwMCI+PHBhdGggZD0ibTg2LjQgOTExaC0xLjkyIi8+PHBhdGggZD0ibTc2LjggOTExaDEuOTIiLz48cGF0aCBkPSJtNjcuMiA5MTFoOS42Ii8+PHBhdGggZD0ibTk2IDkxMWgtOS42Ii8+PHBhdGggZD0ibTE0NCA5MzF2MS45MiIvPjxwYXRoIGQ9Im0xNDQgOTQwdi0xLjkyIi8+PHBhdGggZD0ibTE0NCA5NTB2LTkuNiIvPjxwYXRoIGQ9Im0xNDQgOTIxdjkuNiIvPjxwYXRoIGQ9Im0xMDEgODg3IDQuOCA5LjYiLz48cGF0aCBkPSJtMTA2IDg5NyA0LjgtOS42Ii8+PHBhdGggZD0ibTExMCA4OTdoLTQuOCIvPjxwYXRoIGQ9Im0xMTAgODg3aC05LjYiLz48cGF0aCBkPSJtMTA2IDg5N2gtNC44Ii8+PHBhdGggZD0ibTEwNiA4ODN2OS42Ii8+PHBhdGggZD0ibTEwNiA5MDJ2LTkuNiIvPjxwYXRoIGQ9Im0xMjAgOTE2IDkuNi00LjgiLz48cGF0aCBkPSJtMTMwIDkxMS05LjYtNC44Ii8+PHBhdGggZD0ibTEzMCA5MDd2NC44Ii8+PHBhdGggZD0ibTEyMCA5MDd2OS42Ii8+PHBhdGggZD0ibTEzMCA5MTF2NC44Ii8+PHBhdGggZD0ibTExNSA5MTFoOS42Ii8+PHBhdGggZD0ibTEzNCA5MTFoLTkuNiIvPjxwYXRoIGQ9Im03OS4yIDkxNnYtOS42Ii8+PHBhdGggZD0ibTg0IDkxNnYtOS42Ii8+PHBhdGggZD0ibTEzOSA5MzNoOS42Ii8+PHBhdGggZD0ibTEzOSA5MzhoOS42Ii8+PC9nPjwvZz48ZyBmb250LWZhbWlseT0ic2Fucy1zZXJpZiIgZm9udC1zaXplPSIxMy4zcHgiIGxldHRlci1zcGFjaW5nPSIwcHgiIHdvcmQtc3BhY2luZz0iMHB4Ij48dGV4dCB4PSIxMDguMTE0NzgiIHk9Ijg3My4xMTYyMSIgc3R5bGU9ImxpbmUtaGVpZ2h0OjEuMjUiIHhtbDpzcGFjZT0icHJlc2VydmUiPjx0c3BhbiB4PSIxMDguMTE0NzgiIHk9Ijg3My4xMTYyMSI+My4zdjwvdHNwYW4+PC90ZXh0Pjx0ZXh0IHg9IjE2NC43NjU0OSIgeT0iOTA4LjM2NDk5IiBzdHlsZT0ibGluZS1oZWlnaHQ6MS4yNSIgeG1sOnNwYWNlPSJwcmVzZXJ2ZSI+PHRzcGFuIHg9IjE2NC43NjU0OSIgeT0iOTA4LjM2NDk5Ij5+NS4ydjwvdHNwYW4+PC90ZXh0Pjx0ZXh0IHg9IjIwLjM0MDEwNyIgeT0iOTI3LjUxNjc4IiBzdHlsZT0ibGluZS1oZWlnaHQ6MS4yNSIgeG1sOnNwYWNlPSJwcmVzZXJ2ZSI+PHRzcGFuIHg9IjIwLjM0MDEwNyIgeT0iOTI3LjUxNjc4Ij5GUlBANjBIejwvdHNwYW4+PHRzcGFuIHg9IjIwLjM0MDEwNyIgeT0iOTQ0LjE4MzQxIi8+PC90ZXh0Pjx0ZXh0IHg9IjE0NS4wODQ3IiB5PSI5NjMuODgxNTMiIHN0eWxlPSJsaW5lLWhlaWdodDoxLjI1IiB4bWw6c3BhY2U9InByZXNlcnZlIj48dHNwYW4geD0iMTQ1LjA4NDciIHk9Ijk2My44ODE1MyI+R05EPC90c3Bhbj48L3RleHQ+PC9nPjwvZz48L3N2Zz4K)

## LIPOCHARGE: LiPo charger

If a PMIC doesn't have charging built in, can we have LiPo charger like BQ25120A (which has I2C communications for battery voltage) and connect it to I2CBUS? 

We don't have enough free IO to get battery voltage+charge status into the microcontroller.

## 4PIN: 4 Pin side connector:

Allows debug and charge, but pins are also usable as IO, with pin 4 able to supply switched battery power.

* 1: GND
* 2: to TS5A23157 COM1 (SWDCLK)
* 3: to TS5A23157 COM2 (SWDIO)
* 4: diode **to** LiPo battery charger, second diode **from** 4PINPFET_DRAIN

![circuit diagram](data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPHN2ZyB3aWR0aD0iMTc5IiBoZWlnaHQ9IjY4LjQiIHZlcnNpb249IjEuMSIgdmlld0JveD0iMCAwIDE3OSA2OC40IiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciPjxkZXNjPnZvbHRhZ2VfbXVsdC5keGYgLSBzY2FsZSA9IDEuMDAwMDAwLCBvcmlnaW4gPSAoMC4wMDAwMDAsIDAuMDAwMDAwKSwgbWV0aG9kID0gbWFudWFsPC9kZXNjPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0yMi44IC04OTcpIj48ZyBmaWxsPSJub25lIj48ZyBzdHJva2U9IiMwMDk4MDAiPjxwYXRoIGQ9Im05Ni45IDkyOWgzOC40Ii8+PHBhdGggZD0ibTE0OCA5NDd2OS42Ii8+PHBhdGggZD0ibTEyMiA5MTFoLTI2LjEiLz48cGF0aCBkPSJtNzcuNCA5MTFoLTI5LjQiLz48cGF0aCBkPSJtNzMuMyA5MzB2LTE5LjIiLz48cGF0aCBkPSJtNzguMiA5MzBoLTQuNTQiLz48L2c+PHBhdGggZD0ibTEzNSA5MjloOS42IiBzdHJva2U9IiM5MDAiLz48L2c+PGcgdHJhbnNmb3JtPSJyb3RhdGUoOTAgNzcuOSA5MDIpIiBmaWxsPSJub25lIiBzdHJva2U9IiM5MDAiPjxwYXRoIGQ9Im0xMDEgODg3IDQuOCA5LjYiLz48cGF0aCBkPSJtMTA2IDg5NyA0LjgtOS42Ii8+PHBhdGggZD0ibTExMCA4OTdoLTQuOCIvPjxwYXRoIGQ9Im0xMTAgODg3aC05LjYiLz48cGF0aCBkPSJtMTA2IDg5N2gtNC44Ii8+PHBhdGggZD0ibTEwNiA4ODN2OS42Ii8+PHBhdGggZD0ibTEwNiA5MDJ2LTkuNiIvPjwvZz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMzggLS4wMzQ1KSIgZmlsbD0ibm9uZSIgc3Ryb2tlPSIjOTAwIj48cGF0aCBkPSJtMTIwIDkxNiA5LjYtNC44Ii8+PHBhdGggZD0ibTEzMCA5MTEtOS42LTQuOCIvPjxwYXRoIGQ9Im0xMzAgOTA3djQuOCIvPjxwYXRoIGQ9Im0xMjAgOTA3djkuNiIvPjxwYXRoIGQ9Im0xMzAgOTExdjQuOCIvPjxwYXRoIGQ9Im0xMTUgOTExaDkuNiIvPjxwYXRoIGQ9Im0xMzQgOTExaC05LjYiLz48L2c+PHBhdGggZD0ibTE0OCA5NDd2LTYuODEiIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzkwMCIvPjxwYXRoIGQ9Im0xNDQgOTM5di05LjYiIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzkwMCIvPjxnIGZvbnQtZmFtaWx5PSJzYW5zLXNlcmlmIiBsZXR0ZXItc3BhY2luZz0iMHB4IiB3b3JkLXNwYWNpbmc9IjBweCI+PHRleHQgeD0iMTIzLjY5NSIgeT0iOTE1LjU1Mjg2IiBmb250LXNpemU9IjEzLjNweCIgc3R5bGU9ImxpbmUtaGVpZ2h0OjEuMjUiIHhtbDpzcGFjZT0icHJlc2VydmUiPjx0c3BhbiB4PSIxMjMuNjk1IiB5PSI5MTUuNTUyODYiPkxpUG8gY2hhcmdlcjwvdHNwYW4+PC90ZXh0Pjx0ZXh0IHg9IjIyLjQ4NTgxNSIgeT0iOTA2LjQ1NDkiIGZvbnQtc2l6ZT0iMTMuM3B4IiBzdHlsZT0ibGluZS1oZWlnaHQ6MS4yNSIgeG1sOnNwYWNlPSJwcmVzZXJ2ZSI+PHRzcGFuIHg9IjIyLjQ4NTgxNSIgeT0iOTA2LjQ1NDkiPjRQSU48L3RzcGFuPjx0c3BhbiB4PSIyMi40ODU4MTUiIHk9IjkyMy4xMjE1MiIvPjwvdGV4dD48dGV4dCB4PSIxMTUuOTI0NTEiIHk9Ijk2My45NTE2IiBmb250LXNpemU9IjguNHB4IiBzdHlsZT0ibGluZS1oZWlnaHQ6MS4yNSIgeG1sOnNwYWNlPSJwcmVzZXJ2ZSI+PHRzcGFuIHg9IjExNS45MjQ1MSIgeT0iOTYzLjk1MTYiPjRQSU5QRkVUX0dBVEU8L3RzcGFuPjwvdGV4dD48L2c+PGcgZmlsbD0ibm9uZSIgc3Ryb2tlPSIjOTAwIj48cGF0aCBkPSJtMTUyIDkzOXYtOS42Ii8+PHBhdGggZD0ibTE1MSA5MjloOS42Ii8+PHBhdGggZD0ibTE0MiA5MzloMTEuOSIvPjxwYXRoIGQ9Im0xNDIgOTQxaDExLjkiLz48L2c+PHRleHQgeD0iMTYxLjE1Nzg4IiB5PSI5MzQuMjI2NjgiIGZvbnQtZmFtaWx5PSJzYW5zLXNlcmlmIiBmb250LXNpemU9IjEzLjNweCIgbGV0dGVyLXNwYWNpbmc9IjBweCIgd29yZC1zcGFjaW5nPSIwcHgiIHN0eWxlPSJsaW5lLWhlaWdodDoxLjI1IiB4bWw6c3BhY2U9InByZXNlcnZlIj48dHNwYW4geD0iMTYxLjE1Nzg4IiB5PSI5MzQuMjI2NjgiPkxpUG8rPC90c3Bhbj48L3RleHQ+PC9nPjwvc3ZnPgo=)

## 4PINPFET: allow voltage output on 4PIN connector

Should be 500mA+ rated, 3.3v. When 4PINPFET_GATE is at 3.3v and LiPo+ is at ~4.2v, gate voltage should be low enough that PFET is effectively off

* 4PINPFET_SOURCE: LiPo+
* 4PINPFET_DRAIN: diode to 4PIN pin 4
* 4PINPFET_GATE: resistor to LiPo+, 0.7v diode to nRF54L15 (to avoid over-voltage on IO)

## ASWITCH: Analog Switch for 4 Pin connector

Use TS5A23157/similar (https://www.ti.com/lit/ds/symlink/ts5a23157.pdf)
This will switch between SWD on the nRF54 and also configurable IO, allowing SWD to be turned off and protected.

* IN1/IN2: joined, to LCDMCU (if 0=NC, 1=NO)
* COM1: 4PIN pin 2
* COM2: 4PIN pin 3
* NC1: nRF54L15_EXT1
* NC2: nRF54L15_EXT2
* N01: nRF54L15_SWDCLK
* N02: nRF54L15_SWDIO


## nRF54L15: Assuming we're using the WLCSP300 BGA

See https://www.mouser.com/datasheet/2/297/nRF54L15_nRF54L10_nRF54L05_Datasheet_v0_8-3568773.pdf page 808 for reference schematic for wiring.

* XC1/XC2: 32MHz Crystal
* DECA, DECD, VSS, DCC, DECRF, VDD, VSS_PA: Power supply
* SWDCLK/SWDIO: Connect to TEST_POINT, ASWITCH, and LCDMCU
* ANT -> antenna
* nRESET -> LCDMCU
* P0.00 -> LCDMCU_SPI_SCK
* P0.01 -> LCDMCU_SPI_MOSI
* P0.02 -> LCDMCU_SPI_MISO
* P0.03 (RTC_OUT): UNUSED
* P0.04 -> LCDMCU_SPI_NSS
* P1.00/P1.01: 32.768kHz crystal (must use this pin)
* P1.02/P1.03: NFC (must use this pin)
* P1.04/AIN0 -> nRF54L15_EXT1 (to ASWITCH, FLEXPCB)
* P1.05/AIN1 -> nRF54L15_EXT2 (to ASWITCH, FLEXPCB)
* P1.06/AIN2 -> 4PINPFET_GATE
* P1.07/AIN3 -> TORCH_RGB
* P1.08 -> SPEAKER
* P1.09 -> LCDMCU_SWDIO
* P1.10 -> LCDMCU_SWDCLK
* P1.11/AIN4 -> I2CBUS_SDA
* P1.12/AIN5 -> I2CBUS_SCL
* P1.13/AIN6 -> TOUCHSCREEN_IRQ
* P1.14/AIN7 -> MICROPHONE_CLK
* P1.15 -> MICROPHONE_DATA
* P2.00 -> QSPIFLASH D3 (must use this pin)
* P2.01 -> QSPIFLASH SCK (must use this pin)
* P2.02 -> QSPIFLASH D0 (must use this pin)
* P2.03 -> QSPIFLASH D2 (must use this pin)
* P2.04 -> QSPIFLASH D1 (must use this pin)
* P2.05 -> QSPIFLASH CS (must use this pin)
* P2.06 -> ESP8684_BOOT
* P2.07 -> ESP8684_TX (UARTE00/21 RX)
* P2.08 -> ESP8684_RX (UARTE00/21 TX)
* P2.09 -> ESP8684_EN
* P2.10 -> LCDMCU_IRQ

**Notes:**

* QSPI Flash has to stay where it is, but most other pins could move around to help routing
* Do we need interrupt pins for HRM/acc/mag/touch/gas? These are not vital as should have FIFOs and could be polled.


## I2CBUS

Use for HR/Gas sensor/Mag/Accel/Vibration controller on Flex PCB

If we have a PMIC and it supports I2C we could add it too.

* I2CBUS_SDA
* I2CBUS_SCL

## FLEXPCB - Flex PCB with HRM/etc on it

Add pads that users can solder to for:

* GND
* VCC 3.3v
* VBAT (if possible)
* I2CBUS_SDA/I2CBUS_SCL
* nRF54L15_EXT1/nRF54L15_EXT2


## TORCH

Contains one high-power white LED controlled by FET, two 
SK6805-EC15/similar (single-wire addressable RGB LED), and IR transmitter (next heading)

* TORCH_FET -> FET to drive torch
* TORCH_RGB -> digital to RGB LEDs

Torch also contains IRDIODE

**Note:**

* VCC on SK6805-EC15 will need to be switchable (1mA static power draw)
* SK6805-EC15 want 3.7-5.5v input - either power them from VBAT, or if the 5v power rail can supply 30mA we could use that

## IRDIODE

IR diode should be connected to 3.3v rail, with FET to pull it down to 0v

* IRDIODE_READ -> connected to LCDMCU analog input - allows us to read IR signals/ambient light level (also self-test IR)
* IRDIODE_FET -> connected to LCDMCU for IR transmission

## MICROPHONE

Use a PDM MEMS microphone. 

* MICROPHONE_CLK
* MICROPHONE_DATA

## QSPIFLASH: External SPI flash chip

512mbit QSPI flash chip - mx25lm51245g or similar? Low power usage is quite important.

Connected to nRF54L15 QSPI port

## LCDMCU: LCD interface (Puya PY32F040K2BU6TR?)

Will send data to LCD, but also handles IO from buttons (allowing us to recover the nRF54)

We could do with a device with 64kB RAM (or at least 32kB) as then we can stream the ~44kB of pixel data at high speed and leave this MCU to update as fast as possible. PY32F040K2BU6TR only has 16 and is quite big + power hungry, so a different chip might be better?

* 1 VCC / VDDS Main Digital Power Supply (1.7V to 5.5V) -> VCC
* 8 VSS / VSSA Device Ground -> GND
* 4 VREF+ Analog reference voltage for the ADC -> VCC
* 2 PA0 (ADC_CH0, TIM2_CH1, USART2_CTS) -> LCD_R1
* 3 PA1 (ADC_CH1, TIM2_CH2, USART2_RTS) -> LCD_R2
* 5 PA2 (ADC_CH2, TIM2_CH3, USART2_TX) -> LCD_G1
* 6 PA3 (ADC_CH3, TIM2_CH4, USART2_RX) -> LCD_G2
* 7 PA4 (ADC_CH4, SPI1_NSS, USART1_CK) -> LCD_B1
* 9 PA5 (ADC_CH5, SPI1_SCK, TIM2_CH1) -> LCD_B2
* 10 PA6 (ADC_CH6, SPI1_MISO, TIM3_CH1, TIM16_CH1) -> LCD_HCK
* 11 PA7 (ADC_CH7, SPI1_MOSI, TIM3_CH2, TIM17_CH1, TIM1_CH1N) -> LCD_VCK
* 12 PB0 (ADC_CH8, TIM3_CH3, TIM1_CH2N) -> BUTTONS_COMMON (analog via resistor-divider network - see BUTTONS)
* 13 PB1 (ADC_CH9, TIM3_CH4, TIM1_CH3N) -> IRDIODE_READ (analog)
* 14 PB2 (LPTIM_OUT, RTC_OUT) -> LCD_FRP + LCD_VCOM (external inverter converts FRP to XFRP - RTC_OUT allows 120Hz toggle while sleeping) (must use this pin)
* 15 PA8 (TIM1_CH1, MCO), USART1_CK -> LCD_HST
* 16 PA9 (TIM1_CH2, USART1_TX, I2C1_SCL) -> LCD_VST
* 17 PA10 (TIM1_CH3, USART1_RX, I2C1_SDA) -> LCD_ENB
* 18 PA11 (TIM1_CH4, USART1_CTS) -> LCD_XRST
* 19 PA12 (TIM1_ETR, USART1_RTS) -> nRF54L15 NRST
* 20 PA13 SWDIO (Serial Wire Debug Data) -> TEST_POINT, nRF54L15 LCDMCU_SWDIO
* 21 PA14 SWCLK (Serial Wire Debug Clock) -> TEST_POINT, nRF54L15 LCDMCU_SWDCLK
* 22 PA15 (SPI1_NSS, TIM2_CH1, USART2_RX) -> nRF54L15 LCDMCU_SPI_NSS
* 23 PB3 (SPI1_SCK, TIM2_CH2, USART2_TX) -> nRF54L15 LCDMCU_SPI_SCK
* 24 PB4 (SPI1_MISO, TIM3_CH1) -> nRF54L15 LCDMCU_SPI_MISO
* 25 PB5 (SPI1_MOSI, TIM3_CH2) -> nRF54L15 LCDMCU_SPI_MOSI
* 26 PB6 (USART1_TX, I2C1_SCL, TIM16_CH1N) -> LCD_BACKLIGHT
* 27 PB7 (USART1_RX, I2C1_SDA, TIM17_CH1N) -> IRDIODE_FET
* 28 PB8 (I2C1_SCL, TIM16_CH1) -> TORCH_FET
* 29 PF0 / OSC_IN -> nRF54L15 SWDIO
* 30 PF1 / OSC_OUT -> nRF54L15 SWDCLK
* 32 NRST (optionally PF2) System Reset (Active Low) -> resistor to VCC, ASWITCH_IN1IN2
* 31 PF3 / BOOT0 Boot Mode Selection Pin -> resistor to GND, nRF54L15 LCDMCU_IRQ

**Notes:** 

* It's important we keep LCD R/G/B wires in-order on one port, so the code can set them quickly.
* PY32F040K2BU6TR runs at 24MHz with HSI so SPI slave is 6MHz max (LCD only accepts 4.5mbps so this is *just* ok)
* We use RTC_OUT to allow us to toggle FRP/XFRP at 120hz while sleeping

## BUTTONS

To same some pins, the 4 buttons are connected via resistors:

BUTTONS_COMMON is connected to:

* LCDMCU PB0
* 40k pull-up resistor to 3.3v
* 0.1uF capacitor
* 1k pull-down resistor via BUTTON1
* 2.2k pull-down resistor via BUTTON2
* 4.7k pull-down resistor via BUTTON3
* 10k pull-down resistor via BUTTON4

## LCD

**external inverter needed** to invert LCD_FRP to LCD_XFRP

LCD needs a 5v supply - so this needs to be created from some low power circuit (could it be a capacitive voltage doubler off an MCU IO pin?)

* VST -> LCDMCU // Start signal for the vertical driver
* ENB -> LCDMCU // Write enable signal for the pixel memory
* HST -> LCDMCU // Start signal for the horizontal driver
* HCK -> LCDMCU // Shift clock for the horizontal driver
* XRST -> LCDMCU // Reset signal for the horizontal and vertical driver
* VCK -> LCDMCU // Shift clock for the vertical driver
* R1,R2,G1,G2,B1,B2 -> LCDMCU

* FRP : LCDMCU // Liquid crystal driving signal ("OFF pixel")
* XFRP : LCDMCU (via inverter) // Liquid crystal driving signal ("ON pixel")
* VCOM : LCDMCU // Liquid crystal driving signal

**Notes**: 

* LCD HCK max pulse width is 0.66us, so 1000000/1.32 = 758kHz for 6 bits = 4.5mbps data transfer needed
* FRP+VCOM can be joined together
* VCOM has to be the opposite of FRP

# ESP8684 (ESP32)

Use ESP8684 (all in one ESP32C2 WiFi). Design docs: https://documentation.espressif.com/esp-hardware-design-guidelines/en/latest/esp32c2/index.html

We may have to recompile the AT command firmware to make it work on UART0 so we can save some pins. Should also
change firmware to use ESP8684_BOOT as an CTS flow control pin so nRF54 can limit sent data.

Needs 26MHz XTAL

* CHIP_EN -> resistor to GND, nRF54L15 ESP8684_EN
* GPIO8 -> VCC
* GPIO9 (internal pullup) - nRF54L15 ESP8684_BOOT (1 = normal boot, 0 = bootloader)
* UART0 (Debug/bootloader)
  * GPIO19 (RX) -> TEST_POINT, nRF54L15 ESP8684_RX
  * GPIO20 (TX) -> TEST_POINT, nRF54L15 ESP8684_TX
* UART1 (AT command output)
  * GPIO6 (RX)
  * GPIO7 (TX)
  * GPIO5 (CTS) 
  * GPIO4 (RTS)
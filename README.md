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

<svg width="200" height="113" version="1.1" viewBox="0 0 200 113" xmlns="http://www.w3.org/2000/svg"><desc>voltage_mult.dxf - scale = 1.000000, origin = (0.000000, 0.000000), method = manual</desc><g transform="translate(-19 -856)"><g fill="none"><g stroke="#009800"><path d="m134 911h9.6"/><path d="m144 921v-9.6"/><path d="m144 911h38.4"/><path d="m144 950v9.6"/><path d="m115 911h-9.6"/><path d="m106 911h-9.6"/><path d="m106 902v9.6"/><path d="m67.2 911h-19.2"/><path d="m106 883v-19.2"/></g><g stroke="#900"><path d="m86.4 911h-1.92"/><path d="m76.8 911h1.92"/><path d="m67.2 911h9.6"/><path d="m96 911h-9.6"/><path d="m144 931v1.92"/><path d="m144 940v-1.92"/><path d="m144 950v-9.6"/><path d="m144 921v9.6"/><path d="m101 887 4.8 9.6"/><path d="m106 897 4.8-9.6"/><path d="m110 897h-4.8"/><path d="m110 887h-9.6"/><path d="m106 897h-4.8"/><path d="m106 883v9.6"/><path d="m106 902v-9.6"/><path d="m120 916 9.6-4.8"/><path d="m130 911-9.6-4.8"/><path d="m130 907v4.8"/><path d="m120 907v9.6"/><path d="m130 911v4.8"/><path d="m115 911h9.6"/><path d="m134 911h-9.6"/><path d="m79.2 916v-9.6"/><path d="m84 916v-9.6"/><path d="m139 933h9.6"/><path d="m139 938h9.6"/></g></g><g font-family="sans-serif" font-size="13.3px" letter-spacing="0px" word-spacing="0px"><text x="108.11478" y="873.11621" style="line-height:1.25" xml:space="preserve"><tspan x="108.11478" y="873.11621">3.3v</tspan></text><text x="164.76549" y="908.36499" style="line-height:1.25" xml:space="preserve"><tspan x="164.76549" y="908.36499">~5.2v</tspan></text><text x="20.340107" y="927.51678" style="line-height:1.25" xml:space="preserve"><tspan x="20.340107" y="927.51678">FRP@60Hz</tspan><tspan x="20.340107" y="944.18341"/></text><text x="145.0847" y="963.88153" style="line-height:1.25" xml:space="preserve"><tspan x="145.0847" y="963.88153">GND</tspan></text></g></g></svg>

## LIPOCHARGE: LiPo charger

If a PMIC doesn't have charging built in, can we have LiPo charger like BQ25120A (which has I2C communications for battery voltage) and connect it to I2CBUS? 

We don't have enough free IO to get battery voltage+charge status into the microcontroller.

## 4PIN: 4 Pin side connector:

Allows debug and charge, but pins are also usable as IO, with pin 4 able to supply switched battery power.

* 1: GND
* 2: to TS5A23157 COM1 (SWDCLK)
* 3: to TS5A23157 COM2 (SWDIO)
* 4: diode **to** LiPo battery charger, second diode **from** 4PINPFET_DRAIN

<svg width="179" height="68.4" version="1.1" viewBox="0 0 179 68.4" xmlns="http://www.w3.org/2000/svg"><desc>voltage_mult.dxf - scale = 1.000000, origin = (0.000000, 0.000000), method = manual</desc><g transform="translate(-22.8 -897)"><g fill="none"><g stroke="#009800"><path d="m96.9 929h38.4"/><path d="m148 947v9.6"/><path d="m122 911h-26.1"/><path d="m77.4 911h-29.4"/><path d="m73.3 930v-19.2"/><path d="m78.2 930h-4.54"/></g><path d="m135 929h9.6" stroke="#900"/></g><g transform="rotate(90 77.9 902)" fill="none" stroke="#900"><path d="m101 887 4.8 9.6"/><path d="m106 897 4.8-9.6"/><path d="m110 897h-4.8"/><path d="m110 887h-9.6"/><path d="m106 897h-4.8"/><path d="m106 883v9.6"/><path d="m106 902v-9.6"/></g><g transform="translate(-38 -.0345)" fill="none" stroke="#900"><path d="m120 916 9.6-4.8"/><path d="m130 911-9.6-4.8"/><path d="m130 907v4.8"/><path d="m120 907v9.6"/><path d="m130 911v4.8"/><path d="m115 911h9.6"/><path d="m134 911h-9.6"/></g><path d="m148 947v-6.81" fill="none" stroke="#900"/><path d="m144 939v-9.6" fill="none" stroke="#900"/><g font-family="sans-serif" letter-spacing="0px" word-spacing="0px"><text x="123.695" y="915.55286" font-size="13.3px" style="line-height:1.25" xml:space="preserve"><tspan x="123.695" y="915.55286">LiPo charger</tspan></text><text x="22.485815" y="906.4549" font-size="13.3px" style="line-height:1.25" xml:space="preserve"><tspan x="22.485815" y="906.4549">4PIN</tspan><tspan x="22.485815" y="923.12152"/></text><text x="115.92451" y="963.9516" font-size="8.4px" style="line-height:1.25" xml:space="preserve"><tspan x="115.92451" y="963.9516">4PINPFET_GATE</tspan></text></g><g fill="none" stroke="#900"><path d="m152 939v-9.6"/><path d="m151 929h9.6"/><path d="m142 939h11.9"/><path d="m142 941h11.9"/></g><text x="161.15788" y="934.22668" font-family="sans-serif" font-size="13.3px" letter-spacing="0px" word-spacing="0px" style="line-height:1.25" xml:space="preserve"><tspan x="161.15788" y="934.22668">LiPo+</tspan></text></g></svg>

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
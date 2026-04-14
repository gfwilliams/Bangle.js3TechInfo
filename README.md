# Bangle.js3 Tech Info

Technical info on Bangle.js 3 (subject to change!)

### Notes:

I've named certain elements in caps, eg $PIN/LIPOCHARGE/etc
TEST_POINT means this should go to a test point on the PCB

## Power Supply

* ESP8684 states it needs 3.3v at no less than 500mA, so we will need a reasonably powerful supply
* nRF54L15 can run from 1.8-3.3v and has a switching regulator so it shouldn't mind, but it might make sense to use 1.8v so we can power other peripherals off this and save some power.
* LCD also needs 5v (but at very low power)

## 4PIN: 4 Pin side connector:

Allows debug and charge, but pins are also usable as IO, with pin 4 able to supply battery power.

1: GND
2: to TS5A23157 COM1 (SWDCLK)
3: to TS5A23157 COM2 (SWDIO)
4: diode to LIPOCHARGE, second diode from 4PINPFET DRAIN

## 4PINPFET: allow voltage output on 4PIN connector

500mA rated

SOURCE: LiPo+
DRAIN: diode to 4PIN pin 4
GATE: resistor to LiPo+, 0.7v diode to nRF54L15 FIXME (to avoid over-voltage on IO)

## LIPOCHARGE: LiPo charger

Standard LiPo charger

STATUS: connect to nRF54L15 FIXME
ENABLE: connect to nRF54L15 FIXME

## ASWITCH: Analog Switch for 4 Pin connector

Use TS5A23157/similar (https://www.ti.com/lit/ds/symlink/ts5a23157.pdf)
This will switch between SWD on the nRF54 and also configurable IO, allowing SWD to be turned off and protected.

IN1/IN2: joined, to LCDMCU FIXME
COM1: 4PIN pin 2
COM2: 4PIN pin 3
NC1: nRF54L15 SWDCLK
NC2: nRF54L15 SWDIO
N01: nRF54L15 FIXME
N02: nRF54L15 FIXME


## nRF54L15: Assuming we're using the WLCSP300 BGA

* XC1/XC2: 32MHz Crystal
* P1.12/AIN5
* DECA, DECD, VSS, DCC, DECRF, VDD, VSS_PA: See https://www.mouser.com/datasheet/2/297/nRF54L15_nRF54L10_nRF54L05_Datasheet_v0_8-3568773.pdf page 808 for reference schematic
* P1.09
* P1.13/AIN6
* P1.14/AIN7
* P1.15
* P1.10
* P1.11/AIN4
* P1.00/P1.01: 32.768kHz crystal
* P1.02/P1.03: NFC
* ANT: antenna
* nRESET: to LCDMCU IO
* P0.04
* P2.08: SPIM SD0
* P1.04/AIN0
* P1.06/AIN2
* P0.03: // RTC PWM out - could use for LCD FRP/XFRP?
* P0.2:
* SWDCLK/SWDIO: Connect to TEST_POINT and ASWITCH
* P2.07: 
* P1.05/AIN1
* P1.07/AIN3
* P0.01
* P2.09
* P2.06
* P1.08
* P0.00
* P2.10

* P2.00: QSPIFLASH D3
* P2.01: QSPIFLASH SCK
* P2.02: QSPIFLASH D0
* P2.03: QSPIFLASH D2
* P2.04: QSPIFLASH D1
* P2.05: QSPIFLASH CS

FIXME:

LCDMCU SPI
LCDMCU SWD
Touchscreen

## QSPIFLASH: External SPI flash chip

512mbit QSPI flash chip - mx25lm51245g or similar?

Connected to nRF54L15

## LCDMCU: LCD interface (Puya PY32F040K2BU6TR?)

Will send data to LCD, but also handles IO from buttons (allowing us to recover the nRF54)

We could do with a device with 64kB RAM (or at least 32kB) as then we can stream the ~44kB of pixel data at high speed and leave this MCU to update as fast as possible. PY32F040K2BU6TR only has 16 and is quite big + power hungry, so a different chip might be better?

* 1 VCC / VDDS Main Digital Power Supply (1.7V to 5.5V) -> VCC
* 8 VSS / VSSA Device Ground -> GND
* 32 NRST System Reset (Active Low). Can also be used as a GPIO.
* 4 VREF+ Analog reference voltage for the ADC -> VCC
* 2 PA0 ADC_CH0, TIM2_CH1, USART2_CTS -> LCD R1
* 3 PA1 ADC_CH1, TIM2_CH2, USART2_RTS -> LCD R2
* 5 PA2 ADC_CH2, TIM2_CH3, USART2_TX -> LCD G1
* 6 PA3 ADC_CH3, TIM2_CH4, USART2_RX -> LCD G2
* 7 PA4 ADC_CH4, SPI1_NSS, USART1_CK -> LCD B1
* 9 PA5 ADC_CH5, SPI1_SCK, TIM2_CH1 -> LCD B2
* 10 PA6 ADC_CH6, SPI1_MISO, TIM3_CH1, TIM16_CH1 -> LCD HCK
* 11 PA7 ADC_CH7, SPI1_MOSI, TIM3_CH2, TIM17_CH1, TIM1_CH1N -> LCD VCK
* 12 PB0 ADC_CH8, TIM3_CH3, TIM1_CH2N
* 13 PB1 ADC_CH9, TIM3_CH4, TIM1_CH3N
* 14 PB2 LPTIM_OUT, RTC_OUT
* 15 PA8 TIM1_CH1, MCO (Clock Output), USART1_CK -> LCD HST
* 16 PA9 TIM1_CH2, USART1_TX, I2C1_SCL -> LCD VST
* 17 PA10 TIM1_CH3, USART1_RX, I2C1_SDA -> LCD ENB
* 18 PA11 TIM1_CH4, USART1_CTS, CAN_RX (if supported)-> LCD XRST
* 19 PA12 TIM1_ETR, USART1_RTS, CAN_TX (if supported)
* 20 PA13 SWDIO (Serial Wire Debug Data) -> TEST_POINT, nRF54L15 FIXME
* 21 PA14 SWCLK (Serial Wire Debug Clock) -> TEST_POINT, nRF54L15 FIXME
* 22 PA15 (SPI1_NSS, TIM2_CH1, USART2_RX) -> nRF54L15 FIXME
* 23 PB3 (SPI1_SCK, TIM2_CH2, USART2_TX) -> nRF54L15 FIXME
* 24 PB4 (SPI1_MISO, TIM3_CH1) -> nRF54L15 FIXME
* 25 PB5 (SPI1_MOSI, TIM3_CH2) -> nRF54L15 FIXME
* 26 PB6 USART1_TX, I2C1_SCL, TIM16_CH1N
* 27 PB7 USART1_RX, I2C1_SDA, TIM17_CH1N
* 28 PB8 I2C1_SCL, TIM16_CH1
* 29 PF0 / OSC_IN High-Speed External Crystal Input (HSE)
* 30 PF1 / OSC_OUT High-Speed External Crystal Output (HSE)
* 31 PF3 / BOOT0 Boot Mode Selection Pin.

FIXME:

nRF54L15 SWD (and reset??)
nRF54L15 IRQ pin (for button presses)
4x Buttons

## LCD

FIXME: do we control FRP/XFRP from LCDMCU? it'd have to be on all the time then

LCD needs a 5v supply - so this needs to be created from some low power circuit (could it be a capacitive voltage doubler off an MCU IO pin?)

* VST : LCDMCU // Start signal for the vertical driver
* ENB : LCDMCU // Write enable signal for the pixel memory
* HST : LCDMCU // Start signal for the horizontal driver
* HCK : LCDMCU // Shift clock for the horizontal driver
* XRST : LCDMCU // Reset signal for the horizontal and vertical driver
* VCK : LCDMCU // Shift clock for the vertical driver
* R1,R2,G1,G2,B1,B2 : LCDMCU

* FRP : // Liquid crystal driving signal ("OFF pixel")
* XFRP :  // Liquid crystal driving signal ("ON pixel")
* VCOM : 

# ESP32

Use ESP8684 (all in one ESP32C2). Design docs: https://documentation.espressif.com/esp-hardware-design-guidelines/en/latest/esp32c2/index.html

We may have to recompile the AT command firmware to make it work on UART0 so we can save some pins.

Needs 26MHz XTAL

* CHIP_EN -> resistor to GND, nRF54L15 FIXME
* GPIO8 -> VCC
* GPIO9 (internal pullup) - 1 = normal boot, 0 = bootloader
UART0 (Debug/bootloader)
* GPIO19 (RX) -> TEST_POINT
* GPIO20 (TX) -> TEST_POINT
UART1 (AT command output)
* GPIO6 (RX)
* GPIO7 (TX)
* GPIO5 (CTS)
* GPIO4 (RTS)
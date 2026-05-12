# STM32H733VGT6 Flight Controller

## Overview

This repository contains the hardware documentation and pinout routing for a custom flight controller based on the STM32H733VGT6 microcontroller. Designed for high-performance flight dynamics, this board leverages the MCU's 550 MHz clock speed, math coprocessor, and extensive I/O capabilities.

---

## Current Pinout & Peripheral Mapping

Below is the structured peripheral documentation based on the current schematic routing.

### 1. Serial Peripheral Interface (SPI)

*Primarily used for high-speed sensors (IMU), OSD chips, Blackbox Flash memory, and SD Cards.*

| Peripheral     | Pins              | Typical Signal Mapping                           | Suggested Use Case                                 |
| :------------- | :---------------- | :----------------------------------------------- | :------------------------------------------------- |
| **SPI1** | PA4 - PA7         | PA4 (NSS), PA5 (SCK), PA6 (MISO), PA7 (MOSI)     | **Primary IMU** (e.g., MPU6000, BMI270)      |
| **SPI2** | PB12 - PB15       | PB12 (NSS), PB13 (SCK), PB14 (MISO), PB15 (MOSI) | **Secondary IMU** or **OSD** (AT7456E) |
| **SPI3** | PA15, PC10 - PC12 | PA15 (NSS), PC10 (SCK), PC11 (MISO), PC12 (MOSI) | **Blackbox Flash** (e.g., W25Q128)           |
| **SPI4** | PE11 - PE14       | PE11 (NSS), PE12 (SCK), PE13 (MISO), PE14 (MOSI) | **SD Card** or **Third IMU**           |

### 2. Inter-Integrated Circuit (I2C)

*Used for slower, address-based peripherals like Barometers and Compasses.*

| Peripheral     | Pins        | Signal Mapping         | Suggested Use Case                             |
| :------------- | :---------- | :--------------------- | :--------------------------------------------- |
| **I2C1** | PB6 - PB7   | PB6 (SCL), PB7 (SDA)   | **Barometer** (e.g., BMP280, DPS310)     |
| **I2C2** | PB10 - PB11 | PB10 (SCL), PB11 (SDA) | **Magnetometer/Compass** (e.g., QMC5883) |
| **I2C4** | PD12 - PD13 | PD12 (SCL), PD13 (SDA) | Breakout / I2C Expansion Pad                   |

### 3. Universal Asynchronous Receiver-Transmitter (UART)

*Used for external communications: Radio Receivers, GPS, Digital Video Transmitters, and Telemetry.*

| Peripheral       | Pins        | Signal Mapping       | Suggested Use Case                                |
| :--------------- | :---------- | :------------------- | :------------------------------------------------ |
| **UART1**  | PA9 - PA10  | PA9 (TX), PA10 (RX)  | **Digital VTX** (DJI O3, Walksnail, HDZero) |
| **UART2**  | PA2 - PA3   | PA2 (TX), PA3 (RX)   | **Radio Receiver** (ExpressLRS / Crossfire) |
| **UART3**  | PD8 - PD9   | PD8 (TX), PD9 (RX)   | **GPS** (RX to GPS TX, TX to GPS RX)        |
| **UART4**  | PA0 - PA1   | PA0 (TX), PA1 (RX)   | **ESC Telemetry** (Usually RX only needed)  |
| **UART7**  | PE7 - PE8   | PE8 (TX), PE7 (RX)   | Free Serial Port                                  |
| **UART8**  | PE0 - PE1   | PE1 (TX), PE0 (RX)   | Free Serial Port                                  |
| **UART9**  | PD14 - PD15 | PD15 (TX), PD14 (RX) | Free Serial Port                                  |
| **UART10** | PE2 - PE3   | PE3 (TX), PE2 (RX)   | Free Serial Port                                  |

### 4. Controller Area Network (CAN)

*Used for robust, high-speed communication with DroneCAN peripherals (smart ESCs, advanced GPS modules).*

| Peripheral       | Pins      | Signal Mapping     | Suggested Use Case                |
| :--------------- | :-------- | :----------------- | :-------------------------------- |
| **FDCAN1** | PB8 - PB9 | PB9 (TX), PB8 (RX) | **DroneCAN Peripheral Bus** |

### 5. Timers & Motor Outputs

*Used for sending DSHOT or PWM signals to the Electronic Speed Controllers (ESCs).*

| Peripheral     | Pins      | Signal Mapping                             | Suggested Use Case                      |
| :------------- | :-------- | :----------------------------------------- | :-------------------------------------- |
| **TIM8** | PC6 - PC9 | PC6 (CH1), PC7 (CH2), PC8 (CH3), PC9 (CH4) | **Motor 1-4 Outputs** (DSHOT/PWM) |

---

## Hardware Routing To-Do List

The following critical subsystems still need to be established to make the flight controller fully functional.

- [ ] **USB Communication:** Route `PA11` (USB_D-) and `PA12` (USB_D+) and design the USB-C connector circuit with 5.1k CC pull-down resistors.
- [ ] **Debug and Flashing (SWD):** Route `PA13` (SWDIO) and `PA14` (SWCLK) to dedicated debug pads.
- [ ] **Analog Inputs (ADC):** Select ADC-capable pins (like `PC1`, `PC2`, or `PC3`) and route them to the Voltage Divider circuit (VBAT) and Current Sensor amplifier output.
- [ ] **System Basics:**
  - [ ] **Buzzer:** Route a GPIO pin to a transistor circuit for a 5V active buzzer.
  - [ ] **Status LEDs:** Route 1 or 2 free GPIO pins to drive status LEDs.
  - [ ] **Addressable LED Strip:** Route a DMA-capable timer pin for WS2812B RGB LEDs.

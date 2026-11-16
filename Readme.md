# Industrial Sensor Telemetry & Monitoring System 🏭📡

![STM32](https://img.shields.io/badge/STM32-NUCLEO--F446RE-blue?style=flat-square&logo=stmicroelectronics)
![Language](https://img.shields.io/badge/Language-C-00599C?style=flat-square&logo=c)
![Protocol](https://img.shields.io/badge/Protocol-UART-orange?style=flat-square)
![ADC](https://img.shields.io/badge/ADC-12--bit-green?style=flat-square)

## Overview
This project simulates an **Industrial Data Acquisition (DAQ)** node. It is designed to read continuous analog sensor data (such as voltage, temperature, or pressure) using an STM32 microcontroller and transmit it to a computer terminal (SCADA/HMI equivalent) via the UART protocol in real-time. 

To meet industrial standards, the architecture strictly avoids blocking functions like `HAL_Delay()`. Instead, it utilizes **Hardware Interrupts (IT)** and **Timer-based execution** to ensure the main CPU remains completely free for critical motor control operations.

## Key Features & Architecture
* **Non-Blocking Data Acquisition:** ADC conversions are triggered every 500ms using `HAL_GetTick()`, and the results are processed purely inside the `HAL_ADC_ConvCpltCallback` interrupt function.
* **12-bit Precision Scaling:** Converts raw ADC values (0-4095) into actual physical floating-point voltages (0.00V - 3.30V).
* **UART Telemetry:** Transmits formatted data packets to the host PC at a high baud rate (115200 bps) using `HAL_UART_Transmit`.
* **Compiler Optimization Safety:** All shared variables between the Main Loop and ISR are correctly defined as `volatile` to prevent optimization failures.

## Hardware Setup
* **Board:** STM32 NUCLEO-F446RE
* **Analog Input:** Pin `PA0` (ADC1_IN0) - Connected to an analog sensor (or physically toggled between 0V and 3.3V for simulation).
* **Communication:** `PA2` (TX) and `PA3` (RX) connected to the ST-Link Virtual COM Port.

## System Output (Live Telemetry)
*Below is the real-time telemetry data captured via PuTTY Serial Terminal (115200 Baud):*

![Telemetry Output](Docs)

## How to Run
1. Flash the code to the NUCLEO-F446RE board.
2. Connect the board via USB to the PC.
3. Open a Serial Terminal (PuTTY, Tera Term, or Arduino Serial Monitor).
4. Select the ST-Link Virtual COM Port.
5. Set the Baud Rate to `115200` (8 Data bits, No Parity, 1 Stop bit).
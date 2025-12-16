---
layout: post
title: Firmware work at US Department of Agriculture
description:  The US Department of Agriculture aims to record insect behavior by passing a current through an insect and digitizing the signal with an MCU. The Nordic nRF5340 MCU’s default 100 Hz ADC sampling causes aliasing, so we implemented a DMA-driven pipeline to sample at 10 kHz and stream the data over Bluetooth Low Energy without packet loss. **We solve the long-standing problem that has been bothering the ecologists for over a year.**
skills: 
  - Microcontroller Programming (C/C++)
  - Direct Memory Access
  - Analog-to-Digital Converter
  - Bluetooth Low Energy
  - FPGA Programming (SystemVerilog)
  - ZephyrRTOS
  - Oscilloscope / JTAG

main-image: /nordic.png
---

---

## MCU Design
{% include image-gallery.html images="mcu_overview.png" height="300" %} 
High-frequency ADC sampling (10 kHz) is implemented using a fully hardware-driven pipeline to avoid RTOS scheduling jitter. Rather than triggering ADC reads from RTOS threads, sampling is driven by a hardware timer and Direct Memory Access (DMA), with minimal CPU involvement.

A hardware timer (TIMER2) is configured to generate a compare event every 100 µs, corresponding to a 10 kHz sampling rate. This event is routed via the Distributed Programmable Peripheral Interconnect (DPPI) directly to the SAADC SAMPLE task. Each timer event triggers exactly one ADC conversion, and the resulting 12-bit sample is written directly into a DMA buffer.

{% include image-gallery.html images="mcu_flowchart.png" height="300" %} 
The SAADC operates in a double-buffered mode. After 80 samples are collected, the SAADC generates an END event. This event triggers two parallel actions:

via DPPI, the SAADC is immediately restarted to fill the next buffer, ensuring continuous sampling with no gaps;

an interrupt is raised to notify the CPU that a buffer is complete.

In the SAADC interrupt handler, a timestamp is captured and the completed buffer is copied into a staging area. A Bluetooth Low Energy (BLE) work item is then scheduled to run in a non-interrupt context. Hardware sampling continues uninterrupted using the alternate DMA buffer while BLE transmission is handled asynchronously.

The BLE work handler packages each buffer into a notification payload consisting of a 4-byte timestamp followed by 80 serialized 16-bit ADC samples. At a 10 kHz sampling rate, this results in 125 packets transmitted per second. Data is sent using a GATT notify characteristic. If the BLE stack is temporarily congested, the handler retries transmission up to a fixed limit; if retries are exhausted, the buffer is dropped to prevent back-pressure from blocking acquisition.

This design forms a closed hardware loop:

TIMER2 → SAADC SAMPLE → DMA buffer → SAADC END → SAADC START

As a result, sampling timing is fully decoupled from CPU load and BLE throughput, ensuring deterministic acquisition even under communication congestion. A image of the 10kHz sampled signal that is transmitted via Bluetooth is shown below. 

{% include image-gallery.html images="sine_wave.png" height="300" %} 


## FPGA Design

The role of the UPduino v3.1 FPGA in this project is to generate a signal that simulates the output of a current passing through an insect. By using a MCP4822 12-bit DAC, it is possible to send data values to the DAC, and get a clean analog signal. The DAC uses an SPI communication protocol, which allows for faster data transmission than other communication protocols such as UART.

The FPGA was programmed using an FSM that determines whether to send a new data value, or whether to keep the DAC IDLE. With the on-board 48 MHz oscillator, it is possible to divide to a 3 MHz clock which was used for the SPI clock (SCK) as well as divide to a much lower frequency clock to determine a new data value from a big look-up table.


{% include image-gallery.html images="fpga_diagram.png" height="300" %} 


The output of the DAC goes to a reconstruction filter that smooths out the high frequencies, cleaning the signal further. From there, the output signal goes straight into the ADC of the Nordic MCU.

{% include image-gallery.html images="fpga_design.png" height="300" %} 

Below is the signal that is generated from the FPGA as can be seen from the oscilloscope of a sine wave with frequency of 1 kHz.

{% include image-gallery.html images="sine.png" height="300" %} 


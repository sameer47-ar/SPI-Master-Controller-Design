# SPI-Master-Controller-Design

A synthesizable SPI Master Controller implemented in Verilog for the Zynq Programmable Logic.  
Designed and verified on the **ZedBoard** using Xilinx Vivado and Integrated Logic Analyzer (ILA).

## Features

- Write-only SPI Master (no MISO)
- Gated 10 MHz SPI clock generated from 100 MHz system clock
- 3-state FSM: `IDLE` → `SEND` → `DONE`
- Shift-register based **MSB-first** serial transmission
- Load / Done handshaking protocol
- Fully synthesizable and timing-compliant
- In-system debugging support using Vivado ILA

## Tools & Hardware

| Item              | Details                          |
|-------------------|----------------------------------|
| Language          | Verilog                          |
| Tool              | Xilinx Vivado                    |
| Debug             | Vivado Integrated Logic Analyzer (ILA) |
| Board             | ZedBoard (XC7Z020)               |
| System Clock      | 100 MHz                          |
| SPI Clock         | 10 MHz (gated)                   |

## How it Works

1. When `load_data` is asserted, the 8-bit parallel data is latched into an internal shift register.
2. The controller enters `SEND` state and generates a gated 10 MHz SPI clock.
3. Data is shifted out **MSB first** on the falling edge of the SPI clock.
4. After 8 bits are transmitted, the controller moves to `DONE` state and asserts `send_done`.
5. Once the external logic de-asserts `load_data`, the controller returns to `IDLE`.

## Interface

| Signal       | Direction | Description                          |
|--------------|-----------|--------------------------------------|
| `clk`        | Input     | 100 MHz system clock                 |
| `reset`      | Input     | Active-high reset                    |
| `data_in[7:0]` | Input   | Parallel data to be transmitted      |
| `load_data`  | Input     | Assert to start transmission         |
| `send_done`  | Output    | Asserted when transmission is complete |
| `spi_clock`  | Output    | Gated SPI clock (10 MHz)             |
| `spi_data`   | Output    | Serial data (MOSI)                   |

## File Structure

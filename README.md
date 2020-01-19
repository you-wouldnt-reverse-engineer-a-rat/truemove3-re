# truemove3-re

reverse engineering of the truemove3 sensor used by rival gaming mice developed by pixart

it has now become clear that the truemove3 sensor is heavily based on the PMW3360, they appear to have the same register map.
it is unclear what additional features pixart included in this version of the chip, and what behaves differently from the PMW3360

### Pin Configuration

The pinout of the truemove3 is afawk identical to the PMW3360

Pin no. | Funcion | Symbol | Type | Description
:---: | :---: | :---: | :---: | :---:
1 | NA | NC | NC | (Float)
2 | NA | NC | NC | (Float)
3 | Supply | VDDPIX | Power | LDO output for selective analog circuit
4 | Supply | VDD | Power | Input power supply
5 | Supply | VDDIO | Power | I/O Reference voltage
6 | NA | NC | NC | (Float)
7 | Reset | NRESET | Input | Chip reset(active low)
8 | Ground | GND | GND | Ground
9 | Motion | MOTION | Output | Motion Detect
10 | 4w. SPI | SCLK | Input | Serial data clock
11 | 4w. SPI | MOSI | Input | Serial data input
12 | 4w. SPI | MISO | Output | Serial data output
13 | 4w. SPI | NCS | Input | Chip select(active low)
14 | NA | NC | NC | (Float)
15 | LED | LED_P | Input | LED Anode
16 | NA | NC | NC | (Float)

### Interface

The sensor uses SPI as it's communication interface.
From the Rival-310 we can deduce that:
- 1.2MHz (at least)
- MODE 3 (cpol = 1; cpha = 1)
- Chip-Select active-low

### Registers

Address | Register | Description | Access (R/W/RW) | Default Value
:---: | :---: | :---: | :---: | :---:
0x00 | Product_ID | Unique identification, value does not change, used to verify if serial comunication link is funcional | R | 0x42
0x02 | Motion |  | RW | 0x20
0x03 | Delta_X_L |  | R | 0x00
0x04 | Delta_X_H |  | R | 0x00
0x05 | Delta_Y_L |  | R | 0x00
0x06 | Delta_Y_H |  | R | 0x00
0x10 | Config2 |  | RW | 0x20
0x13 | SROM_Enable |  | W | N/A
0x24 | Observation |  | RW | 0x00 
0x25 | Data_out_Lower |  | R | 0x00
0x26 | Data_out_upper |  | R | 0x00
0x2A | SROM_ID |  | R | 0x00
0x3A | Power_Up_Reset |  | W | N/A
0x50 | Motion_Burst |  | RW | 0x00 
0x62 | SROM_load_Burst |  | W | N/A

### startup sequence (obtained from a Rival-310)
```
Write 0x5A to 0x3A (software reset)

60ms delay

Read 0x02
Read 0x03
Read 0x04
Read 0x05
Read 0x06
Read 0x00
Read 0x10
Write 0x00 to 0x10 (Clear REST mode enable bit)
Write 0x1D to 0x13 (initializing SROM download)

15ms delay

Write 0x18 to 0x13 (start SROM download)

Write 0x62 (SROM Load burst)
Write SROM Image

Read 0x2A (Read SROM ID)
Write 0x15 to 0x13 (SROM CRC check initialization)

15ms delay

Read 0x26 (CRC upper)
Read 0x25 (CRC lower)
Read 0x24 (Check SROM running bit?)
Write 0x00 to 0x10 (Clear REST mode enable bit)
Write 0x01 to 0x50 (initialize motion burst)

500ms delay

Write 0x01 to 0x50 (initialize motion burst)
Read 6B from 0x50 (read motion Burst)

Write 0x00 to 0x24 (Clear observation register)
Write 0x01 to 0x0F (Set CPI)

Write 0x01 to 0x50 (initialize motion burst)
Read 6B from 0x50 (Motion Burst)

normal operation...
(Reads 0x00 every 1000ms)
(Reads 'motion burst' on motion falling down)
```

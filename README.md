# truemove3-re

reverse engineering of the truemove3 sensor used by rival gaming mice developed by pixart

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
0x01 |  |  |  | 
0x02 | Motion |  | RW | 0x20
0x03 | Delta_X_L |  | R | 0x00
0x04 | Delta_X_H |  | R | 0x00
0x05 | Delta_Y_L |  | R | 0x00
0x06 | Delta_Y_H |  | R | 0x00
0x07 |  |  |  | 
0x08 |  |  |  | 
0x09 |  |  |  | 
0x0A |  |  |  | 
0x0B |  |  |  | 
0x0C |  |  |  | 
0x0D |  |  |  | 
0x0E |  |  |  | 
0x0F |  |  |  | 
0x10 |  |  |  | 
0x11 |  |  |  | 
0x12 |  |  |  | 
0x13 |  |  |  | 
0x14 |  |  |  | 
0x15 |  |  |  | 
0x16 |  |  |  | 
0x17 |  |  |  | 
0x18 |  |  |  | 
0x19 |  |  |  | 
0x1A |  |  |  | 
0x1B |  |  |  | 
0x1C |  |  |  | 
0x24 |  |  |  | 
0x25 |  |  |  | 
0x26 |  |  |  | 
0x2A |  |  |  | 
0x2B |  |  |  | 
0x2C |  |  |  | 
0x2F |  |  |  | 
0x3A | Power_Up_Reset |  | W | N/A
0x3B |  |  |  | 
0x3F |  |  |  | 
0x41 |  |  |  | 
0x42 |  |  |  | 
0x4A |  |  |  | 
0x50 |  |  |  | 
0x58 |  |  |  | 
0x5A |  |  |  | 
0x62 |  |  |  | 
0x63 |  |  |  | 
0x64 |  |  |  | 
0x65 |  |  |  | 

### Rival-310 startup sequence
```
Write 0x5A to 0x3A

60ms delay

Read 0x02
Read 0x03
Read 0x04
Read 0x05
Read 0x06
Read 0x00
Read 0x10
Write 0x00 to 0x10
Write 0x1D to 0x13

15ms delay

Write 0x18 to 0x13

Write 0x62
Write SROM Image

Read 0x2A
Write 0x15 to 0x13

15ms delay

Read 0x26
Read 0x25
Read 0x24
Write 0x00 to 0x10
Write 0x01 to 0x50

500ms delay

Write 0x01 to 0x50
Read 6B from 0x50 (Motion Burst)

Write 0x00 to 0x24
Write 0x01 to 0x0F

Write 0x01 to 0x50
Read 6B from 0x50 (Motion Burst)

normal operation...
(Reads 0x00 every 1000ms)
(Reads 'motion burst' on motion falling down)
```

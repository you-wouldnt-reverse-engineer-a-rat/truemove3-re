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
0x00 |  |  |  | 
0x01 |  |  |  | 
0x02 |  |  |  | 
0x03 |  |  |  | 
0x04 |  |  |  | 
0x05 |  |  |  | 
0x06 |  |  |  | 
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
0x3A |  |  |  | 
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

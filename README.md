# Serial Tool (RS232/UART)

A lightweight CLI utility written in C for serial communication on Linux systems. This tool allows users to configure serial port parameters, send data with custom terminators, and monitor incoming transmissions in real-time.

## Features
* **Comprehensive Configuration**: Support for custom Baud rates, data bits, parity, and stop bits.
* **Operating Modes**: 
    * `send`: Transmits a specific string to the device.
    * `receive`: Listens and prints incoming data to STDOUT.
* **Flow Control**: Supports both Hardware (RTS/CTS) and Software (XON/XOFF) flow control.
* **Line Terminators**: Option to append `LF`, `CR`, or `CRLF` to outgoing messages.
* **Raw Mode Handling**: Uses `cfmakeraw` to ensure transparent data transmission without terminal processing.
* **Signal Handling**: Clean exit and port closing using `SIGINT` (Ctrl+C).

---

## Installation
The tool requires a POSIX-compliant environment (Linux) and `gcc`.

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/yourusername/serial-tool.git](https://github.com/yourusername/serial-tool.git)
   cd serial-tool

    Compile the source:
    Bash

    gcc main.c -o serial-tool
   ```

Usage

Basic syntax: ./serial-tool -d [device_path] [options]

Arguments

Short	Long	Description
-d	--device	(Required) Path to the serial device (e.g., /dev/ttyUSB0)
-b	--baudrate	Communication speed (e.g., 9600, 115200)
-D	--databits	Data bits (5, 6, 7, 8)
-p	--parity	Parity bit: N (None), E (Even), O (Odd)
-s	--stopbits	Stop bits (1 or 2)
-m	--mode	Mode of operation: send or receive
-T	--transfer	The string data to be sent
-t	--terminator	Line ending for transfer: LF, CR, CRLF
-S	--softcontrol	Enable software flow control (XON/XOFF)
-H	--hardcontrol	Enable hardware flow control (RTS/CTS)

Examples

Listen to a device at 115200 baud:
```bash
./serial-tool -d /dev/ttyUSB0 -b 115200 -m receive
```

Send an AT command with CRLF terminator:
```bash
./serial-tool -d /dev/ttyUSB0 -m send -T "AT+GMM" -t CRLF
```

Custom 7E1 configuration with hardware flow control:
```bash
./serial-tool -d /dev/ttyS0 -b 19200 -D 7 -p E -s 1 -H -m receive
```

Technical Details

The application utilizes the termios.h library to interface with the Linux tty subsystem. It handles input validation through getopt_long and manages memory dynamically for device configurations.

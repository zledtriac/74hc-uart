# Transmitter
This is the transmitter. It's only consists of 6 ICs. Actually 5 because U6 the 74HC244 IC can be left out, but with the current configuration it can 
be operated manually for demonstrations.

The transmitter transmits the data with **1 start bit, 8bit data without parity and 1 stop bit** on the end, with 9600 baud rate.

## Operation
The transmitter has 3 pin headers for selecting the mode of operation (J3, J5, J6), before powering the transmitter up, **please check the jumper positions, 
because the transmitter can't operate in a circuit if the transmitter is in manual operation mode.**

The transmitter needs a 5V power supply to work, the power pins can be found on the Control header (J4) pin 1 - VCC, pin 2 - GND. After powering up the
transmitter, due to the random behaviour of the set-reset latch made with the U5D and U5C NAND gate (74HC132) and of the U3A and U3B (74HC74) D flip-flops, 
it is possible that the cicuit will transmit a random data on the output.

*If this behaviour can cause issues in the target device, it is recommended to build
an output enable circuit after the U3B pin 9 output, which can be controlled by a switch, delay circuit or from software if the transmitter is connected to a CPU.*

The Control header (J4) has two other pins for control, pin 5 - !WR, and pin 8 - Busy. The !WR is an active-low input and this input starts the data transmission.
The Busy is an output which will be held high while the transmission is going, or while the !WR input is held low.

## Manual operation

## Oscillator

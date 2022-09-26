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

To send data, first set the data on the Data header (J1) then send a low pulse on the !WR pin. This will trigger the Input trigger circuitry. In this stage the output 
of the U5D will turn high, but since the U3A D flip-flop is not set, the inverted output of the U3A D flip-flop is also high, this will set the output of the U4A NAND 
gate to low, this will enables the load pins of the U1 (74HC165) shift register and the U2 (74HC191) counter IC, so the data will be loaded into the U1 shift register 
and the U2 counter IC will be set to 0. There is an extra bit made by the U4B NAND gate and the U3B D flip-flop, in the loading process it will set to one, this extra 
bit is the START bit. Since the U3A D flip-flop's clock input is connected to the main clock signal, the U3A D flip-flop will set and the inverted output of the U3A D 
flip-flop will turn to low.
After this, every clock pulse will shift out the data from U1 (74HC165) shift register, and increment the U2 (74HC191) counter until U2 (74HC191) counter reaches 8.
When the U2 (74HC191) counter reaches 8, the counting will stop and if the !WR input is high, it will reset the Input trigger circuitry, and after this, one clock 
pulse is needed to reset the U3A D flip-flop. So when the next write pulse comes in, the whole process will repeat.

![Timing diagram](../../img/transmittertimig.svg)

## Manual operation
To use the circuit in a manual operation mode, set the jumper on the J3 pin header to bridge the 1-2 pins, and on the J5 pin header to bridge the 2-3 pins. This will 
enables the U6 (74HC244) output so the data set by the SW1 dip switch will be put on the DATA input, and it will connect the SW2 button to the !WR input.
After the data set on the SW1 dip switch, the data sending can be initiated by pressing the SW2 button.

**please note that if the circuit is in manual operation mode, the DATA input will be driven by the U6 (74HC244) output and the !WR input will connected to the SW2 
button, so it can't be used in other circuits!**

## Oscillator
The oscillator is made out of two schmitt-triggered NAND gate of the U4 (74HC132) IC. With the 10nF C7 capacitor and the 10kOhm R5 plus the 10kOhm RV1 trimmer 
potentiometer this oscillator can be adjusted between 6kHz and 12kHz. The oscillator's frequency is equal to the baud rate, so for the 9600 baud rate set the 
oscillator to 9.6kHz. For the tuning, a simple digital multimeter with frequency measuring capability is enough.

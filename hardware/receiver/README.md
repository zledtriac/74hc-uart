# Receiver
This is the receiver. It's only consists of 6 ICs. Actually 5 because U6 the 74HC244 IC can be left out, but with the current configuration it can 
be operated manually for demonstrations.

The receiver can receive a serial data transmission with **1 start bit, 8bit data without parity and 1 stop bit** on the end, with 9600 baude rate.

## Oscillator
The oscillator is made out of two schmitt-triggered NAND gate of the U1 (74HC132) IC. With the 560pF C1 capacitor and the 10kOhm R1 plus the 10kOhm RV1 trimmer 
potentiometer this oscillator can be adjusted between 107kHz and 214kHz. The oscillator's frequency is not equal to the baud rate, because the sampling clock is
generated with the U2 (74HC191) counter IC which is in this case act as a clock divider. The Q3 output of the U2 (74HC191) IC will give a square wave signal with 
16th of the input frequency. So the equation for the baudrate is:
```
baud_rate = input_freq / 16
```
So the frequency of the oscillator should be 153.6kHz for a 9600 baud rate.\
For tuning the oscillator, a simple digital multimeter with frequency measuring capability is enough.

# Receiver
This is the receiver. It's only consists of 6 ICs. Actually 5 because U6 the 74HC244 IC can be left out, but with the current configuration it can 
be operated manually for demonstrations.

The receiver can receive a serial data transmission with **1 start bit, 8bit data without parity and 1 stop bit** on the end, with 9600 baude rate.

## Operation
The receiver has 3 pin headers for selecting the mode of operation (J4, J5, J6), before powering the receiver up, **please check the jumper positions, 
because the receiver can't operate in a circuit if the receiver is in manual operation mode.**

The receiver needs a 5V power supply to work, the power pins can be found on the Control header (J2) pin 1 - VCC, pin 2 - GND. After powering up the
receiver, due to the random behaviour of the U5A and U5B (74HC74) D flip-flops, it is possible that the cicuit will load a random data in the output 
register. It is recommanded to wait one full transmition time after power up and read the output register in order to clear the data ready signaling 
U5B D flip-flop.

The Control header (J2) has two other pins for control, pin 4 - !RD, and pin 7 - Ready. The !RD is an active-low input and this input enables the 
output shift register's tri-state output drivers so the received data will be present on the output.
The Ready is an output which will turns on when the data is received and ready to read.

When the RX input goes low, it will start the colock divider and if the input is held low for more than half the time of one bit then U5B (74HC74) 
D flip-flop will be set, and the Input trigger will lock on the signal and keeps the clock divider running. When the clock divider is running it means 
that the Bit counter and the Output shift register will receive a clock signal, and the data reception sequence is in progress. When the Q3 output on 
the Bit counter goes high, it will stops the Clock divider and the circuit will wait until the RX input goes back to high, and when the RX input went 
high it will resets the Input trigger and load the received data into the U4 (74HC595) output register and sets the U5A (74HC74) D flip-flop so the 
Ready output will turn to high.

The Output data register can be read at the same time when the data reception sequence is running because the output data register only updates when 
the data reception sequence is finished. But if the data is not readed befor the second reception finishes it will overwrite the data in the output 
register so the previous data will be lost.

## Manual operation
To use the circuit in a manual operation mode, set the jumper on the J5 pin header to enable the output of the U6 (74HC244) IC so the data on the 
output can be seen on the LED bar. If you want to use the button as a read signal then bridge the 1-2 pins on the J6 Mode select pin header. Or if you 
want to see the received data immediatelly, bridge the 3-4 pins in the J6 Mode select pin header.

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

The J4 Clock select pin header can be used to fed external clock signal into the circuit. Just unplug the bridge from the 2-3 pins and connect the external clock 
signal the pin 3. The pin 4 can be used as a grounding point.
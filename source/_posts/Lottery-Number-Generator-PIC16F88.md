---
title: Lottery Number Generator - PIC16F88
date: 2019-06-13 02:07:00
tags:
    - Assembler
    - Electronics
    - Microcontroller
---

### Design Brief
The lottery is a part of weekly life in the UK, for many. Adults will often buy tickets hoping to win the jackpot, or simply for the fun of playing. As every number is just as likely as each other, unless you play with lucky pick numbers or personal numbers in mind, it can be hard to choose your numbers. Especially if you wish to use different numbers every week. This product shall generate numbers for the UK lottery, to pick the numbers for people.

### Specification
⦁	It must run off of a 5V supply.
⦁	It must have a button when pressed and released generates a number.
⦁	The random numbers generated must be between 1 and 59.
⦁	Must display in alphanumeric decimal.
⦁	It should display 2 digits.
⦁	It must output a desired number 98% of the time.
⦁	It must generate and display the numbers in under 5 milliseconds.

### Inputs & Outputs

Schmitt NOT Gate (40106B)
Pin	Connection	Pin	Connection
1		VDD	Connect High
2		13	
3		12	
4		11	
5	Connection from PTM switch	10	
6	Connection to PIC Input	9	
GND	Connect Low	8	

PIC16F88
Pin	Code	Connection	Pin	Code	Connection
1	A2		18	A1	
2	A3		17	A0	Input (from Schmitt Inverter)
3	A4		16	A7	
4	A5		15	A6	
5	GND	Connect Low	14	VDD	Connect High
6	B0	Units BCD A	13	B7	
7	B1	Units BCD A	12	B6	
8	B2	Units BCD A	11	B5	
9	B3	Units BCD B	10	B4	


BCD Decoder Driver (4511B)
Pin	Connection	Pin	Connection
1	BCD B	VDD	Connect High
2	BCD C	15	Segment F
3	Connect Low (lamp test)	14	Segment G
4	Connect High (ripple blanking)	13	Segment A
5	Connect Low (enable/store input)	12	Segment B
6	BCD D	11	Segment C
7	BCD A	10	Segment D
GND	Connect Low	9	Segment E

### Circuit Diagram
 
### Flow Chart 
 
### Program 
A copy of program version “0700t” & “0700u” attached to the post. When initially coding my project I ran into the problem of it not correctly compiling, upon inspecting my code I found all my instances of “MOVLW” to be “MOVW” this is not a recognised bit of code as it as is not specify a target byte to move to the working register. I remedied this problem by using Notepad++’s find and replace function to correct every instance of this problem.
  

Another change I made whilst writing my code was to the display section. When I first wrote it I used “BTFSC” and “BSF” commands to transfer the information from the clock register to PORTB. I then found out you can write to PORTB in the same way as any other directory. This allowed me to simplify a section of clumsy and repetitive code to a simpler and  much more elegant solution.  My new code used “MOVF” and “MOVWF” to simply transfer the data in the clock register to the output register of PORTB.
  

### Evidence of Building
I started the building of my circuit by placing all the parts that I thought would go over the trench, this allows me to set up an efficient placement for wiring, this is particularly evident in that the “units” BCD decoder driver is underneath the the dual seven segment display so that output C directly lines up with its pin on the display. 

I then proceeded to wire the chips, inputs and outputs together. All the wires connecting to positive and negative pins with red and black wire respectively. I used green wire for all the data on the breadboard. I started with the switches to the PIC, then the PIC to the BCD decoder drivers, and finally the BCD decoder drivers to the dual seven segment display.

Whilst testing the circuit I added a Schmitt NOT gate, this is why it is not connected particularly neatly. I used the Schmitt NOT gate to debounce the button input  to the PIC, this was done in an attempt to fix one of the bugs found. Although it was unsuccessful at fixing the bug I kept it in the circuit as I feel it will help the stability of my program.

###	Evidence of Testing
I first tested my program to make sure that the code was working as desired. I did this by using the integrated testing systems on the PIC loading board. By using the buttons and LEDs I could see if any incorrect BCD signals were being sent. I did not find any numbers on either chips that were outside the desired range.
  
To make sure that the lottery number generator worked I connected the breadboards to 5V and 0V. This showed that it was working as intended. After testing a few random numbers I concluded that there was an error in the wiring of the display. After looking back over the pinout information I found and corrected the error. This meant that all the device was working as intended.

### Photo of Working circuit
Finally I neatened the connections on the board, making everything sit flush on breadboard whilst making sure it all stays working. This makes circuit easier to follow and reduces any stray capacitance in the circuit.

⦁	Testing against specification
⦁	It must run off of a 5V supply.
⦁	Both the testing board and the final circuit ran off of a 5V supply. I specifically chose 4511s as my BCD decoder driver as they are designed to run off of 5V. The only change I made was adding a capacitor over the power rails as this smoothed any fluctuations in the supply voltage, this was needed as the PIC was especially finicky with its input voltages.
⦁	It must have a button when pressed and released generates a number.
⦁	The sole input for the device is a button, this needs to be pressed and released for a new number to be generated. This feature was especially coded into my project. Once the button had been pressed the PIC would be stuck in a looping of code until it was released again.
 
⦁	The random numbers generated must be between 1 and 59.
⦁	The numbers generated are between the values 0 and 59. This includes every number that it is supposed to have but has a very small chance of displaying zero. I have literally met the needs of the specification. However, I feel that it is implicit that it doesn’t want any numbers but these, so I am not particularly happy about this
⦁	Must display in alphanumeric decimal. 
⦁	I have used two BCD decoder drivers and a dual seven segment display to turn the output of the output to the PIC into a way that can easily be understood visually.
⦁	It should display 2 digits.
⦁	By using a dual seven segment display I have limited myself into only being able to display 2 digits, meeting the specification.
⦁	It must output a desired number 98% of the time.
⦁	To test to see if it was generating a desired number a sufficient amount of the time, I used my generator 200 times and recorded the frequency of every number generated. The number 0 was generated twice giving it a 1% chance of being generated. I see no significant evidence to say that 0 has a different probability of being generated. Giving it a 1.7% chance. This means it generates a desired number 98.3% of the time, meeting the specification. 
⦁	It must generate and display the numbers in under 5 milliseconds.
⦁	The clock speed of the PIC16F88 is 38 KHz. Between the button being released and the clock moving to the output register there is a maximum of 7 lines of code. This means the maximum delay is 0.74 ms, well below the specified 5 ms.

### Evaluation
During this project, I have learnt a great deal about assembly code and microcontrollers in general. This has been a particularly interesting project as I have had no prior experience with these systems, and would be interested to see what I can do with them in the future. If I were to do another project with them I would try using a larger number of Inputs as I feel that processing this information would play to the advantage of micro-controlled systems more than conventional circuits.

I have noticed that my generator also generates the number 00 as well as 01-59. It is my belief that this error is to do with the code. I feel that with an alternative approach to the code that this problem can be remedied. This is likely to need a complete overhaul of the code, but I would say that this is definitely worth it. This would improve the percentage of desired outputs drastically, this would more wholefully meet the specification.

To improve the design of this project, I would reduce the number of PICs to one. This would make a much more efficient design as it is much less wasteful. This would likely make removing 00 as an output easier as it would mean that the two PICs would not have to communicate. If this was made as a commercial project this would drastically reduce the overall cost of the circuit, as PICs are expensive. 

Overall I would say that this circuit was effective as it met every part of the specification. However, I would also call it inefficient as it was wasteful in its resources and the end result was underwhelming. Although I am pleased that I underwent this project I am not displeased with the result. I feel that I may have started this over ambitious and I have definitely learnt a lesson in setting realistic goals.
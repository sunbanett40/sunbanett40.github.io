---
title: SystemVerilog Microprocessor Part 1 - Building the Microprocessor
date: 2021-01-24 15:02:00
tags:
    - Assembler
    - State Machines
    - SystemVerilog HDL
    - Microcontroller
---

### Microprocessor Architecture
<p style="text-align:center;"><img src="/images/sv-microprocessor/IMG_20210123_152930.jpg" alt="CPU Architecture" style="width: 50%; height: 50%">

PC:
The program counter is a sequential logic module that records the position of the program through the ROM and outputs the position to the sysbus when the next instruction needs to be loaded.

IR:
The instruction register is a sequential logic module that reads the opcode from the sysbus and passes it to the sequencer.

Sequencer:
The sequencer is a state machine that controls the order of operation for the other modules. This is important to stop any bus contention occurring.

<p style="text-align:center;"><img src="/images/sv-microprocessor/IMG_20210123_151328.jpg" alt="Sequencer ASM Chart" style="width: 50%; height: 50%">

### Memory Mapped Input and Output
A buffer and a register were now instantiated so that these could be used as input and output to the Microcontroller
```verilog
//instantiate 1 register and 1 buffer here
buffer #(.WORD_W(WORD_W), .OP_W(OP_W)) b1 (.*);
sequencer #(.WORD_W(WORD_W), .OP_W(OP_W)) s1 (.*);
ir #(.WORD_W(WORD_W), .OP_W(OP_W)) i1 (.*);
pc #(.WORD_W(WORD_W), .OP_W(OP_W)) p1 (.*);
alu #(.WORD_W(WORD_W), .OP_W(OP_W)) a1 (.*);
ram #(.WORD_W(WORD_W), .OP_W(OP_W)) r1 (.*);
rom #(.WORD_W(WORD_W), .OP_W(OP_W)) r2 (.*);
register #(.WORD_W(WORD_W), .OP_W(OP_W)) r3 (.*);
```

The memory, going from address 0 to address 31, should now be connected to these applications.
<p style="text-align:center;"><img src="/images/sv-microprocessor/Memory.png" alt="Memory addresses" style="width: 50%; height: 50%">

However, if the CPU attempts to read from address 30, both the buffer and the RAM will be enabled. This is a problem as both RAM and the buffer will assert their values onto the sysbus as they are both being read. The two values on the same bus will interfere with each other giving a signal that is likely neither of them. This is bus contention.

Modifying this line so that mdr is not written to the bus when mar contains addresses 30 or 31.
```verilog
//The following line in the ram module copies the contents of mdr to sysbus:
assign sysbus = (MDR_bus & mar[WORD_W-OP_W-1]
 & ~(mar==30 | mar==31)) ? mdr : 'z ; 
```

Modifying the testbench, I made switches and displays as variables
```verilog
module testcpu;
parameter int WORD_W = 8, OP_W = 3;
logic clock, n_reset;
logic [WORD_W-1:0] switches;
logic [WORD_W-1:0] display;
wire [WORD_W-1:0] sysbus;
```

Simulating my testbench and microprocessor in Modelsim I got this output for my variables.
<img src="/images/sv-microprocessor/waveform1.png" alt="Modelsim output" style="width: 100%; height: 100%">

Both the display register and the switches are of unknown value as they are not defined at any point in the testbench or the program.

I modified the program in the rom module such that the result in the accumulator is STOREd to address 31 (display) as well as address 16.
```verilog
cpu2 #(.WORD_W(WORD_W), .OP_W(OP_W)) c1 (.*);
 0: mdr = {`STORE, 5'd16 }; //Store the contents of accumulator
 1: mdr = {`LOAD, 5'd16 }; //Load the contents of address into the accumulator
 2: mdr = {`ADD, 5'd6 }; //Add the contents of address to the accumulator
 3: mdr = {`STORE, 5'd16 }; //Store the contents of accumulator
 4: mdr = {`STORE, 5'd31 }; //Store the contents of accumulator
 5: mdr = {`BNE, 5'd7 }; //Branch if result of last arithmetic operation is not zero
 6: mdr = 2; //contents used by another instruction
 7: mdr = 1; //contents used by another instruction
 ``` 

My modified program would output the value to the display register. This clearly shows the
program counts up in twos.

<img src="/images/sv-microprocessor/waveform2.png" alt="Modelsim output" style="width: 100%; height: 100%">

I then made a second change to the program in the rom module such that the data for the ADD operation comes from address 30 – the switches. In addition I also modified “testcpu” to apply different values to the switches during the simulation. 
```verilog
 0: mdr = {`STORE, 5'd16 }; //Store the contents of accumulator
 1: mdr = {`LOAD, 5'd16 }; //Load the contents of address into the accumulator
 2: mdr = {`ADD, 5'd30 }; //Add the contents of address to the accumulator
 3: mdr = {`STORE, 5'd16 }; //Store the contents of accumulator
 4: mdr = {`STORE, 5'd31 }; //Store the contents of accumulator
 5: mdr = {`BNE, 5'd6 }; //Branch if result of last arithmetic operation is not zero
 6: mdr = 1; //contents used by another instruction
 ```

My new program would take the values from the switches and add it to the current value, outputting it to the display register.
<img src="/images/sv-microprocessor/waveform3.png" alt="Modelsim output" style="width: 100%; height: 100%">

### Extending the Microprocessor
Creating a new version of the CPU, cpu3, I added the bitwise xor and bitwise not (complement) functions to act on the accumulator. This meant defining the opcodes, adding new enable lines between the ALU and sequencer, adding the functionality to the ALU and Sequencer and writing a new program in ROM to use these commands.

ALU: 
```verilog
 if (load_ACC)
 if (ALU_ACC)
 begin
 if (ALU_add)
 acc <= acc + sysbus;
 else if (ALU_sub)
 acc <= acc - sysbus;
 else if (ALU_xor)
 acc <= acc ^ sysbus;
 else if (ALU_comp)
 acc <= ~acc;
 end
 ```

Sequencer:
```verilog
 else
 acc <= sysbus;
 s8: begin
 MDR_bus = 1'b1 ;
 ALU_ACC = 1'b1 ;
 load_ACC = 1'b1 ;
 if (op == `ADD)
 ALU_add = 1'b1 ;
 else if (op == `SUB)
 ALU_sub = 1'b1 ;
 else if (op == `XOR)
 ALU_xor = 1'b1 ;
 else if (op == `COMP)
 ALU_comp = 1'b1 ;
 Next_State = s0;
 end
 ```

CPU:
```verilog
module cpu1 #( parameter WORD_W = 8, OP_W = 3)
 ( input logic clock, n_reset,
 inout wire [WORD_W-1:0] sysbus);
logic ACC_bus, load_ACC, PC_bus, load_PC, load_IR, load_MAR,
MDR_bus, load_MDR, ALU_ACC, ALU_add, ALU_sub, ALU_xor, ALU_comp,
INC_PC, Addr_bus, CS, R_NW, z_flag; 
```

ROM:
```verilog
 case (mar)
 0: mdr = {`STORE, 5'd16 }; //Store the contents of accumulator
 1: mdr = {`LOAD, 5'd16 }; //Load the contents of address into the accumulator
 2: mdr = {`COMP, 5'd8 }; //Complement the contents of the accumulator
 3: mdr = {`STORE, 5'd16 }; //Store the contents of accumulator
 4: mdr = {`LOAD, 5'd16 }; //Load the contents of address into the accumulator
 5: mdr = {`XOR, 5'd9 }; //Xor the contents of address with the accumulator
 6: mdr = {`STORE, 5'd16 }; //Store the contents of accumulator
 7: mdr = {`BNE, 5'd10 }; //Branch if result of last arithmetic operation is not zero
 8: mdr = 1; //contents used by another instruction
 9: mdr = 170; //contents used by another instruction
 10: mdr = 1; //contents used by another instruction
 default : mdr = 0; //rest of ROM is 0
 endcase
 ``` 

I then verified that the program was working correctly by simulating it in modelsim and going through each operation. I found that by the end of the first cycle the value in ram was 1’h55, which was to be expected when 1’hFF and 1’hAA are XORed together.
<img src="/images/sv-microprocessor/waveform4.png" alt="Modelsim output" style="width: 100%; height: 100%">

#### Github Repository: https://github.com/sunbanett40/XOR-Decryptor
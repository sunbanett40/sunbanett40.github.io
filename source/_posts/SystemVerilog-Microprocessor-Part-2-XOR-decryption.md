---
title: SystemVerilog Microprocessor Part 2 - XOR decryption
date: 2021-01-26 20:38:43
tags:
    - Assembler
    - SystemVerilog HDL
    - Microcontroller
---

### Encryption and Decryption
To decrypt a XOR encrypted message the decryption is to XOR the encrypted message with the  key again.

As the key is unknown to us and the number of possible keys (32) is small it is easy to brute force the solution by testing every possible key.

To test the output when the string “oiytmmvk” is decrypted with every possible key I wrote a program in ROM that would load in each value from the switches into a position in the RAM. It would then XOR each of these with a key outputting it to the display. Finally it would increment the key by one and repeat. This program meant I also had to modify the testbench so that it
would input the value wanted.

As the length of my program was now longer than even the 32 total addresses in the 8 bit system, I had to increase the system size to a ten bit system. This provided me with 128 total addresses for my program to be stored.

Updating to 10 bit from 8 bit meant that I had increase the word width so that the system could target each address. I did this by changing the parameter in each file. I also moved the addresses of the the tri-state buffer and the display registers, to positions 126 and 127, so that they weren’t taking ROM slots part way through the program.

I found that I did not need the subtract operation or the complement operation (that I implemented). However, as the total number of operations was still five, requiring 3 bits to encode them still, I did not see the value in removing them from my design.

```verilog
 case (mar)
 0: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 1: mdr = {`STORE, 7'd64 }; //Store the contents of accumulator
 2: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 3: mdr = {`STORE, 7'd65 }; //Store the contents of accumulator
 4: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 5: mdr = {`STORE, 7'd66 }; //Store the contents of accumulator
 6: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 7: mdr = {`STORE, 7'd67 }; //Store the contents of accumulator
 8: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 9: mdr = {`STORE, 7'd68 }; //Store the contents of accumulator
 10: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 11: mdr = {`STORE, 7'd69 }; //Store the contents of accumulator
 12: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 13: mdr = {`STORE, 7'd70 }; //Store the contents of accumulator
 14: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 15: mdr = {`STORE, 7'd71 }; //Store the contents of accumulator
 16: mdr = {`BNE, 7'd52 };
 22: mdr = {`LOAD, 7'd64 }; //Load the character 1 into the accumulator
 23: mdr = {`XOR, 7'd80 }; //XOR accumulator with key
 24: mdr = {`STORE, 7'd127 }; //Output to display
 25: mdr = {`LOAD, 7'd65 }; //Load the character 2 into the accumulator
 26: mdr = {`XOR, 7'd80 }; //XOR accumulator with key
 27: mdr = {`STORE, 7'd127 }; //Output to display
 28: mdr = {`LOAD, 7'd66 }; //Load the character 3 into the accumulator
 29: mdr = {`XOR, 7'd80 }; //XOR accumulator with key
 30: mdr = {`STORE, 7'd127 }; //Output to display
 31: mdr = {`LOAD, 7'd67 }; //Load the character 4 into the accumulator
 32: mdr = {`XOR, 7'd80 }; //XOR accumulator with key 
 33: mdr = {`STORE, 7'd127 }; //Output to display
 34: mdr = {`LOAD, 7'd68 }; //Load the character 5 into the accumulator
 35: mdr = {`XOR, 7'd80 }; //XOR accumulator with key
 36: mdr = {`STORE, 7'd127 }; //Output to display
 37: mdr = {`LOAD, 7'd69 }; //Load the character 6 into the accumulator
 38: mdr = {`XOR, 7'd80 }; //XOR accumulator with key
 39: mdr = {`STORE, 7'd127 }; //Output to display
 40: mdr = {`LOAD, 7'd70 }; //Load the character 7 into the accumulator
 41: mdr = {`XOR, 7'd80 }; //XOR accumulator with key
 42: mdr = {`STORE, 7'd127 }; //Output to display
 43: mdr = {`LOAD, 7'd71 }; //Load the character 8 into the accumulator
 44: mdr = {`XOR, 7'd80 }; //XOR accumulator with key
 45: mdr = {`STORE, 7'd127 }; //Output to display
 46: mdr = {`LOAD, 7'd80 }; //Load key
 47: mdr = {`ADD, 7'd53 }; //Add 1
 48: mdr = {`STORE, 7'd80 }; //Store key
 49: mdr = {`XOR, 7'd54 };
 50: mdr = {`BNE, 7'd55 }; //Loop until all keys are tried
 52: mdr = 22;
 53: mdr = 1;
 54: mdr = 8;
 55: mdr = 16;
 /*RAM:
64: encrypted character 1
65: encrypted character 2
66: encrypted character 3
67: encrypted character 4
68: encrypted character 5
69: encrypted character 6
70: encrypted character 7
71: encrypted character 8
80: key
*/
 default : mdr = 0; //rest of ROM is 0
 endcase
 end
```

This is the start of the program in modelSIM as the value on the switches is loaded in. Brute forcing through all 32 combinations gives the solutions:
<img src="/images/sv-microprocessor/waveform5.png" alt="Modelsim output" style="width: 100%; height: 100%">

```
NHXULLWJ
MK[VOOTI
LJZWNNUH
KM]PIIRO
JL/QHHSN
IO RKKPM
HN.SJJQL
GAQ/EE.C
F@P]DD B
ECS.GG/A
DBR FF]@
CEUXAAZG
BDTY@@[F
AGWZCCXE
@FV[BBYD
YID]]F[ 
.XHE//GZ
][KF DY 
/ZJG..EX
[]M@YYB 
Z/LAXXC.
Y OB[[@]
X.NCZZA\
WQALUUNS
VP@MTTOR
USCNWWLQ
TRBOVVMP
SUEHQQJW
RTDIPPKV
QWGJSSHU
PVFKRRIT
OIYTMMVK
```

The tenth key is a link to the ecs intranet and provides the webpage below:
<img src="/images/sv-microprocessor/message.png" alt="ECS Intranet page" style="width: 100%; height: 100%">


### Encryption and Decryption with loops
Before my invigilator made it clear that a brute force attack was sufficient. I began to try and make a program that could operate loops in such a way that it could change the target address. It would do this by loading a “function” into memory it could then update the address by
overwriting later lines with commands dependent on the outcome from the operation.

This method required the entire program to be written in ROM and the space for the largest function and the auto loader to be in the RAM. This meant that this design still required moreaddress than the eight bits would allow.

```verilog
 //auto Loader
 90: mdr = {`LOAD, (`LOAD position_rom_start)};
 91: mdr = {`ADD, load_position};
 92: mdr = {`STORE, 96}; //update next line
 93: mdr = {`LOAD, (`STORE position_ram_start)};
 94: mdr = {`ADD, position_load};
 95: mdr = {`STORE, 97}; //update next line
 96: mdr = {`LOAD, program_in};
 97: mdr = {`STORE, program_out};
 98: mdr = {`BNE, 90};
 //input
 100: mdr = {`LOAD, switches};
 101: mdr = {`STORE, ACC_TEMP};
 102: mdr = {`LOAD, (`STORE position_character_start)};
 103: mdr = {`ADD, character_position};
 104: mdr = {`STORE, 106};
 105: mdr = {`LOAD, ACC_TEMP};
 106: mdr = {`STORE, position};
 107: mdr = {`BNE, 100};
 //encryption-decryption
 100: mdr = {`LOAD, (`LOAD character_in_start)};
 101: mdr = {`ADD, position};
 102: mdr = {`STORE, 106}; //update next line
 103: mdr = {`LOAD, (`STORE character_out_start)};
 104: mdr = {`ADD, position};
 105: mdr = {`STORE, 108}; //update next line
 106: mdr = {`LOAD, character_in};
 107: mdr = {`XOR, key};
 108: mdr = {`STORE, character_out};
 109: mdr = {`STORE, Display};
 110: mdr = {`LOAD, position};
 111: mdr = {`ADD, 1};
 112: mdr = {`STORE, position};
 113: mdr = {`XOR, length};
 114: mdr = {`BNE, 100};
 115: mdr = {`LOAD, key};
 116: mdr = {`ADD, 1};
 117: mdr = {`STORE, key};
 118: mdr = {`XOR, possible_keys};
 119: mdr = {`BNE, 100};
 ```

I abandoned this method upon understanding that brute force was sufficient. By this point, I had written in pseudo code an auto-loader to load the program. A function to load the inputs and the brute force method with a loop to target different addresses.

### XOR Then XNOR Encryption
I changed the complement into a XNOR function. The XNOR function will add further complexity to the encryption as it adds a second key on top of the existing function.

```verilog
always_ff @( posedge clock, negedge n_reset)
 begin
 if (!n_reset)
 acc <= 0;
 else
 if (load_ACC)
 if (ALU_ACC)
 begin
 if (ALU_add)
 acc <= acc + sysbus;
 else if (ALU_sub)
 acc <= acc - sysbus;
 else if (ALU_xor)
 acc <= acc ^ sysbus;
 else if (ALU_xnor)
 acc <= ~(acc ^ sysbus);
 end
 else
 acc <= sysbus;
 end
endmodule
```

To decrypt this some one will have to know both keys and the order they were applied in. This increases the number of possible sets of keys to 2048. This is still manageable to brute force but will take orders of magnitude longer.

```verilog
 case (mar)
 0: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 1: mdr = {`STORE, 7'd64 }; //Store the contents of accumulator
 2: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 3: mdr = {`STORE, 7'd65 }; //Store the contents of accumulator
 4: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 5: mdr = {`STORE, 7'd66 }; //Store the contents of accumulator
 6: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 7: mdr = {`STORE, 7'd67 }; //Store the contents of accumulator
 8: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 9: mdr = {`STORE, 7'd68 }; //Store the contents of accumulator
 10: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 11: mdr = {`STORE, 7'd69 }; //Store the contents of accumulator
 12: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 13: mdr = {`STORE, 7'd70 }; //Store the contents of accumulator
 14: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 15: mdr = {`STORE, 7'd71 }; //Store the contents of accumulator
 16: mdr = {`LOAD, 7'd64 }; //Load the character 1 into the accumulator
 17: mdr = {`XOR, 7'd51 }; //XOR accumulator with XOR key
 18: mdr = {`XNOR, 7'd52 }; //XNOR accumulator with XNOR key
 19: mdr = {`STORE, 7'd127 }; //Output to display
 20: mdr = {`LOAD, 7'd65 }; //Load the character 2 into the accumulator
 21: mdr = {`XOR, 7'd51 }; //XOR accumulator with XOR key
 22: mdr = {`XNOR, 7'd52 }; //XNOR accumulator with XNOR key
 23: mdr = {`STORE, 7'd127 }; //Output to display
 24: mdr = {`LOAD, 7'd66 }; //Load the character 3 into the accumulator
 25: mdr = {`XOR, 7'd51 }; //XOR accumulator with XOR key
 26: mdr = {`XNOR, 7'd52 }; //XNOR accumulator with XNOR key 
 27: mdr = {`STORE, 7'd127 }; //Output to display
 28: mdr = {`LOAD, 7'd67 }; //Load the character 4 into the accumulator
 29: mdr = {`XOR, 7'd51 }; //XOR accumulator with XOR key
 30: mdr = {`XNOR, 7'd52 }; //XNOR accumulator with XNOR key
 31: mdr = {`STORE, 7'd127 }; //Output to display
 32: mdr = {`LOAD, 7'd68 }; //Load the character 5 into the accumulator
 33: mdr = {`XOR, 7'd51 }; //XOR accumulator with XOR key
 34: mdr = {`XNOR, 7'd52 }; //XNOR accumulator with XNOR key
 35: mdr = {`STORE, 7'd127 }; //Output to display
 36: mdr = {`LOAD, 7'd69 }; //Load the character 6 into the accumulator
 37: mdr = {`XOR, 7'd51 }; //XOR accumulator with XOR key
 38: mdr = {`XNOR, 7'd52 }; //XNOR accumulator with XNOR key
 39: mdr = {`STORE, 7'd127 }; //Output to display
 40: mdr = {`LOAD, 7'd70 }; //Load the character 7 into the accumulator
 41: mdr = {`XOR, 7'd51 }; //XOR accumulator with XOR key
 42: mdr = {`XNOR, 7'd52 }; //XNOR accumulator with XNOR key
 43: mdr = {`STORE, 7'd127 }; //Output to display
 44: mdr = {`LOAD, 7'd71 }; //Load the character 8 into the accumulator
 45: mdr = {`XOR, 7'd51 }; //XOR accumulator with XOR key
 46: mdr = {`XNOR, 7'd52 }; //XNOR accumulator with XNOR key
 47: mdr = {`STORE, 7'd127 }; //Output to display
 51: mdr = 21; //XOR Key
 52: mdr = 10; //XNOR key
 53: mdr = 1;
 54: mdr = 8;
 55: mdr = 16;
 default : mdr = 0; //rest of ROM is 0
 endcase
 end 
```

### XOR With Multiple Keys Encryption
Finally I implemented a new program that would encrypt or decrypt the message using a key of multiple bytes. This program would read in both the string and a key, of length 3.

```verilog
 case (mar)
 0: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 1: mdr = {`STORE, 7'd64 }; //Store the contents of accumulator
 2: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 3: mdr = {`STORE, 7'd65 }; //Store the contents of accumulator
 4: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 5: mdr = {`STORE, 7'd66 }; //Store the contents of accumulator
 6: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 7: mdr = {`STORE, 7'd67 }; //Store the contents of accumulator
 8: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 9: mdr = {`STORE, 7'd68 }; //Store the contents of accumulator
 10: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 11: mdr = {`STORE, 7'd69 }; //Store the contents of accumulator
 12: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 13: mdr = {`STORE, 7'd70 }; //Store the contents of accumulator
 14: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 15: mdr = {`STORE, 7'd71 }; //Store the contents of accumulator
 16: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator 
 17: mdr = {`STORE, 7'd72 }; //Store the contents of accumulator
 18: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 19: mdr = {`STORE, 7'd73 }; //Store the contents of accumulator
 20: mdr = {`LOAD, 7'd126 }; //Load the contents of address into the accumulator
 21: mdr = {`STORE, 7'd74 }; //Store the contents of accumulator
 22: mdr = {`LOAD, 7'd64 }; //Load the character 1 into the accumulator
 23: mdr = {`XOR, 7'd72 }; //XOR accumulator with key
 24: mdr = {`STORE, 7'd127 }; //Output to display
 25: mdr = {`LOAD, 7'd65 }; //Load the character 2 into the accumulator
 26: mdr = {`XOR, 7'd73 }; //XOR accumulator with key
 27: mdr = {`STORE, 7'd127 }; //Output to display
 28: mdr = {`LOAD, 7'd66 }; //Load the character 3 into the accumulator
 29: mdr = {`XOR, 7'd74 }; //XOR accumulator with key
 30: mdr = {`STORE, 7'd127 }; //Output to display
 31: mdr = {`LOAD, 7'd67 }; //Load the character 4 into the accumulator
 32: mdr = {`XOR, 7'd72 }; //XOR accumulator with key
 33: mdr = {`STORE, 7'd127 }; //Output to display
 34: mdr = {`LOAD, 7'd68 }; //Load the character 5 into the accumulator
 35: mdr = {`XOR, 7'd73 }; //XOR accumulator with key
 36: mdr = {`STORE, 7'd127 }; //Output to display
 37: mdr = {`LOAD, 7'd69 }; //Load the character 6 into the accumulator
 38: mdr = {`XOR, 7'd74 }; //XOR accumulator with key
 39: mdr = {`STORE, 7'd127 }; //Output to display
 40: mdr = {`LOAD, 7'd70 }; //Load the character 7 into the accumulator
 41: mdr = {`XOR, 7'd72 }; //XOR accumulator with key
 42: mdr = {`STORE, 7'd127 }; //Output to display
 43: mdr = {`LOAD, 7'd71 }; //Load the character 8 into the accumulator
 44: mdr = {`XOR, 7'd73 }; //XOR accumulator with key
 45: mdr = {`STORE, 7'd127 }; //Output to display
 46: mdr = {`BNE, 7'd55 }; //Loop until all keys are tried
 53: mdr = 1;
 54: mdr = 8;
 55: mdr = 22;

/*RAM:
64: encrypted character 1
65: encrypted character 2
66: encrypted character 3
67: encrypted character 4
68: encrypted character 5
69: encrypted character 6
70: encrypted character 7
71: encrypted character 8
72: key 1
73: key 2
74: key 3
*/

 default : mdr = 0; //rest of ROM is 0
 endcase
 end
```

To brute force a key length of three it would have to try 32768 possible combinations of keys. However, if the length of the key is unknown it becomes exponentially harder to brute force the attack.

This method is scalable with different key lengths but can still be decrypted as long as the length of the key is less than the length of the encrypted string.

#### Github Repository: https://github.com/sunbanett40/XOR-Decryptor
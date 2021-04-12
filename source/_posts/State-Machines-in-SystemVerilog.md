---
title: State Machines in SystemVerilog
date: 2021-01-19 20:38:25
tags:
    - State Machines
    - SystemVerilog HDL
---

A state machine circuit has two main parts to it, a sequential series of memory states, and control logic to determine the next state. When converting a state machine into SystemVerilog making a distinction between the two elements can be useful. The memory states can be described by an always_ff block; the control logic can be described as an always_comb block.

```verilog
//SystemVerilog State Machine template

module template (output logic output1, output2, output3
                  input logic clock, n_reset, input1, input2);

enum {state1, state2, state3} present_state, next_state;

always_ff @(posedge clock, negedge n_reset)
    begin: SEQ                              //Sequential label for modelsim
        if (!n_reset)                       //Reset
            present_state <= state1;
        else                                //Update state on clockedge
            present_state <= next_state;
    end

always_comb
    begin: COM                              //Combinational label for modelsim

        //Set default values of output
        output1 = '0
        output2 = '0
        output3 = '0

        unique case (present_state)         //Unique case label gives compiler error if a state is missing
            state1 : begin
                output1 = '1;               //Assert unconditional output
                if (input1)                 //Conditional to progress to next state
                    next_state = state2;
                else                        //Else stay in ready state
                    next_state = state1;
            end
            state2 : begin
                output2 = '1;               //Assert unconditional output
                if (input2)                 //Conditional to progress to next state
                    next_state = state3;
                else
                    begin
                        output1 = '1;       //Assert conditional output
                        next_state = state1;//Else go to ready state
                    end
            end
            state3 : begin
                output3 = '1;               //Assert unconditional output
                next_state = state1;        //Progress to next state
            end
        endcase
    end
endmodule
```
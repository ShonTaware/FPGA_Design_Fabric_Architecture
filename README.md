# FPGA - Fabric, Design and Architecture
  This repository contains all the information studied and created during the [FPGA - Fabric, Design and Architecture](https://www.vlsisystemdesign.com/fpga/) workshop. It is primarily foucused on a complete FPGA flow using the maximum open-soucre tools.

# Table of Contents
  - [Introduction To FPGA](#introduction-to-fpga)
    - [FPGA vs ASIC Comparison](#fpga-vs-asic-comparison)
  - [Introduction To Xilinx Vivado](#introduction-to-xilinx-vivado)
  - [List of All Open-Source Tools Used](#list-of-all-open-source-tools-used)
  - [Setting Up Environment](#setting-up-environment)
  - [Day 1 - Exploring FPGA Basics and Vivado](#day-1---exploring-fpga-basics-and-vivado)
    - [FPGA Basics](#fpga-basics)
      - [FPGA Architecture](#fpga-architecture)
      - [Configurable Logic Blocks](#configurable-logic-blocks)
      - [Basys FPGA Board](#basys-fpga-board)
    - [Counter Example in Vivado](#counter-example-in-vivado)
      - [Counter Elaboration](#counter-elaboration)
      - [Counter Synthesis](#counter-synthesis)
      - [Counter Implementation](#counter-implementation)
      - [Constraints](#constraints)
      - [Bitstream](#bitstream)
      - [Counter Timing, Power and Area](#counter-timing-power-and-area)
    - [Introduction To VIO](#introduction-to-vio)
  - [Day 2 - Exploring OpenFPGA, VPR and VTR](#day-2---exploring-openfpga-vpr-and-vtr)
    - [Introduction To OpenFPGA](#introduction-to-openfpga)
    - [VPR](#vpr)
    - [VTR](#vtr)
      - [VTR Flow](#vtr-flow)
      - [Post Synthesis Simulation](#post-synthesis-simulation)
      - [Timing Area VTR Flow](#timing-area-vtr-flow)
      - [Power Analysis VTR](#power-analysis-vtr)
    - [Basys3 vs VTR Earch Comparison](#basys3-vs-vtr-earch-comparison)
  - [Day 3 - RISCV Core Programming Using Vivado](#day-3---riscv-core-programming-using-vivado)
    - [RTL To Synthesis](#rtl-to-synthesis)
    - [Synthesis To Bitstream](#synthesis-to-bitstream)
  - [Day 4 - Pre-layout timing analysis and importance of good clock tree](#day-4---pre-layout-timing-analysis-and-importance-of-good-clock-tree)
    - [Magic Layout to Standard Cell LEF](#magic-layout-to-standard-cell-lef)
    - [Timing Analysis using OpenSTA](#timing-analysis-using-opensta)
    - [Clock Tree Synthesis using TritonCTS](#clock-tree-synthesis-using-tritoncts)
  - [Day 5 - Final steps for RTL2GDS](#day-5---final-steps-for-rtl2gds)
    - [Generation of Power Distribution Network](#generation-of-power-distribution-network)
    - [Routing using TritonRoute](#routing-using-tritonroute)
    - [SPEF File Generation](#spef-file-generation)
  - [References](#references)
  - [Acknowledgement](#acknowledgement)
 
# Introduction To FPGA
  FPGA (Field Programmable Gate Array) are intergated circuits which have a complex arrangement of configurable logic blocks (CLBs) and programmable interconnects. 
 
 ## FPGA vs ASIC Comparison
   | FPGA                                                                         | ASIC                                           |
   | ---                                                                          | ---                                            |
   | Field Programmable Gate Array                                                | Application Specific Integrated Circuit        |
   | RTL to Bitstream                                                             | RTL to Layout                                  |
   | Reconfigurable Circuit                                                       | Permanent Circuit                              |
   | Less Energy Efficient. Required more power for same task as compared to ASIC | More energy efficient                          |
   | Useful for prototyping or validating a design                                | Used for final product design after validation |

# Introduction To Xilinx Vivado


# Day 1 - Exploring FPGA Basics and Vivado
 ## FPGA Basics
   ### FPGA Architecture
    The FPGA Architecture primarily consists of :
    - Configurable Logic Blocks
    - Programmable Interconnects 
    - I/O Cells
    - Memory / Block RAM
    
    <img src="images/fpga_arch_1.png">

   ### Configurable Logic Block
    Configurable Logic Block (CLB) is responsible for the combinational or sequential logic implementation. CLB consists of :

    - Look-up Table (LUT) - Logic function implementation
    - Carry and Control Logic - Arithmetic Operations
    - Flip-flops and/or latches

   ### Basys FPGA Board

    <img src="images/basys3_board.png">

   | No  | Description               | No  | Description |
   | --- | ---                       | --- | ---         |
   | 01  | Power Good LED            | 09  | Reset       |
   | 02  | I/O                       | 10  |             |
   | 03  | I/O                       | 11  |             |
   | 04  | Four 7-segment Display    | 12  |             |
   | 05  | Slide switches            | 13  |             |
   | 06  | LEDs                      | 14  |             |
   | 07  | Pushbuttons               | 15  |             |
   | 08  | FPGA programming done LED | 16  |             |

 ## Counter Example in Vivado

<table>
<tr>
    <td> Design </td><td> Testbench </td>
</tr>
<tr>     
    <td>
```verilog
   `timescale 1ns / 1ps
   // Description: 4 bit counter with source clock (100MHz) division.

   //////////////////////////////////////////////////////////////////////////////////
   module counter_clk_div(clk,rst,counter_out);
   input clk,rst;
   reg div_clk;
   reg [25:0] delay_count;
   output reg [3:0] counter_out;

   //////////clock division block////////////////////
   always @(posedge clk)
   begin
       if(rst) begin
           delay_count<=26'd0;
           counter_out<=4'b0000;
           div_clk <= 1'b0; //initialise div_clk
           counter_out<=4'b0000;
       end
       else if(delay_count==26'd212) begin
           delay_count<=26'd0; //reset upon reaching the max value
           div_clk <= ~div_clk;  //generating a slow clock
       end
       else begin
           delay_count<=delay_count+1;
       end
   end


   /////////////4 bit counter block///////////////////
   always @(posedge div_clk)
   begin
       if(rst) 
           counter_out<=4'b0000;
       else
           counter_out<= counter_out+1;
   end

   endmodule

``` 

</td>
<td></td>
</tr>

</table>

   ### Counter Elaboration

   ### Counter Synthesis

   ### Counter Implementation

   ### Constraints

   ### Bitstream

   ### Counter Timing, Power and Area

 ## Introduction To VIO

# Day 2 - Exploring OpenFPGA, VPR and VTR

 ## Introduction To OpenFPGA
 ## VPR
 ## VTR
   ### VTR Flow
   ### Post Synthesis Simulation
   ### Timing Area VTR Flow
   ### Power Analysis VTR
 ## Basys3 vs VTR Earch Comparison

# Day 3 - RISCV Core Programming Using Vivado
 ## RTL To Synthesis](#rtl-to-synthesis)
 ## Synthesis To Bitstream](#synthesis-to-bitstream)
   
# References
  - VLSI System Design: https://www.vlsisystemdesign.com/ip/
  - 4-stage RISC-V Core: https://github.com/ShonTaware/RISC-V_Core_4_Stage
  - RISC-V based Microprocessor: https://github.com/shivanishah269/risc-v-core

# Acknowledgement
  - [Kunal Ghosh](https://github.com/kunalg123), Co-founder, VSD Corp. Pvt. Ltd.
  - 

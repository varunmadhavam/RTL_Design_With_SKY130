# RTL design using Verilog with SKY130 Technology

![](/src/img/Verilog-flyer.png)

This repo aims to document the 5 day [workshop](https://www.vlsisystemdesign.com/rtl-design-using-verilog-with-sky130-technology/) conducted by [VSD Corp. Pvt Ltd](https://www.vlsisystemdesign.com/) on RTL desing using verilog with SKY130 technology.

# Table of Contents

1. [Introduction](#introduction)
2. [Day1 - Introduction to Verilog RTL Design and Synthesis](#day1---introduction-to-verilog-rtl-design-and-synthesis)
3. [Day2]
4. [Day3]
5. [Day4]
6. [Day5]
7. [FAQs](#faqs)
8. [Acknowledgemnts](#acknowledgements)
9. [References](#references)


# Introduction
The below represent a generic ASIC flow.

![](/src/img/asic-flow.png)

The workshop intends to cover in detail the step marked in red, that is **logic synthesis**

## What is logic synthesis .!?
Logic sysnthesis is the process of translating your RTL Desing, which is the behavioral represntation of some specifications, usaully written in a HDL language like verilog, to a gate level netlist. The netlist contains a set of gates and connections between them as per the description. ......

## Why is it important .!?

# Day1 - Introduction to Verilog RTL Design and Synthesis
1. Setting Up the Lab.
    - Login to your lab instance and in our home directory create a directory named VLSI.
      - 
        ``` 
            cd ~
            mkdir VLSI
        ```
      - ![](/src/img/mkdir.png)

# FAQs
1.  What is the significance of -lib while importing liberty file in yosys
    -   the -lib option creates like a library/list of all the cells present in the .lib file with only IO ports and not the internal structure.

2. yosys produces different implemnetations for the same circuit and standard libary file
    - This would mostly be because of difference in the versions of yosys that you are running. Different versions might be optimised for different areas of optimisations like power, performance or area. Also later version might have improvments in algorithms that might help it to choose a better standard cell for the same design.
    
# Acknowledgements

* Kunal Gosh, Co-Founder (VSD Corp. Pvt Ltd)
* Shon Taware, Teaching Assistant (VSD Corp. Pvt Ltd)

# References
1. https://www.vlsisystemdesign.com/rtl-design-using-verilog-with-sky130-technology/
2. http://verificationexcellence.in/vlsi-design-front-end-and-back-end/

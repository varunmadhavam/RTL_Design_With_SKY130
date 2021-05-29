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

## What is logic synthesis .!?
Logic sysnthesis is the process of translating your RTL Design, which is the behavioral representation of some specifications, usaully written in a HDL language like verilog, to an optimised gate level netlist based on a given set of design contraints and a standard cell library.
![](/src/img/synth.png)

## Synthesis Steps
  1. **Translation** : 
     In this step, the input hdl files are processed and the constructs in them are interpreted as various logical structures. An example would be an if-else statement might infer a MUX and so on. Contraints and optimisation are not taken into account in this stage.
  2. **Optimisation** : 
     In this step, various techniques are used to optimize and minimise the logic and remove redundant logic.
  3. **Mapping** : 
     In this step, the desing is mapped to the desired target technology, based on the standard cell library. This implimentation also takes into account the design contraints given as inputs like power,area and timing and tries to satisfy them.

## Inputs To The Synthesis Process
  1. **RTL Description** :
     These are your HDL source files which contains a behavioral description of the specifications of the design.
  2. **The Standard Cell Library** :
     The library file contains a collection of cells that are proivded by the foundary. These includes basic logic gates like inverters, and gates, or gates etc as well as macrocells like multiplexers, flip flops etc. Each cell has an associated description which contains detailed  information regarding its functionality, timing , power and area in a format which is readable by the synthesis tool.
  3. **Contraints** : 
     These contains guidelines to the synthesiser on how to optimise the desing. Based on the contraints, the synthesiser can choose different flavours from the standard cell to implement the same logic while trying to satisfy the various design contraints. The contraints typically includes timing requirements, area restrictions and power limitations.
## Output Of The Synthesis Process
  1. The output of the systhesis process is a optimised gate-level netlist mapped to the given target technology cells. 

## Why is it important .!?
  **TBD**

# Day1 - Introduction to Verilog RTL Design and Synthesis
1. ### Setting Up the Lab.
    - Login to your lab instance and in our home directory create a directory named VLSI.
      ![](/src/img/mkdir.png)
      ``` 
      cd ~
      mkdir VLSI
      ```
    - Clone the github repos into the VLSI directory.
      ![](/src/img/clone.png)
      ``` 
      git clone https://github.com/kunalg123/vsdflow.git
      git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
      ```
    - Go to the newly created vsdflow directory and execute the command ./opensource_eda_tool_install.sh . This will begin the installation of the various tools required for this workshop.(**Note. The tools would have been alredy installed on the lab machines and this step is not necessary**)
      ![](/src/img/vsdinstall1.png)
      ``` 
      cd /home/varun/VLSI/vsdflow
      ./opensource_eda_tool_install.sh
      ```
    - Directory Structure of sky130RTLDesignAndSynthesisWorkshop directory
      ![](/src/img/ds.png)
2. ### Simulating the Designs with iverilog
    1. **what is simulation !?** : 
    Simulation is the process of checking whether the desing is adhering to the given specs. The tool used for simulating the design is called a simulator.
    2. **How does a simulator work** : 
    Simulator works by continiously monitoring the inputs for changes. Upon a change in any one of the inputs, the outputs are re evaluated. The changes to the inputs and corresponding changes to the output can be logged in a file for later analysis.
    3. **Inputs to the simulator** : 
    The simulator accepts two main inputs.
        1. The Design : This is usaully the behavioral description of the specs in some HDL language.
        2. The Testbench : The testbench is a setup to apply stimulas or test vectors to the design to check its functionality and correctness.
    4. **iverilog Simulation Flow**

     ![](/src/img/iverilogn.png)
      1. Run iverilog with the design verilog file and the testbench as inputs. This will create an executable named a.out.
         ![](/src/img/iverilog1.png)
         ```
         iverilog good_mux.v tb_good_mux.v
         ```
      2. Execute the file a.out. This will generate the value change dump (.vcd) file.
         ![](/src/img/iverilog2.png)
         ```
         ./a.out
         ```
      3. Now run gtkwave with the vcd file as input to view the timing diagram.
         ![](/src/img/iverilog3.png)
         ```
         gtkwave tb_good_mux.vcd
         ```
      4. To view the signal on the wave window click and drag them to the signal column.
         ![](/src/gif/iverilog44.gif)

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
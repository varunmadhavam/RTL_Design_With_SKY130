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
  1. **Translation**\
     In this step, the input hdl files are processed and the constructs in them are interpreted as various logical structures. An example would be an if-else statement might infer a MUX and so on. Contraints and optimisation are not taken into account in this stage.
  2. **Optimisation**\
     In this step, various techniques are used to optimize and minimise the logic and remove redundant logic.
  3. **Mapping**\
     In this step, the desing is mapped to the desired target technology, based on the standard cell library. This implimentation also takes into account the design contraints given as inputs like power,area and timing and tries to satisfy them.

## Inputs To The Synthesis Process
  1. **RTL Description**\
     These are your HDL source files which contains a behavioral description of the specifications of the design.
  2. **The Standard Cell Library**\
     The library file contains a collection of cells that are proivded by the foundary. These includes basic logic gates like inverters, and gates, or gates etc as well as macrocells like multiplexers, flip flops etc. Each cell has an associated description which contains detailed  information regarding its functionality, timing , power and area in a format which is readable by the synthesis tool.
  3. **Contraints**\
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
    1. **what is simulation !?**\
    Simulation is the process of checking whether the desing is adhering to the given specs. The tool used for simulating the design is called a simulator.
    2. **How does a simulator work**\
    Simulator works by continiously monitoring the inputs for changes. Upon a change in any one of the inputs, the outputs are re evaluated. The changes to the inputs and corresponding changes to the output can be logged in a file for later analysis.
    3. **Inputs to the simulator**\
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
      5. To zoom in, click on the area that you want to zoom in and then click on the [+] button.
         ![](/src/gif/iverilog5.gif)
3. ### Synthesis with Yosys
    1. **What is yosys.!?**\
       Yosys is free software framework for RTL synthesis licensed under the ISC license. It currently has extensive Verilog-2005 support and provides a basic set of synthesis algorithms for various application domains. Yosys  reads a design  from the verilog file, synthesizes it to a gate-level netlist using the cell library in the given Liberty file  and writes the synthesized results as Verilog netlist to output.
    2. **The Standard Library File**\
       The standard library file is a collection of logical modules. It can include basic gates like not, and , or etc and macrocells like flops and muxes. Further many flavours of the same gate might be presnet like slow, medium, fast as well as multiple input options like 2 inputs , 3 inputs etc..
    3. **Why different flavours of gates**\
       The combinational delay in the logic path between two flops determine the maximum frequency of operation. The delay should be such that the data produced by the source flop reaches the input of the destination flop Tsetup time before the capturing clock edge, to avoid setup violations. Slower the delay, faster we can operate the circuit without failing this condition. So are only fast gates sufficient..!?. Its seems that we have to ensure a minimum delay on the path too. This ensure thats the data launched by a flop is captured by the other flop only in the next clock cycle and not the current cycle. Cases where this conditions are not met are called hold violations. So we need a mix of fast and slow gates for a proper working design.
    4. **Selection of cell**\
       The synthesizer selects the best cell from the standard library based on the inputs given to it called contraints. Contraints are the designers guide to the systnesis tool on what to optimise the desing for, like power, performance or area. For high performance, the tool might choose faster gates which would indeed result in high power and more area. If slower gates are used to minimise power, the performace of the desing will be impacted.
    5. **Yosys flow**
        1. start yosys.
           ![](/src/img/yosys1.png)
           ```
           yosys
           ```
        2. load the sky130 standard library.
           ![](/src/img/yosys2.png)
           ```
           read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
           ```
        3. Read the design files
           ![](/src/img/yosys3.png)
           ```
           read_verilog good_mux.v
           ```
        4. Synthesize the top level module
           ![](/src/img/yosys4.png)
           ```
           synth -top good_mux
           ```
           ![](/src/img/yosys5.png)
        
        5. Map to the standard library
           ![](/src/img/yosys6.png)
           ```
           abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
           ```
           ![](/src/img/yosys7.png)
        6. Two view the result as a graphich use the show command.
           ```
           show
           ```
           ![](/src/img/yosys8.png)
        7. To write the result netlist to a file use the write_veriog command. This will output the netlist to a file in the current directory.
           ```
           write_verilog -noattr synth_result.v
           ```
           ![](/src/img/yosys9.png)

# Day2 - Timing libs, hierarchical vs flat synthesis and efficient flop coding styles 
1. ### The standard cell library
   A standrard cell library is a collection of charactrized logic gates that can be used to implement digital circuits. This is usaully part of set of file created by the foundary called the PDK(Process Develpment Kit). The designers use the PDK to design, simulate, draw and verify the design before handing the design back to the foundry to produce chips.\
   The standard libray contains.
   1. Behavioral Views\
      HDL Descriptions of the cells that can be used for simulation and logic equivalence checking.
   2. Physical View\
      Contains layout of cells(GDSII) and abstract of cells(LEF)
   3. Transistor Level View\
      Can be used for spice simulations.
   4. Timing/Power
      Liberty files with charactrization of timing and power.
   etc...
2. ### The SkyWater Open Source PDK
   The SkyWater Open Source PDK is a collaboration between Google and SkyWater Technology Foundry to provide a fully open source Process Design Kit and related resources, which can be used to create manufacturable designs at SkyWater’s facility. In this workshop we will be targetting this foundary and hence will be using the library file from this pdk for synthesis.
3. ### Library naming convention
   The SkyWater Foundry Provides multiple Standard Cell Libraries. For this workshop we will be using the "sky130_fd_sc_hd". The sky130_fd_sc_hd library is designed for high density. This library enables higher routed gated density, lower dynamic power consumption, and comparable timing and leakage power. As a trade-off it has lower drive strength. Specifically this workshop uses the library "sky130_fd_sc_hd__tt_025C_1v80".
   ![](/src/img/libname.png)
4. ### The .lib(liberty) File contents
   The timing data of standard cells is provided in the liberty format. Every .lib file will provide timing, power, noise, area information for a single corner ie process,voltage, temperature etc.
   1. Library\
      general information common to all cells in the library.
   2. Cell\
      specific information about each standard cell.
   3. Pin\
      Timing, power, capacitance, leakage functionality etc characteristics for each pin in each cell.
   ![](/src/img/lib1.png)
   ![](/src/img/lib2.png)
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
2. https://www.eng.biu.ac.il/temanad/files/2017/02/Lecture-4-Standard-Cell-Libraries.pdf
3. https://skywater-pdk.readthedocs.io/en/latest/
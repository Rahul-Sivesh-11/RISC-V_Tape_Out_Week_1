# Day 2: Timing Libraries, Synthesis Methods, and Flip-Flop Coding Practices
- Introduction to timing libraries such as sky130_fd_sc_hd__tt_025C_1v80.lib commonly used in open-source PDKs.
- Understanding the difference between hierarchical synthesis and flat synthesis approaches.
- Learning best practices for writing efficient flip-flop code in RTL design.
## Timing Libraries
### SKY130 PDK Overview
The SKY130 PDK is an open-source Process Design Kit developed around SkyWater’s 130nm CMOS technology. It contains the necessary models and libraries required for IC design, covering aspects such as timing, power, and process variations.

In timing libraries (e.g., sky130_fd_sc_hd__tt_025C_1v80.lib), the filename itself encodes the PVT (Process, Voltage, Temperature) conditions under which the standard cells are characterized. Knowing how to interpret this naming format allows designers to link the library with its corresponding operating conditions.
- **sky130_fd_sc_hd** – Base library name:  
  - `sky130` → SkyWater 130nm process  
  - `fd` → Fully Depleted (process type)  
  - `sc` → Standard Cells  
  - `hd` → High Density variant 

#### Decoding tt_025C_1v80 in the SKY130 PDK

`tt`: Typical process corner.
 - `tt` → Typical-Typical (nominal NMOS and PMOS)

`025C`: Represents a temperature of 25°C, relevant for temperature-dependent performance.

`1v80`: Indicates a core voltage of 1.8V.

This systematic naming allows tools to select the appropriate library for synthesis, timing analysis, or corner-based verification.
The delay model LUT is used for the timing analysis.
### Understanding the .lib File
- It is a collection of standard logic cells.
- Contains various logic gates characterized for different input conditions.
- Provides multiple variants (flavors) of the same gate.
- Includes both high-speed cells (for meeting performance requirements) and slower cells (to satisfy hold constraints).
  The `.lib` file defines each standard cell in detail, including:
- Cell name and function (e.g., AND, OR, D flip-flop)
- Pin details (inputs, outputs, clock signals)
- Timing parameters such as setup, hold, and propagation delays
- Power data, including dynamic and leakage power
- Operating conditions for different PVT corners (process, voltage, temperature)
### Opening and Exploring the .lib File

To open the sky130_fd_sc_hd__tt_025C_1v80.lib file:

**Open the file:**
```shell
vim sky130_fd_sc_hd__tt_025C_1v80.lib
```
![To open the file](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/Screenshot%202025-09-26%20150019.png)
### What is Synthesis?
Synthesis is the process of transforming RTL code (written in Verilog or VHDL) into a gate-level netlist using a standard cell library. In this step, the high-level design description is translated into actual logic gates and flip-flops that can be implemented and eventually fabricated on silicon.
### Hierarchical Synthesis
-In hierarchical synthesis, the design is compiled module by module, maintaining the original RTL hierarchy.
#### Advantages:
- Each module is synthesized independently.
- Preserves the design structure → makes debugging and reuse easier.
- Netlist remains organized according to modules.
- Simplifies debugging and management of large projects.
- Reusable blocks can be synthesized once and used multiple times.
- More efficient in terms of runtime and memory for very large designs.
#### Disadvantages:
- Limited optimization across module boundaries.
- May result in slightly higher area or slower timing compared to flat synthesis.
### Flat Synthesis
In flat synthesis, the design is treated as a single block, with module boundaries removed during compilation.
#### Advantages:
- Enables cross-module optimizations.
- Can produce better results for area, timing, and power.
- Often delivers improved overall performance.
- Generates a single-level netlist.
#### Disadvantages:
- Higher memory usage and longer runtime for large designs.
- Debugging and modifying specific parts of the design becomes difficult.
![multiple modules](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/Screenshot%202025-09-26%20154617.png)
## Hierarchical Synthesis Workflow
### Hierarchical synthesis of multiple_modules.v using Yosys:

### 1. Open the directory where you would want to run the synthesis

```bash
cd ~/vcd/photos
```

### 2. Run hierarchical synthesis in Yosys:
```bash
yosys
```

Inside yosys,

```bash
read_liberty -lib ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files/multiple_modules.v 
dfflibmap -liberty ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
abc -liberty ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
synth -top multiple_modules
show multiple_modules
show -format png multiple_modules
write_verilog ~/vsd/VLSI/multiple_modules_hier
```

This approach keeps sub-modules separate, making it easier to debug, reuse, and manage large designs, while still producing a synthesized netlist compatible with the SkyWater 130nm PDK.


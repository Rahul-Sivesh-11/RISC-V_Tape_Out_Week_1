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
![Code](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/Screenshot%202025-09-26%20155003.png)
### Verify the Synthesis:
- **Netlist Dot file**
![netlist](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/Screenshot%202025-09-26%20155238.png)
### Statistics

 ```bash

=== multiple_modules ===

   Number of wires:                  5
   Number of wire bits:              5
   Number of public wires:           5
   Number of public wire bits:       5
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:                  2
     sub_module1                     1
     sub_module2                     1

=== sub_module_01 ===

   Number of wires:                  3
   Number of wire bits:              3
   Number of public wires:           3
   Number of public wire bits:       3
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:                  1
     $_AND_                          1

=== sub_module_02 ===

   Number of wires:                  3
   Number of wire bits:              3
   Number of public wires:           3
   Number of public wire bits:       3
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:                  1
     $_OR_                           1

=== design hierarchy ===

   multiple_modules                  1
     sub_module_01                   1
     sub_module_02                   1

   Number of wires:                 11
   Number of wire bits:             11
   Number of public wires:          11
   Number of public wire bits:      11
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:                  2
     $_AND_                          1
     $_OR_                           1
```
---

## Flat Synthesis Workflow

### Flat synthesis of multiple_modules.v using Yosys:

### 1. Open the directory where you want to run the synthesis

```bash
cd ~/VCD/pictures
```
### 2. Run flat synthesis in Yosys:

```bash
yosys
```
Inside yosys, 
```bash
read_verilog ~/VSD/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files/multiple_modules.v                         
synth -top multiple_modules
abc -liberty ~/VSD/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib      
flatten
show multiple_modules
show -format png multiple_modules
write_verilog ~/VSD/photos/multiple_modules_flat.v
```
#### Explanation:

`synth -top` generates the gate-level netlist of the flattened design.

`abc` performs technology mapping and logic optimization.

`flatten` removes module hierarchy so the tool can optimize across the entire design.

`write_verilog` outputs the flat netlist ready for further analysis or place-and-route.
![Code](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/Screenshot%202025-09-26%20155737.png)
#### Netlist dot file
![Netlist dot file](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/Screenshot%202025-09-26%20160016.png)
### Statistics

```bash

=== multiple_modules ===

   Number of wires:                  5
   Number of wire bits:              5
   Number of public wires:           5
   Number of public wire bits:       5
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:                  2
     sub_module_01                   1
     sub_module_02                   1

=== sub_module_01 ===

   Number of wires:                  3
   Number of wire bits:              3
   Number of public wires:           3
   Number of public wire bits:       3
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:                  1
     $_AND_                          1

=== sub_module_02 ===

   Number of wires:                  3
   Number of wire bits:              3
   Number of public wires:           3
   Number of public wire bits:       3
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:                  1
     $_OR_                           1

=== design hierarchy ===

   multiple_modules                  1
     sub_module_01                   1
     sub_module_02                   1

   Number of wires:                 11
   Number of wire bits:             11
   Number of public wires:          11
   Number of public wire bits:      11
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:                  2
     $_AND_                          1
     $_OR_                           1
```
### Sub-Module Synthesis
RTL designs are usually structured in a modular fashion, containing several functional blocks or sub-modules. Sub-module synthesis focuses on compiling each of these blocks independently rather than the entire design at once.
#### Why is sub-module synthesis important?
- Optimization and Area Efficiency: By handling sub-modules separately, the synthesis tool can fine-tune each block for better performance. This includes logic simplification, technology mapping, and minimizing silicon area.
- Faster Processing: Since sub-modules are independent, they can be synthesized in parallel, reducing overall runtime. For large-scale designs, this parallelism greatly improves synthesis turnaround time.
- ### Open the directory where you want to run the sub-module synthesis

```bash
cd ~/vcd/photos
yosys
```
*Inside yosys:*

```bash
read_liberty -lib ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files/multiple_modules.v
synth -top sub_module1
abc -liberty ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
show -format png
```
### Verification of Sub-module Synthesis:

#### Netlist Dot file
![netlist](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(44).png)
Staistics:

```bash
=== sub_module1 ===

   Number of wires:                  3
   Number of wire bits:              3
   Number of public wires:           3
   Number of public wire bits:       3
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:                  1
     $_AND_                          1
```
---

## The Various Flip-Flop Coding Styles  

### 1. **Asynchronous Reset Flip-Flop**

```verilog
always @(posedge clk or posedge rst) begin
    if (rst)
        q <= 0;
    else
        q <= d;
end
```
- Output q resets immediately when rst is asserted.

- Useful for global reset in ASIC/FPGA designs.


### 2. **Asynchronous Set Flip-Flop**
```verilog
always @(posedge clk or posedge set) begin
    if (set)
        q <= 1;
    else
        q <= d;
end
```

- Output q is set to 1 immediately when set is asserted.

- Helps initialize signals quickly.

### 3. **Synchronous Reset Flip-Flop**
```verilog
always @(posedge clk) begin
    if (rst)
        q <= 0;
    else
        q <= d;
end
```
  - The output `q` is reset to `0` only on the active clock edge when `rst` is asserted.  
  - Keeps all flip-flops aligned with the clock and avoids timing issues.


### 4. **Synchronous Set Flip-Flop**
```verilog
always @(posedge clk) begin
    if (set)
        q <= 1;
    else
        q <= d;
end
```
  - The output `q` is set to `1` only on the active clock edge when `set` is asserted.  
  - Ensures predictable and controlled behavior for sequential logic.

---

## Synthesis and Simulation of Flip-FLops

### Simulation

Firstly, iverilog simulation of the flipflop is done, to verify its functionality

Locate the file in the verilog_files folder
```bash
iverilog -o ~/vcd/photos/dff_syncres.vvp dff_syncres.v tb_dff_syncres.v
```

then run the complied file
```bash
cd ~/vcd/photos           
vvp dff_syncres.vvp
```

Now, run the .vcd file through GTKWave and check its waveform
```bash
gtkwave tb_dff_syncres.vcd
```

Waveform:
![waveform](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(45).png)
### Synthesis

Synthesis is done through yosys.
```bash
yosys 
```
then, Inside yosys

```bash
read_liberty -lib ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files/dff_syncres.v
synth -top dff_syncres
dfflibmap -liberty ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
show -format png
```

### Final Netlist Dot File
![netlist](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(46).png)
## Summary
### Timing Libraries
- .lib files (e.g., sky130_fd_sc_hd__tt_025C_1v80.lib) specify timing and electrical characteristics of standard cells, enabling accurate synthesis and timing verification.
### Synthesis Methods
- Hierarchical Synthesis: Optimizes sub-modules independently, giving finer control over area and performance.
- Flat Synthesis: Processes the whole design as one block; simpler, but may lose some local optimizations.
- Flattening Option: A hierarchical design can be flattened later if required.
### Efficient Flip-Flop Coding
- Asynchronous Reset/Set:
  - Responds immediately, independent of the clock.
  - Useful for global initialization.
- Synchronous Reset/Set:
  - Updates only on the clock edge.
  - Provides predictable timing and avoids glitches.
### Workflow Overview
- Simulation: iverilog → vvp → GTKWave
- Synthesis (Yosys): Load library → read Verilog → synth → dfflibmap → abc → show

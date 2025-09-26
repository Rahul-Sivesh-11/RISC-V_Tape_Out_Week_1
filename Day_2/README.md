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
![To open the file](

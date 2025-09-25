
# RISC-V_Tape_Out_Week_1
# Day 1: Introduction to Verilog RTL Design & Synthesis
#### This document covers the complete workflow of a 2:1 Multiplexer using Verilog in Yosys and th SkyWater 130nm standard cell library.
---
### *SKY130RTL D1SK1 L1 Introduction to iverilog design and test bench*
### Simulator
- A simulator is a software tool that verifies the functionality of a digital circuit by applying test inputs and observing the outputs. This allows designers to check their design before implementing it on hardware.
- A simulator is mainly used to simulate and validate your circuit design.
- On Day 1, we will be using the open-source simulator Icarus Verilog (iverilog).
- A stimulator provides changes in the input values to test how the circuit responds.
- The output of the stimulator is typically a VCD (Value Change Dump) file, which records signal transitions during simulation.
![Stimulation flow](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/Screenshot%202025-09-24%20111612.png)
### Design
A design is the collection of Verilog codes that implement the intended functionality to satisfy specific requirements.
### Test Bench
- A test bench is created to apply stimulus to the design and verify its functionality.
- It does not contain primary inputs or outputs.
- Only the design module has primary inputs and outputs.
![Test Bench](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/Screenshot%202025-09-24%20113002.png)
### GTKWave
- It is used for viewing the waveform.

---
### SKY130RTL D1SK2 L1 Lab1 introduction to lab
For cloning the git https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git

```bash
sudo -i
sudo apt-get install git
cd /home
# for the vbox users
cd vboxuser 
cd vsd
cd VLSI
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop
cd verilog_files/
ls
```
## After  cloning the git

```bash
iverilog good_mux.v tb_good_mux.v
ls
# To dump the vcd file
./a.out
gtkwave tb_good_mux.vcd
```
To install gvim:
``` bash
apt install vim-gtk3 -y
```
If you want to change the desgin
```bash
gvim tb_good_mux.v -o good_mux.v
```

---
**Waveform**
![Wave Form](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/WhatsApp%20Image%202025-09-24%20at%2018.51.14_d6a3b090.jpg)
### Verilog Code Analysis

**The code for the multiplexer (`good_mux.v`):**

```verilog
module good_mux (input i0, input i1, input sel, output reg y);
always @ (*)
begin
    if(sel)
        y <= i1;
    else 
        y <= i0;
end
endmodule
```

###  **How It Works**

**Inputs:**
- Data Inputs: `i0`, `i1`
- Selection Line: `sel`

**Registered Output:** `y`

**Logic:**
  - If `sel`= 1, `y`=`i1`
  - If `sel`= 0, `y`=`i0`.

---
## **Introduction to Yosys**
 - Yosys is a powerful open-source synthesis tool for digital hardware. 
 - It takes your Verilog code and converts it into a gate-level netlist.
 - The set of primary inputs and outputs of the design code should be same.
 - Verification of the netlist file can be done by generating gtkwave for netlist file.
 - The same TB can be used in verification process.
![yosys](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/Screenshot%202025-09-24%20185438.png)
### What is .lib
- Collection of standard logic modules.
- It consists of all the logic gates with the variation in inputs given to it.
- It has same gates with different flavors.
- We need cells that work fast to meet the specific performance and also cells that work slow to meet HOLD.
---

## Synthesis Lab with Yosys

Let’s synthesize the `good_mux` design!

###  Step-by-Step Yosys Flow

1. **Start Yosys**
```shell
yosys
```
2. **Read the liberty library**
```shell
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
3. **Read the Verilog code**
```shell
read_verilog good_mux.v
```
4.  **Synthesis of the design**
```shell
synth -top good_mux
```
5.  **Map to Technology File**
```bash
dfflibmap -liberty ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
6. **Visualize the Netlist**
```shell
show
```
7. **Write the Gate-Level Netlist**

Copy: `good_mux_netlist.v` and paste it in `home`
```shell
write_verilog ~/good_mux_netlist.v
```
8. **Generate Report**
```bash
stat
```
---
### **Synthesis Outputs:**
#### Netlist Dot File:
![Dot File](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/WhatsApp%20Image%202025-09-24%20at%2019.00.04_0df42d2a.jpg)

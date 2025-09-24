
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


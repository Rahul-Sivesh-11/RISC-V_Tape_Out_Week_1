# Day 4: Gate-Level Simulation (GLS), Blocking vs. Non-Blocking in Verilog, and Synthesis-Simulation Mismatch

Welcome to Day 4 of the RTL Workshop!

1. **Gate-Level Simulation (GLS)**
2. **Blocking vs. Non-Blocking Assignments in Verilog**
3. **Synthesis-Simulation Mismatch**

---
## 1. Gate-Level Simulation (GLS)
### Definition
Gate-Level Simulation (GLS) is a verification method used after synthesis to check both the functionality and timing of a digital design. The synthesized netlist should behave equivalently to the RTL code, so the same testbenches can usually be applied for validation.
### Purpose of GLS
- Validate Functionality: Ensure the synthesized netlist matches RTL behavior.
- Check Timing: Use realistic delays (from SDF files) to detect setup/hold violations.
- Power Estimation: Provide more accurate power usage insights compared to RTL simulation.
- Verify DFT Structures: Confirm correct operation of scan chains and other test logic.
### Why Use GLS?
- Confirms correct translation of RTL into gate-level representation.
- Identifies timing-related issues that don’t appear in RTL simulations.
- Verifies post-synthesis test features before moving forward.
### When is GLS Run?
- After Synthesis: Once RTL is mapped to gates.
- Before Physical Design: To detect potential errors early in the flow.
### Types of GLS
- Functional GLS: Logic verification with zero or unit delays.
- Timing GLS: Delay-annotated simulation to capture real-world timing effects.
### GLS using iverilog
![GLS](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(16).png)
### Challenges in GLS
- Increased Complexity: Compared to RTL simulations, GLS involves detailed gate-level modeling, which leads to significantly longer runtimes and higher memory consumption.
- Event-Driven Slowdown: Since modern simulators only update signals on events (like clock edges or input changes), large designs may take hours to simulate.
- Debugging Difficulties: Issues such as improper initialization and X-propagation (unknown logic states) make troubleshooting more difficult.
## Synthesis–Simulation Mismatch
### Common Cause: Missing Sensitivity List
Simulators depend on activity changes, so if a sensitivity list is incomplete, the behavior during RTL simulation may differ from what is synthesized into hardware.
### Definition
A synthesis–simulation mismatch occurs when the RTL simulation (pre-synthesis) produces results that do not align with the gate-level simulation (post-synthesis) or actual hardware behavior.
### Typical Reasons
- Non-synthesizable code: Use of constructs like delays (#), initial blocks, or other unsupported features.
- Incomplete/unclear coding: Examples include missing else statements or improper sensitivity lists.
- Tool differences: Ambiguous RTL may be interpreted differently by simulators and synthesis tools.
### Best Practice
Always write synthesizable, deterministic RTL with clear coding style to minimize mismatches.
### Blocking vs. Non-blocking Assignments
 Within an always block
#### - = (Blocking assignment):
- Executes statements in the written order (sequentially).
- Each assignment completes before the next starts.
- Execution: Immediate.
- Best used for: Combinational logic always @(*).
- ```verilog
            always @(*)
                y = a & b;
           ```
#### <= (Non-blocking assignment):
- All assignments are evaluated first, then updated together at the end of the time step.
- Execution: Parallel.
- Best used for: Sequential logic (clocked always @(posedge clk)).
-  ```verilog
            always @(posedge clk)
            q <= d;
          ```
---
## Labs

### Lab 1

```bash
iverilog -o ~/vcd/a.out ternary_operator_mux.v tb_ternary_operator_mux.v
```
then,
```bash
cd ~/vcd
./a.out
gtkwave tb_ternary_operator_mux.vcd
```
![gtk wave](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(17).png)
To view the verilog file 
```bash
gedit ternary_operator_mux.v
Write down the given code
```
<pre>module ternary_operator_mux (input i0, input i1, input sel, output y);
       assign y = sel?i1:i0;
       endmodule</pre>
 #### Enter into the yosys by running it in the verilog_files directory:
```bash
cd ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files
yosys
```
Inside yosys, run
```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ternary_operator_mux.v
synth -top  ternary_operator_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![netlist dot file](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(18).png)
## GLS of Ternary Operator MUX
Gate-Level Simulation is carried out on the synthesized netlist (ternary_operator_mux_net.v). This step verifies that the multiplexer design functions correctly after synthesis, using real standard-cell implementations and accounting for delays if they are included in the model.
```bash
iverilog -o ~/vsd/a.out ../my_lib/verilog_model/primitives.v  ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
cd ~vsd
./a.out
gtkwave tb_ternary_operator_mux.vcd
```
![gtk wave](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(19).png)
## Lab 2

```bash
iverilog -o ~/vcd/a.out bad_mux.v tb_bad_mux.v
```
then,
```bash
cd ~/vcd
./a.out
gtkwave tb_bad_mux.vcd
```
![gtk wave](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(19).png)
To view the verilog file 
```bash
gedit bad_mux.v
```
![code](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(21).png)
#### Enter into the yosys by running it in the verilog_files directory:
```bash
cd ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files
yosys
```
Inside yosys, run
```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog bad_mux.v
synth -top  bad_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
#### Net List Dot File
![net dot file](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(22).png)
## GLS of Ternary Operator MUX
Gate-Level Simulation is performed using the synthesized netlist (bad_mux_net.v). This helps verify the functional correctness of the design after synthesis, using the actual standard cells and any delays (if modeled)

```bash
iverilog -o ~/vsd/a.out ../my_lib/verilog_model/primitives.v  ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v
cd ~/vsd
./a.out
gtkwave tb_bad_mux.vcd
```
![wave form](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(23).png)
### Lab 3
```bash
iverilog -o ~/vcd/a.out blocking_caveat.v tb_blocking_caveat.v
```
then,
```bash
cd ~/vcd
./a.out
gtkwave tb_blocking_caveat.vcd
```
![ wave form](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(24).png)
To view the verilog file 
```bash
gedit blocking_caveat.v
```
![code](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(24).png)
#### Enter into the yosys by running it in the verilog_files directory:
```bash
cd ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files
yosys
```
Inside yosys, run
```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog blocking_caveat.v
synth -top  blocking_caveat
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![netlist dot file](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(24).png)
## GLS of Blocking Caveat MUX
The Gate-Level Simulation is executed on the synthesized netlist (blocking_caveat_mux_net.v). This process validates the functional correctness of the multiplexer after synthesis, using the actual standard-cell library and considering delays if they are modeled.
```bash
iverilog -o ~/vsd/a.out ../my_lib/verilog_model/primitives.v  ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v
cd ~/vsd
./a.out
gtkwave tb_blocking_caveat.vcd
```
![wave](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(27).png)
## Summary
- Gate-Level Simulation (GLS): Ensures the synthesized netlist is functionally correct, meets timing requirements, and supports testability.
- Synthesis–Simulation Mismatch: Prevent mismatches by writing clear, synthesizable RTL code.
- Blocking vs. Non-blocking Assignments: Use = for combinational logic, and <= for sequential (clocked) logic.







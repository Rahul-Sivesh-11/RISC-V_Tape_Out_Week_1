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
- Best used for: Combinational logic <prev>always @(*).</prev>
#### <= (Non-blocking assignment):
- All assignments are evaluated first, then updated together at the end of the time step.
- Execution: Parallel.
- Best used for: Sequential logic (clocked always @(posedge clk)).

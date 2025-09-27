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
## GLS using iverilog


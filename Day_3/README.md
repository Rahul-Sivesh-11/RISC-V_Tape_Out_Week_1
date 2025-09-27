# Day 3: Combinational and Sequential Optimization
Welcome to the third day of our workshop! Today, we’ll explore methods for optimizing both combinational and sequential circuits, focusing on strategies that improve their efficiency and overall performance
### Objective:
- Squeeze and simplify logic to get the most optimized design in terms of area and power.

### Techniques:

- #### Constant Propagation:
   - Replaces logic blocks with constants when input values are fixed.
   - - **Example:**
    ```
    Y = ((A·B) + C)' 
    If A = 0 → Y = (0 + C)' = C'
    ```
  - **Result:**
   - Complex gate logic (6 MOS transistors) simplifies to a single inverter (2 MOS transistors).
- #### Boolean Logic Optimization:
   - Simplify Boolean expressions using: Karnaugh Map (K-Map) and Quine McCluskey Method.
   - **Example Verilog:**
    ```verilog
    assign y = a ? (b ? c : (c ? a : 0)) : (!c);
    ```
    
    - Optimized expression:  
      ```
      y = a ⊕ c
      ```
  ### Yosys Optimization Commands:
  
  - The command to perform logic optimization in Yosys is opt_clean.
    ```bash
       opt_clean
    ```
  - Additionally, for a hierarchical design involving multiple sub-modules, the design must be flattened by running the flatten command before executing the opt_clean command.
     ```bash
        flatten
     ```
     then,
    ```bash
        synth -top <module_name>
        opt_clean -purge
    ```

---
## Constant Propagation
1. Definition
This technique involves replacing signals that are permanently set to logic values (0 or 1) with those constant values directly. By doing so, redundant gates and interconnections are removed.
2. Explanation
Constant propagation identifies nets or signals that always resolve to a fixed logic level and restructures the circuit based on that fact. For instance, if one input of a gate is permanently tied to 0, the output can be directly simplified to 0, eliminating the need for that gate.
### Benefits
Minimizes the number of gates used
Produces simpler logic expressions
Enhances efficiency in terms of chip area and power consumption
## State Optimization
1. Definition
This method streamlines a finite state machine (FSM) by minimizing its state set, typically through the elimination of unreachable states or the merging of states that exhibit identical behavior.
2. Explanation
Within an FSM, certain states may never be entered, while others may function equivalently to one another. State optimization detects such redundancies and simplifies the machine into a reduced form without changing its functionality.
### Benefits
- Decreases the number of flip-flops required
- Enhances performance by reducing unnecessary transitions
- Saves chip area and lowers power consumption
## Cloning
1. Definition
Cloning is the process of duplicating registers or logic elements to handle excessive fanout and enhance timing performance.
2. Explanation
When one flip-flop or gate is connected to a large number of loads, the resulting fanout can slow down signal propagation. By creating additional copies of the logic or register, the loads are distributed across them, reducing delay.
### Benefits
- Minimizes delay caused by heavy fanout
- Enhances timing convergence
- Supports operation at higher clock frequencies
## Retiming
1. Definition
Retiming is the technique of relocating flip-flops within a circuit while maintaining the original functionality.
2. Explanation
The goal of retiming is to distribute combinational delays more evenly by shifting registers across logic elements. This adjustment ensures that setup and hold constraints are satisfied, while the circuit’s input-output behavior remains unaffected.
### Benefits
- Reduces critical path delay
- Supports faster clock operation
- Achieves balanced pipeline stages for improved efficiency
## Sequential Logic Optimization
### Introduction
Sequential logic optimization focuses on improving circuits that contain flip-flops and memory elements, making them more efficient in terms of speed, area, and power.
### Foundational Technique
#### Sequential Constant Propagation: Identifies and propagates constant values through flip-flops during synthesis, eliminating unnecessary logic.
### Advanced Techniques
- State Minimization
Shrinks the number of states in a finite state machine (FSM) by removing redundant or unreachable ones, thereby reducing area and simplifying transitions.
- Retiming
Relocates registers across combinational logic to balance delays, shorten critical paths, and enhance timing performance.
- Sequential Cloning
Creates duplicates of registers or logic elements in a floorplan-aware manner, helping achieve timing closure and reducing congestion.
### Result
Together, these methods yield circuits that are faster, occupy less area, and consume lower power—making them well-suited for practical chip implementations.
### **To get start through the optimization of design logic**

**Go through the step by step worflow**

#### Locate the file in your project directory. 

```bash
cd ~/VSD/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
#### List the optimization designs
 ```bash
 ls *opt*
```

#### list the sequential optimization designs
 ```bash
 ls *dff*
```
---

# Combinational Logic Optimization labs

### Lab 1 :

To see the verilog logic of Lab1.
```bash
gedit opt_check.v
```
![Verilog code](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(2).png)
#### Explanation:

- assign y = a ? b : 0; means:
- If a is true, y is assigned the value of b.
- If a is false, y is 0.

#### Lanch yosys:
```bash
yosys
```

#### 6. **Run this command inside yosys:**
```bash
read_liberty -lib ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check.v
synth -top opt_check
opt_clean -purge
abc -liberty ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```



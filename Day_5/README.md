# Day 5: Optimization in Synthesis
Welcome to Day 5 of the RTL workshop! In this session, we’ll dive into Verilog synthesis optimization, exploring constructs such as if–else statements, for-loops, and generate blocks. We’ll also examine how poor coding practices may result in unintended latches. The day includes lab sessions for practical, hands-on learning.
1. If–Else Statements in Verilog
- Used for conditional execution in behavioral modeling, typically inside procedural blocks (always, initial, tasks, or functions).
- Commonly applied to describe priority logic.
2. Inferred Latches
- A latch is inferred when an if statement is incomplete (i.e., missing an else or default assignment).
-In such cases, the output retains its previous value whenever the condition is not satisfied—effectively creating latch behavior.
- While unintended latches are usually a coding mistake, there are situations where they are deliberate.
- Example: A counter implemented with an incomplete if may be intentional, and its behavior is not negatively impacted by the missing else.
```verilog
             module counter (
                   input clk, 
                   input rst, 
                   input enable,
                   output reg [2:0] count
              );

            always @(posedge clk or posedge rst) begin
            if (rst)
             count <= 3'd0;
           else if (enable)
              count <= count + 1;
           end

           endmodule
      ```
### Syntax

```verilog
if (condition) begin
    // Code for true Condition
end else begin
    // Code for false Condition
end
```
### Nested If-Else

```verilog
if (condition1) begin
    // Code for condition1 true
end else if (condition2) begin
    // Code for condition2 true
end else begin
    // Code if all Conditions are false
end
```

---

### Case Statement 
The `if-else` and `case` statement should be in the `always` block and the variable declared in inside it should be `reg <variable>` .

#### Caveats with case statement
- Due to the incomplete `case` statement, it will become inferred latch .
- Due to this, the incomplete case statements is latched with the output.
- To avoid this, code `case` with default case statement.

```verilog
case (expression)
    value1 : begin
                 // statements for value1
             end
    value2 : begin
                 // statements for value2
             end
    ...
    default : begin
                 // statements if none of the above match
              end
endcase
```
#### Solution: Add Else or Default Case
```verilog
module ex (
    input wire a, b, sel,
    output reg y
);
    always @(a, b, sel) begin
        case(sel)
            1'b1 : y = a;
            default : y = 1'b0; // Default assignment
        endcase
    end
endmodule
```
---
## 3. Labs for If-Else and Case Statements
### Lab 1: Incomplete If Statement
The incomplete if verilog code is 
<pre>module incomp_if input i0 , input i1, input i2 , outputreg y);
always @ (*)
begin
      ifi0)
            y <= i1;
end
endmodule</pre>
The gtkwave is:
![gtk wave](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(28).png)
The Netlist Dot File:
![netlist](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(29).png)
### Lab 2: Nested If-Else
The incomplete nested if-else verilog code is
![net list](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(30).png)
gtkwave is:
![gtk wave](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(31).png)
Netlist Dot File is:
![netlist](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(32).png)
### Lab 3: Complete Case Statement
The complete case statement verilog code of comp_case is
![code](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(33).png)
gtkwave is:
![gtk wave](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(34).png)
Netlist Dot File is:
![netlist](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(35).png)
### Lab 4: Incomplete Case Handling
The bad_case verilog code is
![code](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(36).png)
gtkwave is:
![gtk wave](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(37).png)
Netlist Dot File is:
![netlist dotfile](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(38).png)
## 4. For Loops in Verilog
- Executes a group of statements multiple times for a defined count within an always block.
- Commonly applied for iterative operations in both combinational and sequential logic.
- A for loop can only be written inside procedural blocks.
### Syntax
**Syntax:**
```verilog
for (initialization; condition; increment) begin
    // Statements to execute
end
```
### 2. `generate for` Loop

- Used at **compile/synthesis time** to generate multiple instances of modules or repeated RTL structures.
- Creates hardware replication.

**Syntax:**
```verilog
genvar i;  // loop variable must be declared as genvar

generate
    for (i = 0; i < N; i = i + 1) begin : block_name
        // Code to be generated
    end
endgenerate

```
## Labs on `for` loop

## `mux_generate.v`

**Verilog Code**

#### Example: 4-to-1 MUX Using a For Loop

```verilog
module mux_4to1_for_loop (
    input wire [3:0] data, // 4 input lines
    input wire [1:0] sel,  // 2-bit select
    output reg y           // Output
);
    integer i;
    always @(data, sel) begin
        y = 1'b0; // Default output
        for (i = 0; i < 4; i = i + 1) begin
            if (i == sel)
                y = data[i];
        end
    end
endmodule
```
![verilog code](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(47).png)
#### GTK wave form
![wave form](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(48).png)
#### Netlist Dot File
![netlis](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(49).png)
## `demux_generate.v`

**Verilog Code**

Verilog Code:
![verilog code](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(50).png)
#### GTK wave Form
![wave](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(51).png)
#### Netlist Dot File
![netlist](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(52).png)
## Ripple Carry Adder
- Bitwise Addition: Each stage of the RCA consists of a full adder that takes two input bits (A and B) along with a carry-in.
- First Stage: The least significant bits (LSBs) of A and B are added in the first full adder along with the initial carry-in (usually 0).
- Carry Propagation: The carry-out from the first full adder becomes the carry-in for the next stage.
- Sequential Processing: This process repeats for each higher-order bit, with carries propagating (rippling) through the chain of adders.
- Final Carry: The last full adder generates the most significant sum bit along with a final carry-out, which may indicate an overflow.
- Operation Speed: Since each full adder must wait for the carry from the previous stage, the addition is completed only after the carry ripples through all adders.
![ripple carry adder](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(39).png)
**Verilog Code**

RCA (`rca.v`): _8 bit Ripple Carry Adder_

```verilog
module rca (input [7:0] num1 , input [7:0] num2 , output [8:0] sum);
wire [7:0] int_sum;
wire [7:0]int_co;

genvar i;
generate
        for (i = 1 ; i < 8; i=i+1) begin
                fa u_fa_1 (.a(num1[i]),.b(num2[i]),.c(int_co[i-1]),.co(int_co[i]),.sum(int_sum[i]));
        end

endgenerate
fa u_fa_0 (.a(num1[0]),.b(num2[0]),.c(1'b0),.co(int_co[0]),.sum(int_sum[0]));


assign sum[7:0] = int_sum;
assign sum[8] = int_co[7];
endmodule
```

FA (`fa.v`): _1 bit Full Adder_

```verilog 
module fa (input a , input b , input c, output co , output sum);
        assign {co,sum}  = a + b + c ;
endmodule
```
## 7. Labs on Loops and Generate Blocks

### Lab 9: 4-to-1 MUX Using For Loop

```verilog
module mux_generate (
    input i0, input i1, input i2, input i3,
    input [1:0] sel,
    output reg y
);
wire [3:0] i_int;
assign i_int = {i3, i2, i1, i0};
integer k;
always @(*) begin
    for (k = 0; k < 4; k = k + 1) begin
        if (k == sel)
            y = i_int[k];
    end
end
endmodule
```
![gtk wave](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(40).png)
---

### Lab 10: 8-to-1 Demux Using Case

```verilog
module demux_case (
    output o0, output o1, output o2, output o3,
    output o4, output o5, output o6, output o7,
    input [2:0] sel,
    input i
);
reg [7:0] y_int;
assign {o7, o6, o5, o4, o3, o2, o1, o0} = y_int;
always @(*) begin
    y_int = 8'b0;
    case(sel)
        3'b000 : y_int[0] = i;
        3'b001 : y_int[1] = i;
        3'b010 : y_int[2] = i;
        3'b011 : y_int[3] = i;
        3'b100 : y_int[4] = i;
        3'b101 : y_int[5] = i;
        3'b110 : y_int[6] = i;
        3'b111 : y_int[7] = i;
    endcase
end
endmodule
```
![gtk wave](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(41).png)
### Lab 11: 8-to-1 Demux Using For Loop

```verilog
module demux_generate (
    output o0, output o1, output o2, output o3,
    output o4, output o5, output o6, output o7,
    input [2:0] sel,
    input i
);
reg [7:0] y_int;
assign {o7, o6, o5, o4, o3, o2, o1, o0} = y_int;
integer k;
always @(*) begin
    y_int = 8'b0;
    for (k = 0; k < 8; k = k + 1) begin
        if (k == sel)
            y_int[k] = i;
    end
end
endmodule
```
![gtk wave](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(42).png)
### Lab 12: 8-bit Ripple Carry Adder with Generate Block

```verilog
module rca (
    input [7:0] num1,
    input [7:0] num2,
    output [8:0] sum
);
wire [7:0] int_sum;
wire [7:0] int_co;

genvar i;
generate
    for (i = 1; i < 8; i = i + 1) begin
        fa u_fa_1 (.a(num1[i]), .b(num2[i]), .c(int_co[i-1]), .co(int_co[i]), .sum(int_sum[i]));
    end
endgenerate

fa u_fa_0 (.a(num1[0]), .b(num2[0]), .c(1'b0), .co(int_co[0]), .sum(int_sum[0]));

assign sum[7:0] = int_sum;
assign sum[8] = int_co[7];
endmodule
```
#### Waveform
![Waveform](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(53).png)
#### Netlist Dot File
![Netlist](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(55).png)
**Full Adder Module:**
```verilog
module fa (input a, input b, input c, output co, output sum);
    assign {co, sum} = a + b + c;
endmodule
```
![gtk wave](https://github.com/Rahul-Sivesh-11/RISC-V_Tape_Out_Week_1/blob/main/Images/2025-09-27%20(43).png)
---
## Summary
- Write complete if–else and case statements to prevent accidental latch inference.
- Leverage for-loops and generate blocks to create scalable and synthesizable designs.
- Ensure that all signals are assigned on every path in combinational logic to avoid unintended storage elements.
- Reinforce learning through lab exercises, applying concepts with real Verilog code and synthesis analysis.
---

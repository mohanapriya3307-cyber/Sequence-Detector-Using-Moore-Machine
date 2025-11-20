# Design and Simulation of Sequence Detector Using Moore Machine in Verilog HDL

---

## **Aim**
To design, simulate, and verify a **Sequence Detector** using the **Moore Finite State Machine (FSM)** model in Verilog HDL.

---

## **Apparatus Required**
- System with **Vivado Design Suite**
---

## **Theory**

A **sequence detector** is a type of **finite state machine (FSM)** that detects the occurrence of a specific sequence of bits in a given input stream.

### **Moore Machine Concept**
In a **Moore Machine**, the **output depends only on the present state** and **not on the current input**.  
The output changes only when the state changes.

---

### **Example Sequence: 1011**
The sequence detector should produce an output `Z = 1` whenever the input sequence **“1011”** is detected in the serial input stream.

#### **State Description**
| State | Description | Output (Z) |
|:------:|:-------------|:-----------:|
| S0 | Initial State / No bits matched | 0 |
| S1 | ‘1’ detected | 0 |
| S2 | ‘10’ detected | 0 |
| S3 | ‘101’ detected | 0 |
| S4 | ‘1011’ detected | 1 |

#### **State Transition Diagram**
<img width="403" height="557" alt="image" src="https://github.com/user-attachments/assets/9b24604c-69d2-412f-a302-2c23aed79038" />

#### **State Transition Table**
| Current State | Input (X) | Next State | Output (Z) |
|:---------------|:----------|:------------|:------------|
| S0 | 0 | S0 | 0 |
| S0 | 1 | S1 | 0 |
| S1 | 0 | S2 | 0 |
| S1 | 1 | S1 | 0 |
| S2 | 0 | S0 | 0 |
| S2 | 1 | S3 | 0 |
| S3 | 0 | S2 | 0 |
| S3 | 1 | S4 | 1 |
| S4 | 0 | S2 | 0 |
| S4 | 1 | S1 | 0 |

---

## **Program**

```verilog
`timescale 1ns/1ps
module moree(clk, rst, in, out);
input clk;
input rst;
input in;
output reg out;
parameter S0=3'b000,
          S1=3'b001,
          S2=3'b010,
          S3=3'b011,
          S4=3'b100;
reg [2:0] cs, ns;
always @(posedge clk or posedge rst)
begin 
if(rst)
cs=S0;
else
cs=ns;
end 
always @ (*) begin 
case(cs)
S0:if(in)
ns=S1;
else ns=S0;
S1:
begin
if (in)
ns=S1;
else ns=S2;
end
S2:
begin 
if(in)
ns=S3;
else 
ns=S0;
end 
S3: 
begin 
if(in)
ns=S4;
else
ns=S2;
end
S4:
begin
if(in)
ns=S1;
else
ns=S0;
end
default: ns=S0;
endcase
end
always @(*)
begin
case(cs)
S4: out = 1'b1;
default: out=1'b0;
endcase
end
endmodule
```
### Testbench
```
module tb_moree;
reg clk, rst, in;
wire out;
moree uut(clk, rst, in ,out);
initial begin
clk=0;
forever #5 clk=~clk;
end
initial
begin rst=1;
in =0;
#10 rst=0;
in=1;#10
in=0;#10
in=1;#10
in=1;#10
#10 rst=0;
#20

in=1;#10
in=0;#10
in=1;#10
in=0;#10
#10 $finish;
end 
endmodule
```
### Simulation Output

<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/e617252d-21cc-4d46-b2fc-5dd7180bf65e" />


### Result

The Sequence Detector using Moore Machine was successfully designed and simulated in Vivado Design Suite using Verilog HDL.
The simulation confirmed that the output Z is asserted high only when the input sequence “1011” is detected.

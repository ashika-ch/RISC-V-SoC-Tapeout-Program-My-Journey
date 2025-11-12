# RISC-V-SoC-Tapeout-Program-My-Journey
👩‍💻 Participant: Ashika C H 
<br>
📍 Program: RISC-V Reference SoC Tapeout Program (VSD) 
<br>
🏭 Impact: Part of India’s largest collaborative open-source tapeout with 3500+ participants .
<br>
This repository tracks my week-by-week progress in the SoC Tapeout Program, covering everything from RTL design to GDSII.

<details>
	<summary>Week 0 - Tools Installation </summary>

# Week0 - Tools Installation

## Yosys
```
$ git clone https://github.com/YosysHQ/yosys.git
$ cd yosys 
$ sudo apt install make (If make is not installed please install it) 
$ sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
$ make 
$ sudo make install
```
<img width="777" height="496" alt="Image" src="https://github.com/user-attachments/assets/07c16b28-8c3c-42fd-8915-6742bbfdb04c">

## Iverilog
```
$ sudo apt-get install iverilog
```
<img width="750" height="280" alt="Image" src="https://github.com/user-attachments/assets/73934186-9120-4efd-ab9c-830e09850606">

## GTKWave
```
$ sudo apt update
$ sudo apt install gtkwave
```
<img width="1109" height="725" alt="Image" src="https://github.com/user-attachments/assets/bc857c53-b062-4c9d-a008-a3753eae8eec">

## NgspiceM
```
After downloading the tarball from https://sourceforge.net/projects/ngspice/files/ to a local
directory, unpack it using:
$ tar -zxvf ngspice-37.tar.gz
$ cd ngspice-37
$ mkdir release
$ cd release
$ ../configure --with-x --with-readline=yes --disable-debug
$ make
$ sudo make install 
```
<img width="777" height="496" alt="Image" src="https://github.com/user-attachments/assets/649a46f0-8d79-40cd-a2db-0dfde9474467">

## Magic
```
$ sudo apt-get install m4
$ sudo apt-get install tcsh
$ sudo apt-get install csh
$ sudo apt-get install libx11-dev
$ sudo apt-get install tcl-dev tk-dev
$ sudo apt-get install libcairo2-dev
$ sudo apt-get install mesa-common-dev libglu1-mesa-dev
$ sudo apt-get install libncurses-dev
git clone https://github.com/RTimothyEdwards/magic
cd magic
./configure
make
make install 
```
<img width="984" height="679" alt="Image" src="https://github.com/user-attachments/assets/4e0ae6a6-7eec-46ad-9bfc-de1c4ed42edd">

🛠️ Week 0 — Setup & Tools

🚀 Foundation Week — setting up the environment to begin the RISC-V SoC Tapeout journey.

🎯 Objectives

✔️ Understand program scope & flow (RTL → Synthesis → PD → Tapeout)
✔️ Install & configure open-source EDA tools
✔️ Validate environment with test runs

🧰 Tools Installed

📝 Yosys → Logic Synthesis
🎨 Magic → Layout & DRC/LVS checks
📊 KLayout → GDSII Visualization
📡GTKWave → Simulation & waveform analysis

🔑 Key Learnings

🌐 Explored the open-source EDA ecosystem
🧩 Understood how tools connect in the SoC flow
🛠️ Completed first test synthesis & layout runs successfully
🔗 Realized the importance of environment setup as the backbone for the entire tapeout process

✅ Week 0 Status
🟢 Setup Complete → Ready to begin RTL design in Week 1

✨ “Week 0 laid the foundation — from here, every week builds one more layer towards tapeout.”
</details>


<details>
<summary> Week 1 - Introduction to Verilog RTL Design and Synthesis</summary>

## Introduction to open-source simulator Iverilog

Folder structure of the git clone:
- `lib` - will contain sky130 standard cell library
- `my_lib/verilog_models` - will contain standard cell verilog model
- `verilog_files` -contains the lab experiments source files

<img width="1524" height="628" alt="Image" src="https://github.com/user-attachments/assets/294c6d61-5c58-4261-ad7a-4536e2a954d0">
Example of a design good_mux.v 

```
module good_mux (input i0 , input i1 , input sel , output reg y);
always @ (*)
begin
	if(sel)
		y <= i1;
	else 
		y <= i0;
end
endmodule
```
Example of a testbench tb_good_mux.v 

```
`timescale 1ns / 1ps
module tb_good_mux;
	// Inputs
	reg i0,i1,sel;
	// Outputs
	wire y;

        // Instantiate the Unit Under Test (UUT)
	good_mux uut (
		.sel(sel),
		.i0(i0),
		.i1(i1),
		.y(y)
	);

	initial begin
	$dumpfile("tb_good_mux.vcd");
	$dumpvars(0,tb_good_mux);
	// Initialize Inputs
	sel = 0;
	i0 = 0;
	i1 = 0;
	#300 $finish;
	end

always #75 sel = ~sel;
always #10 i0 = ~i0;
always #55 i1 = ~i1;
endmodule
```
Command to run the design and testbench
```
iverilog good_mux.v tb_good_mux.v
```
The output of the iverilog is a .vcd file and a.out file is created. By executing a.out iverilog dump the vcd file.

## Introduction to GTKWave
gtkwave will be used to generate the waveforms and display in visual format.

Command to view the vcd file in gtkwave 
```
gtkwave tb_good_mux.vcd
```
The waveform in gtwave is shown below

<img width="1985" height="1257" alt="Image" src="https://github.com/user-attachments/assets/a28be5b2-fae6-4df6-9b82-cf4115826627">


## Introduction to Yosys
It is the synthesizer used to convert RTL to netlist.
Netlist should be the same as the Design but represented in the form of standard cells.
The same testbench can be used to verify RTL and Synthesized Netlist.

<img width="1600" height="850" alt="Image" src="https://github.com/user-attachments/assets/4ea5f0b3-21ba-4f7b-b4d0-8e77fc97789c">

## Introduction to Logic Synthesis

<img width="1222" height="857" alt="Image" src="https://github.com/user-attachments/assets/f769ab82-6e6e-49eb-b21e-04d46ec98cdf">

## Lab using Yosys and Sky130 PDKs

<img width="1048" height="486" alt="Image" src="https://github.com/user-attachments/assets/b6ba5a11-672c-4cfd-b042-83d9cfb11677">
<img width="1048" height="486" alt="Image" src="https://github.com/user-attachments/assets/9e73d8e7-7a8e-4a81-9edc-91ea75923349">
<img width="1048" height="486" alt="Image" src="https://github.com/user-attachments/assets/966651d6-59fc-46e7-9d9f-bcd375b07884">

Timing libs, Hierarchical vs Flat Synthesis and Efficient Flop Coding Styles

## Introduction to timing .libs
Libraries are characterized based on PVT (process, voltage, temperature) \
Process -> Variations due to fabrication \
Voltage -> Variations due to voltage \
Temperature -> Variations due to temperature 

As seen in the screenshot below \
tt stands for typical in the .lib name \
025C stands for temperature of 25 C in the .lib name \
1v80 stands for voltage of 1.8V in the .lib name

<img width="1146" height="769" alt="Image" src="https://github.com/user-attachments/assets/d6833e15-8306-4cfc-a2b6-41e3c58f9f58">

-cell defines the beginning of the cell. Other information of cells mentioned are:
- Leakage power based on the combination of inputs
- Area
- Power ports
- Input capacitance
- Power associated with the pin
- Transition
- Delay

## Hierarchical vs Flat Synthesis

### Hierarchical Synthesis
Report after synthesizing multiple_modules.v. As shown below the sub_modules statistics are printed. For example, sub-module1 has 1 AND gate and sub-module2 has 1 OR gate. This is an example of Hierarchical Synthesis.

<img width="1048" height="486" alt="Image" src="https://github.com/user-attachments/assets/390d393a-2f92-4ed4-9c6f-76e6cee05fa1">

Hierarchy is preserved. sub_module1 and sub_module2 are instantiated separately in the synthesized Verilog netlist. Rather than seeing AND or OR gate, we see sub_modules when we run the command 'show' as shown in the screenshot.

<img width="1146" height="769" alt="Image" src="https://github.com/user-attachments/assets/a4efa6b5-41bc-4d08-b2ca-10e65e96f3b6">
<img width="1141" height="758" alt="Image" src="https://github.com/user-attachments/assets/71ba174f-a14b-4c2c-8732-6aef2e04a2c6">

If we look into the sub_module2 in synthesized netlist 'multiple_modules_hier.v', we see that rather than OR gate, the inputs a & b, pass through the inverter and then NAND gate. It is because in CMOS, stacking PMOS, which happens in 'OR' gate is bad as PMOS has lower mobility and always have to be wider to get some meaningful output. The next step is to check .lib file for the answer.

### Flat Synthesis
The design can be flattened by using the command `flatten`.

Screenshot shows the command, synthesized netlist and the logical diagram.

<img width="1005" height="617" alt="Image" src="https://github.com/user-attachments/assets/40a4dc6b-ffa8-4706-a88a-d4c77f19dc66" >

<img width="1146" height="769" alt="Image" src="https://github.com/user-attachments/assets/7ff002c5-63c6-4d49-9279-94a38928c2f7">

<img width="1162" height="192" alt="Image" src="https://github.com/user-attachments/assets/3ff5fc5e-cb69-4888-9399-689bcc8c26f7">

### Sub-module Level Synthesis
RTL (Register Transfer Level) designs are often modular, with various functional blocks or sub-modules. Sub-module level synthesis allows each of these sub-modules to be synthesized independently.

Why is the sub-module level synthesis necessary?
- Optimization and Area Reduction: By synthesizing sub-modules separately, the synthesis tool can optimize each one individually. It performs logic optimization, technology mapping, and area minimization for each sub-module. This leads to more efficient use of resources and reduced overall chip area.
- Resuability: Each submodule can be designed, verified, and optimized independently. They can be reused in a large design multiple times saving time and enhancing efficiency. 
- Parallel Processing: Different sub-modules can be synthesized concurrently, improving efficiency. For large designs, parallel synthesis significantly reduces turnaround time.

The commands to run sub-module synthesis
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top sub_module1
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

The screenshot shows that when sub_module1 is synthesized, only AND gate is generated. 

<img width="849" height="727" alt="Image" src="https://github.com/user-attachments/assets/9a35bb30-2e63-427c-b762-68f681db08aa">
<img width="849" height="727" alt="Image" src="https://github.com/user-attachments/assets/98c6b887-97b3-45e6-8385-2895bc9524fd">

## Various Flop Coding Styles and Optimization

### Why do we need flops and how do they prevent glitches in the circuit?

Glitches can occur in digital circuits due to various reasons such as signal delays, noise, or timing issues. Flops prevent glitches during the operation in the following ways:
- Synchronization: Flops are edge-triggered devices, meaning they respond only to transitions of the input signal (e.g., rising edge, falling edge). This synchronization ensures that the output changes only at specific points, reducing the likelihood of glitches caused by transient signal variations.
- Timing Control: Flops are typically controlled by a clock signal, ensuring that all circuit operations occur synchronously. This eliminates timing issues that could lead to glitches due to data arriving at different times.

<img width="1691" height="1734" alt="Image" src="https://github.com/user-attachments/assets/d27e0c71-3322-4794-8115-82ce925e7df1">

### Different types of flops
To initialize flops, we need to `set` and `reset` which can be synchronous or asynchronous.

<img width="1550" height="846" alt="Image" src="https://github.com/user-attachments/assets/71765cf3-abbc-4f83-ab0c-39f91f0df5a6">

![Image](https://github.com/user-attachments/assets/fb39354b-992c-4a05-81f9-15caf0d2531a)

The screenshot below shows DFF with asynchronous reset HDL simulation in Iverilog and  waveform display in GTKwave. Irrespective of the clock and d, as soon as async_reset=1, q=0.

![Image](https://github.com/user-attachments/assets/ae2bcbc9-0b3f-4c80-b001-5240bc0a55a2)

### Synthesizing flops
The command to synthesize ***DFF with asynchronous reset*** as an example
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres.v
synth -top dff_asyncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
![Image](https://github.com/user-attachments/assets/e8701b87-e90d-47fd-a75d-4a774944d206)

On synthesizing ***DFF with synchronous reset*** we get NOR gate with inverted `d` as shown in the screenshot below. However,on evaluating the boolean expression, we reached the same logic realization. 

![Image](https://github.com/user-attachments/assets/8e635101-9c79-4bfd-bf95-64f1a86e448a)


Using the `stat` command, all the cells used for logic synthesis are visible even though it is not evident from the statistics of doing synthesis.

<img width="1144" height="1230" alt="Image" src="https://github.com/user-attachments/assets/71fa1b2e-481f-4262-a7a2-63e1ef4ce2c1">

### Synthesizing mult2 (multiply by 2)

To implement `y[3:0] = 2*a[2:0]`, we append a `1'b0 `to the `a[2:0]` i.e, `y[3:0] = {a[2:0],0}`. This is also equal to left shift the input bits by 1.
This can be realized by just wiring.
So we expect no hardware which is also seen in the screenshot below, analysis after synthesis and show. The command 'abc' is not required for mapping when there are no cells.

![Image](https://github.com/user-attachments/assets/62f737a5-d327-459d-bf84-5147415a3861)

### Synthesizing mult9 (multiply by 9 or 8+1)

`y=9*a` can be considered `8*a+1*a`
To implement `y[5:0] = 9*a[2:0]`, we append `000` to `a[2:0]` and then add `a` i.e, `y[5:0] = {a[2:0],000} + a[2:0]`.
This can be realized just by wiring.
So we expect no hardware which is also seen in the screenshot below, analysis after synthesis and show. The command 'abc' is not required for mapping when there are no cells.

![WhatsApp Image 2025-09-27 at 20 38 22_650bcc78](https://github.com/user-attachments/assets/1a0feaa2-d6e9-4d4b-9336-5b67a719625b)

#### Combinational and Sequential Optimizations

## Introduction to Optimizations

### Combinational Logic Optimization
It means squeezing the logic to get the most optimized design in terms of area and power. the most commonly used techniques are:
1) Constant propagation using direct optimization
2) Boolean logic optimization using K-map and Quine McKlusky

An example of constant propagation optimization is highlighted below.

<img width="1235" height="647" alt="Image" src="https://github.com/user-attachments/assets/c84db356-d296-4ced-8553-6d8ce0d455c7">

An example of boolean optimization is highlighted below.

<img width="1237" height="599" alt="Image" src="https://github.com/user-attachments/assets/7a85c3cb-1638-4161-9873-e61306b5bdf8">

### Sequential Logic Optimization
The technqiues used are:
1) Basic
   - Sequential constant propagation
2) Advanced (not covered as part of lab)
   - Static optimization
   - Retiming
   - Sequential logic cloning (floorplan aware synthesis)

An example of sequential constant propagation is highlighted below of DFF with asynchronous reset where D input is grounded. To note, the same technique cannot be applied to DFF with the asynchronous set because while `Q=1` when `Set=1`, but `Q=0` at `Set=0` at the next CLK pulse. Q is dependent not only on Set but also on the clock edge.

<img width="1258" height="696" alt="Image" src="https://github.com/user-attachments/assets/72ad10d4-b057-456a-863e-55c39bd1c264">

Retiming is a technique to improve the performance of the circuit.

<img width="1199" height="673" alt="Image" src="https://github.com/user-attachments/assets/68b2c1f9-bb34-471a-ac83-787bfdc73f5a">

## Combinational Logic Optimizations
Commands for optimization

```
opt_clean -purge
```
### Optimization of opt_check.v
Syntax for opt_check.v
```
module opt_check (input a , input b , output y);
        assign y = a?b:0;
endmodule
```
For opt_check.v the assignment `y = a?b:0` reduces to `y = ab`. The screenshot shown below explains this

<img width="1066" height="608" alt="Image" src="https://github.com/user-attachments/assets/086aaed8-2245-4ee6-9cc4-204b8d964060">

The logic implementation after synthesis for opt_check.v is shown below, showing only AND gate.

![Image](https://github.com/user-attachments/assets/4fe94d63-2bd0-4821-b17a-91c77623ea64)

### Optimization of opt_check2.v
Syntax for opt_check2.v
```
module opt_check2 (input a , input b , output y);
        assign y = a?1:b;
endmodule
```
For opt_check2.v the assignment `y = a?1:b` reduces to `y = a + b`. 

The logic implementation after synthesis for opt_check2.v is shown below, showing only OR gate.

![Image](https://github.com/user-attachments/assets/dbf9f02e-b613-42b4-b0c7-07c154fbfeb7)

### Optimization of opt_check3.v
Syntax for opt_check3.v
```
module opt_check3 (input a , input b, input c , output y);
	assign y = a?(c?b:0):0;
endmodule
```
For opt_check.v the assignment `y = a?(c?b:0):0` reduces to `y = abc`. The screenshot shown below explains this.

<img width="1082" height="572" alt="Image" src="https://github.com/user-attachments/assets/fffac397-2045-4cc1-892d-fab71287df52">

The logic implementation after synthesis for opt_check3.v is shown below, showing 3 input AND gate.

![Image](https://github.com/user-attachments/assets/80c50d14-abac-42c2-aafe-61d9b7522ee3)

### Optimization of multiple_module_opt.v

Syntax of multiple_module_opt.v
```
module sub_module1(input a , input b , output y);
 assign y = a & b;
endmodule

module sub_module2(input a , input b , output y);
 assign y = a^b;
endmodule

module multiple_module_opt(input a , input b , input c , input d , output y);
wire n1,n2,n3;

sub_module1 U1 (.a(a) , .b(1'b1) , .y(n1));
sub_module2 U2 (.a(n1), .b(1'b0) , .y(n2));
sub_module2 U3 (.a(b), .b(d) , .y(n3));

assign y = c | (b & n1); 
endmodule
```
The logic implementation after synthesis for multiple_module_opt.v is shown below.

<img width="2159" height="538" alt="Image" src="https://github.com/user-attachments/assets/434b689b-0db2-40aa-a755-2a4db02a4e0a">

## Sequential Logic Optimizations

Both the dff_const1.v and dff_const2 are explained below.

<img width="1701" height="829" alt="Image" src="https://github.com/user-attachments/assets/9de2a578-b6a1-471a-99d2-db22dfcb0143">

### Optimizing dff_const1.v

Syntax for dff_const1.v
```
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end

endmodule
```
For dff_const1.v, `q=0` as long as `reset=1`. However, when `reset=0` `q` doesn't immediately becomes `1` rather at the next rising edge of the `clk` as shown below. ***So the optimization cannot be applied***.

<img width="1995" height="777" alt="Image" src="https://github.com/user-attachments/assets/51d35ab5-10e6-4742-ab9e-27692d927833">

The commands to run the synthesis
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const1.v
synth -top dff_const1
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

The logic implementation after synthesis for dff_const1.v is shown below.

![Image](https://github.com/user-attachments/assets/63627989-6350-4bb7-9f1a-a45b6e83882c)

***complete dff_const2,4,5***

### Optimizing dff_const3.v

Syntax for dff_const3.v
```
module dff_const3(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b0;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end

endmodule
```
For dff_const3.v, there are two flops.  `q1=0` as long as `reset=1`. However, when `reset=0` `q1` doesn't immediately becomes `1` rather at the next rising edge of the `clk` with some propagation delay as shown below. `q=1` as long as `reset=1`, acting as `set` rather than `reset`. However, when `reset=0` `q` samples `q1` as `0` as there are some propagation delay for `q1`as shown below. At the next `clk` edge `q` samples `q1` as `1`.
***So the optimization cannot be applied***.

<img width="1945" height="1137" alt="Image" src="https://github.com/user-attachments/assets/03ebf862-ab50-4595-b239-4a60b8937ac2">

The command to run HDL simulation
```
iverilog dff_const3.v tb_dff_const3.v
./a.out
gtkwave tb_dff_const3.vcd
```
The HDL simulation is shown below.

image 

The commands to run the synthesis
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const3.v
synth -top dff_const3
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

The logic implementation after synthesis for dff_const3.v is shown below.

<img width="2132" height="313" alt="Image" src="https://github.com/user-attachments/assets/adacaeab-038d-4bbb-a03c-de34198882ea">

## Sequential Optimzations for Unused Outputs

### Optimization of Case1: 3-bit Up Counter with q[0] used (counter_opt.v)
Example of a counter where bits at the position of [2] and [1] are unused.

```
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = count[0];

always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end

endmodule
```
The screenshot explains the logic of the counter. Only q[0] is used. ***So the optimization can be applied***.

<img width="2400" height="1011" alt="Image" src="https://github.com/user-attachments/assets/98d8fc75-4a15-4204-82c8-c2c0c20c0351">

The commands to run the synthesis
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog counter_opt.v
synth -top counter_opt
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
We see only one flop after the synthesis and also seen in synthesis report after `synth -top counter_opt.v`

![Image](https://github.com/user-attachments/assets/c65fd5da-30c8-4dfd-8333-776c28b3e370)

### Optimization of Case2: 3-bit Up Counter (counter_opt2.v)

Example of a counter where all the bits are used.
```
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = (count[2:0] == 3'b100);

always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end

endmodule
```
The commands to run the synthesis
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog counter_opt.v
synth -top counter_opt
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
We see only 3 flops after the synthesis and also seen in synthesis report after `synth -top counter_opt.v`

![Image](https://github.com/user-attachments/assets/03d30dcc-75b1-4e63-805b-052e8f42e4e1)

<img width="3338" height="771" alt="Image" src="https://github.com/user-attachments/assets/58fdb4f1-d2e2-4462-906a-85f538dacad7">

<img width="2334" height="941" alt="Image" src="https://github.com/user-attachments/assets/0621b3b7-ccee-43ac-973b-34b19998c454">


## GLS, Synthesis-Simulation Mismatch, and Blocking/Non-blocking Statements

### Why is Gate Level Simulation (GLS) necessary?
- Verify the correctness of the design after synthesis
- Ensure the timing of the design is met which is done with delay annotation (timing aware)

<img width="1256" height="626" alt="Image" src="https://github.com/user-attachments/assets/8be1d66f-928a-464c-b94e-8c49f6c3b03d">

### Synthesis Simulation Mismatches

It happens because of the following reasons
- Missing sensitivity list
- Blocking vs non-blocking assignments
- Non-standard verilog coding

#### (1) Missing sensitivity list

As shown in the screenshot below, `always` block is evaluated only when `sel` is changing. So output `y` is not evaluated when `sel` is not changing although `i0` and `i1` are changing. Rather it acts like a latch. The code on the right side represents the correct design coding for `mux`. In this case `always` is evaluated for any signal changes. 

<img width="1263" height="703" alt="Image" src="https://github.com/user-attachments/assets/892e2060-1327-424c-a46e-3a8dc13a0e77">

#### (2) Blocking vs Non-blocking Assignments

 ##### Blocking Statements
 
 - Represented by `=`
 - Executes the statements in the order it is written inside always block
 - So the first statement is evaluated before the second statement

##### Non-Blocking Statements
- Represented by `<=`
- Executes all the RHS when always block is entered and assigns to LHS
- Parallel execution

   The left side of the screenshot below gives us the correct execution. While the right side can lead to serious issues as `d` is assigned to `q` directly. ***So choosing non-blocking statements is best practice*** (highlighted in the screenshot below).

<img width="1224" height="683" alt="Image" src="https://github.com/user-attachments/assets/d817db65-6fd8-4ab2-a132-eae91dd4fe18">

##### Blocking Statements Leading to Synthesis Simulation Mismatch

In the code shown below, `y` gets the old `q0` value. This will mimic delay or flop. But when you synthesize, there will be no flop. If the order is changed (right side code), latest value of `q0` is assigned to `y`. 

When synthesized, both will lead to the same circuit. However, simulation will result in different behavior. For the left side of the code, `y` gets the old `q0` value and for the right side of the code, `y` gets the latest `q0` value leading to a synthesis simulation mismatch. 

This issue is resolved by using ***non-blocking statements***.

<img width="1229" height="665" alt="Image" src="https://github.com/user-attachments/assets/10d38869-44f3-4ed2-8e54-85b68bc333c8">

## Labs on GLS and Synthesis-Simulation Mismatch

### Ternary operator MUX (ternary_operator_mux.v)

The Verilog code of ternary_operator_mux.v
```
module ternary_operator_mux (input i0 , input i1 , input sel , output y);
	assign y = sel?i1:i0;
	endmodule
```
The command to run HDL simulation
```
iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```
HDL Simulation waveform of ternary_operator_mux.v is shown in the screenshot below

<img width="2026" height="868" alt="Image" src="https://github.com/user-attachments/assets/58778046-f7e6-47a8-8baa-c537736d2685">

The commands to run the synthesis for ternary_operator_mux.v
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ternary_operator_mux.v
synth -top ternary_operator_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog ternary_operator_mux_net.v
```
![Image](https://github.com/user-attachments/assets/e22c9cd1-31cd-4d66-b124-80e1f85e0a4e)

The commands to do GLS for ternary_operator_mux.v
```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```
The GLS output is shown below.

<img width="1964" height="1049" alt="Image" src="https://github.com/user-attachments/assets/cd517bd4-a377-4692-b75c-c70a18b92493">

### Bad MUX (bad_mux.v)

The `always` block is executed only at `sel` signal. It works like a flop rather than mux.
The Verilog code of bad_mux.v
```
module bad_mux (input i0 , input i1 , input sel , output reg y);
always @ (sel)
begin
	if(sel)
		y <= i1;
	else 
		y <= i0;
end
endmodule
```

The command to run HDL simulation
```
iverilog bad_mux.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```
HDL Simulation waveform of bad_mux.v is shown in the screenshot below

<img width="1956" height="916" alt="Image" src="https://github.com/user-attachments/assets/89cf3733-7859-4aa2-930d-a0ec0a1618ce">


The commands to run the synthesis for bad_mux.v.
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog bad_mux.v
synth -top bad_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog bad_mux_net.v
```

The synthesis report shows it is still inferring the mux but not the flop.

![Image](https://github.com/user-attachments/assets/f1e410f9-a815-41a7-8ee9-264af860ede3)

The commands to do GLS for bad_mux.v
```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```
The GLS output is shown below. This shows correct functionality which is different from HDL simulation, leading to ***synthesis simulation mismatch***.

<img width="1969" height="868" alt="Image" src="https://github.com/user-attachments/assets/9be21c1a-14ae-4f88-a63a-b361fda33181">

## Labs on Synthesis-Simulation Mismatch for Blocking Statements

### Blocking Caveat (blocking_caveat.v)

The logic to simulate is shown below.

<img width="1188" height="691" alt="Image" src="https://github.com/user-attachments/assets/2e1138af-e969-4c63-bbcf-256180b73221">

The Verilog code of blocking_caveat.v
```
module blocking_caveat (input a , input b , input  c, output reg d); 
reg x;
always @ (*)
begin
	d = x & c;
	x = a | b;
end
endmodule
```

The command to run HDL simulation
```
iverilog blocking_caveat.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```
HDL Simulation waveform of blocking_caveat.v is shown in the screenshot below. `d` takes the old value of `x` causing incorrect functionality.

<img width="1952" height="847" alt="Image" src="https://github.com/user-attachments/assets/6fcb984a-999f-4321-8a3e-e98dd4ec3cb1">

The commands to run the synthesis for bad_mux.v.
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog blocking_caveat.v
synth -top blocking_caveat
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog blocking_caveat_net.v
```

The synthesis report and logic synthesis is shown below.

![Image](https://github.com/user-attachments/assets/6714825e-08c5-4417-a980-ac4df398aa0f)

The commands to do GLS for bad_mux.v
```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```
The GLS output is shown below. In this case, `d` takes the current value of `x` causing incorrect functionality.The waveform shows correct functionality which is different from HDL simulation, leading to ***synthesis simulation mismatch***.

<img width="1961" height="925" alt="Image" src="https://github.com/user-attachments/assets/09d33e8d-ac96-4595-9497-2a94c87f47d1">

</details>

<details>
	<summary>Week 2 - BabySoC Fundamentals & Functional Modelling  </summary>

# BabySoC Fundamentals & Functional Modelling 

# Objective :
To build a solid understanding of SoC fundamentals and practice functional modelling of the BabySoC using simulation tools (Icarus Verilog & GTKWave). 

🔹 Designed a compact open-source SoC (BabySoC) based on the RVMYTH RISC-V core.
<br>
🔹 Integrated a PLL for accurate clock generation & synchronization.
<br>
🔹 Added a 10-bit DAC to enable digital-to-analog conversion.
<br>
🔹 Enabled interfacing with external analog systems (e.g., 📺 televisions, 📱 mobile phones) for audio/video outputs.
<br>
🔹 Implemented using Sky130 technology, providing a documented educational platform for exploring digital–analog interfacing.

<details>
	
<summary> What is a System-on-Chip (SoC)? </summary>

**📌 System-on-Chip (SoC) – Key Points**

 **Definition**

A System-on-Chip (SoC) is an integrated circuit (IC) that combines multiple components of a complete electronic system into a single chip.

**Core Components**

Processor/Core 🖥️ → CPU, GPU, DSP, or RISC-V/ARM cores for computation.

Memory 💾 → RAM, ROM, Flash, and cache for storage.

Peripherals ⌨️ → Interfaces like USB, I2C, SPI, UART, GPIO.

Analog Blocks 🎛️ → ADC, DAC, PLL, power management.

Interconnect 🔗 → High-speed buses (AMBA, AXI, Wishbone) for communication between blocks.

**Integration**

Everything is fabricated on one silicon die → reduces cost, area, and power.

**Advantages**

🚀 High Performance → Fast data transfer (on-chip communication).

🔋 Low Power Consumption → Optimized integration saves energy.

📏 Small Size → Replaces multi-chip PCB designs.

💰 Cost-Effective → Mass production reduces manufacturing cost.

⚡ Reliability → Fewer interconnections → lower failure rates.

**Applications**

📱 Mobile Phones (Qualcomm Snapdragon, Apple A-series).

🚗 Automotive (ADAS, infotainment).

📺 Consumer Electronics (Smart TVs, IoT devices).

🛰️ Aerospace/Defense (satellite processors).

💻 Embedded Systems & Edge AI devices.

**Technology Nodes**

Fabricated in nm technologies → 180nm, 65nm, 28nm, 7nm, down to 3nm.

Smaller node = more transistors = faster + power-efficient.

**Design Flow**

Specification → RTL Design → Functional Verification → Synthesis → Place & Route → Fabrication → Testing (DFT, Scan Chains).

**Challenges**

🔧 Power Management (low power design techniques).

🔄 Integration Complexity (multiple IPs on same die).

🔐 Security (hardware root of trust).

🧪 Verification & Testing (DFT, BIST, scan).

### Why SoCs Are Awesome

**1️⃣ Compact Integration**

CPU + Memory + Peripherals + Analog + Power circuits → all in one chip.

📏 Reduces board space → smaller devices (smartphones, IoT, wearables).

**2️⃣ High Performance**

⚡ On-chip communication (fast interconnect/NoC) → lower latency vs. multi-chip systems.

🚀 Parallel processing with CPU + GPU + DSP + AI accelerators.

**3️⃣ Low Power Consumption**

🔋 Optimized for mobile/embedded use with DVFS, power gating, clock gating.

✅ Longer battery life for handheld devices.

**4️⃣ Cost-Effective**

💰 Fewer external components = reduced manufacturing cost.

🏭 Easy mass production = economies of scale.

**5️⃣ Reliability**

🔗 Fewer off-chip connections → lower failure rate.

🛡️ On-chip security modules → hardware-level protection.

**6️⃣ Versatility**

📱 Consumer Electronics: smartphones, tablets, smart TVs.

🚗 Automotive: ADAS, infotainment, EVs.

🛰️ Aerospace/Defense: satellite processors.

🤖 AI/IoT: edge devices, wearables, robotics.

**7️⃣ Scalability & Future-Readiness**

🧩 Supports custom accelerators (AI, ML, vision).

🌐 Integrates modern connectivity → Wi-Fi, Bluetooth, 5G.

📉 Shrinks with technology nodes → from 180nm → 7nm → 3nm.

### Where You’ll Find SoCs

**📱 In Your Pocket**

Smartphones, tablets, wearables → SoCs like Snapdragon, Apple A/M series, Exynos.

They manage calls, photos, gaming, AI assistants — all from one chip!

**🚗 On the Road**

Cars run on SoCs for ADAS, infotainment, EV battery control.

Examples: NVIDIA DRIVE, Qualcomm Auto SoCs, Tesla FSD.

Your car is basically a computer-on-wheels 🛞.

**🏠 Inside Your Home**

Smart TVs, Alexa, Google Home, smart bulbs & locks.

SoCs like MediaTek, ESP32, ARM Cortex-M quietly keep your home smart & connected.

**🌐 Across Networks**

Wi-Fi routers, 5G/4G modems, even satellites.

Broadcom, Qualcomm X-series, Space-grade SoCs ensure you stay connected → from your room to outer space 🚀.

**🏥 In Healthcare**

Portable monitors, smart bands, glucose trackers.

SoCs give doctors real-time data and patients life-saving insights.

**🤖 In the Future (Already Here!)**

AI edge devices, robots, drones → powered by NVIDIA Jetson, Google Coral, NPUs.

They enable vision, intelligence, and autonomy.

### Some Popular SoCs You Might Know

Snapdragon X2 Elite ⚡ → 3 nm powerhouse for laptops & PCs.

Snapdragon 8 Elite Gen-5 📱 → flagship mobile chip with AI boost.

MediaTek Dimensity 9400 📸 → camera + AI beast for smartphones.

NVIDIA Jetson Orin 🤖 → brain of robots, drones & edge AI.

Hailo-8 / Axera AX630C 👀 → tiny but strong AI vision SoCs for IoT.

Basilisk RISC-V 🧑‍🎓 → open-source SoC for learning & research.

</details>

<details>
	
<summary> Components of a typical SoC (CPU, memory, peripherals, interconnect) </summary>

	
**📌 Components of a Typical System-on-Chip (SoC)**

**1️⃣ CPU / Processing Cores**

General Purpose CPU: ARM Cortex, RISC-V, x86 cores 🖥️

GPU (Graphics Processing Unit) 🎮: Parallel processing, graphics rendering, video acceleration.

DSP (Digital Signal Processor) 🎵: Optimized for audio, image, and real-time signal processing.

AI/ML Accelerators 🤖: Neural network processing, edge AI inference engines.

Multiple Cores (Multicore SoC): Improves performance with parallel execution.

**2️⃣ Memory Subsystem**

On-Chip Memory

SRAM (Cache): L1, L2, L3 for fast data access.

ROM: Stores firmware, boot code.

External Memory Controllers

DRAM Controllers: DDR, LPDDR.

Flash Controllers: NAND/NOR for storage.

Functions: Data/instruction storage, buffering, booting, and execution.

**3️⃣ Peripherals (I/O Interfaces)**

Communication Interfaces

Low-Speed: UART, I²C, SPI.

High-Speed: USB, PCIe, Ethernet, SATA.

Multimedia Interfaces

Display controller, HDMI, MIPI DSI.

Camera interface (CSI).

Audio codecs.

Timers & Counters ⏱️

GPIO (General-Purpose Input/Output) 🔌

Security Modules 🔐

Cryptographic accelerators.

Secure boot, trusted execution.

**4️⃣ Interconnect (On-Chip Communication)**

Bus-based Fabrics: AMBA (AXI, AHB, APB).

Crossbar Switches: Parallel high-speed data paths.

Network-on-Chip (NoC): Scalable packet-switched fabric for large SoCs.

Role: Ensures efficient CPU–Memory–Peripheral communication.

**5️⃣ Analog & Mixed-Signal Blocks**

PLL (Phase-Locked Loop) ⏱️: Clock generation, synchronization.

ADC (Analog-to-Digital Converter) 🎛️: Sensor inputs (temperature, motion, etc.).

DAC (Digital-to-Analog Converter) 🔊: Audio, video signal output.

PHY Interfaces: For USB, PCIe, Ethernet.

**6️⃣ Power Management**

Power Management Unit (PMU) 🔋: Controls power domains.

Voltage Regulators & DC-DC Converters: Supply stable voltage.

Dynamic Voltage & Frequency Scaling (DVFS) ⚡: Balances performance vs. power.

Clock Gating & Power Gating: Reduce leakage and dynamic power.

Battery Management Circuits (in mobile SoCs).

**7️⃣ Other Special Features**

Security Enhancements: Hardware root of trust, encryption modules, secure enclaves.

Debug & Test Features 🛠️: JTAG, DFT, BIST (Built-In Self-Test), scan chains.

Networking Support 🌐: Wi-Fi, Bluetooth, 5G/4G modem.

Sensor Hubs 📱: For accelerometer, gyroscope, ambient sensors.

Embedded Operating System Support: Runs Linux, RTOS, Android, or bare-metal firmware.

<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/2ced29d1-381a-4520-bcdf-ba3c52ba3b02">

In summary, **System on a Chip (SoC)** technology allows us to create powerful, efficient, and compact devices by combining multiple components into one chip. This is why our phone, smartwatch, and even some household appliances can do so much in such a small package.
</details>

<details>
<summary> Types of SoCs 🖥️✨</summary>

ASIC SoC 🎯 – Custom-built for specific apps, ultra-efficient.

MCU SoC ⚡ – CPU + memory + peripherals for embedded/IoT.

DSP SoC 🔊 – Accelerated for signal processing.

Network SoC 🌐 – Handles routers, modems & comms.

Mobile SoC 📱 – All-in-one for smartphones: CPU, GPU, DSP, modem.

FPGA SoC 🔄 – Programmable logic + CPU, flexible prototyping.

Multimedia SoC 🎥 – Powers video/audio processing & displays.

Power Mgmt SoC 🔋 – Battery & voltage control, energy-efficient.

 ### SoC Design Flow

<img width="867" height="1305" alt="image" src="https://github.com/user-attachments/assets/65a3a4a2-7d3f-4ebb-8e25-da65cb4bddff">
	
</details>

<details>
	
<summary> Introduction to VSDBabySoC </summary>


**VSDBabySoC: Compact Yet Powerful RISC-V SoC 🖥️✨**

The VSDBabySoC is a small but highly capable System-on-Chip (SoC) built on the RISC-V architecture. Its main goal is to simultaneously test three open-source IP cores for the first time while also calibrating its analog components.

**Key components include:**

RVMYTH Microprocessor – Handles the core data processing.

8x Phase-Locked Loop (PLL) – Generates a stable, synchronized clock for smooth operation.

10-bit DAC (Digital-to-Analog Converter) – Converts digital outputs to analog signals for real-world devices.

**1️⃣ Initialization & Clock Generation ⏱️**

When BabySoC receives the initial input signal, the PLL activates, producing a stable and synchronized clock. This ensures that RVMYTH and the DAC work in perfect harmony, avoiding timing mismatches and guaranteeing data integrity across the SoC.

**2️⃣ Data Processing in RVMYTH 💻**

The RVMYTH core is the brain of BabySoC. Its r17 register cycles through values generated during instruction execution. These values are prepared for analog conversion, creating a continuous stream of digital data that the DAC can process seamlessly.

**3️⃣ Analog Signal Generation via DAC 🎶📺**

The DAC receives the digital data from RVMYTH and converts it into analog signals. These outputs, saved in a file called OUT, can drive external devices like TVs, speakers, and mobile phones. This demonstrates how BabySoC bridges digital processing and real-world multimedia outputs, showing its practical applications in consumer electronics.

<img width="2270" height="1260" alt="image" src="https://github.com/user-attachments/assets/ed33055d-058e-4d55-bc83-ece6658d8e49">

## BabySoC Components 🖥️✨

**RVMYTH (RISC-V CPU) 💻**

Acts as the brain of BabySoC.

Based on the open-source RISC-V architecture, making it lightweight, flexible, and customizable.

Handles all processing tasks and communicates with other SoC components.

Perfect for learning, experimenting, and understanding CPU design and instruction flow.

**Phase-Locked Loop (PLL) ⏱️**

Generates a stable, synchronized clock to ensure smooth operation across the SoC.

Aligns BabySoC’s internal clock with a reference frequency, maintaining precise timing for RVMYTH and DAC.

Crucial for timing-critical circuits and widely used in communication and synchronization applications.

**Digital-to-Analog Converter (DAC) 🎶📺**

Converts digital data from RVMYTH into analog signals.

Enables BabySoC to interface with real-world devices like speakers, TVs, or displays.

Demonstrates how digital computation drives multimedia output, bridging the gap between digital processing and analog interaction.

### Phase-Locked Loop (PLL) ⏱️

**1️⃣ Definition**

A Phase-Locked Loop (PLL) is a control system that generates a clock signal synchronized with a reference frequency.

It continuously compares the phase of the output signal with the input reference and adjusts the output to stay in sync.

In simpler words, a PLL locks the clock of a chip to a stable reference, ensuring all components run harmoniously.

**2️⃣ Main Components of a PLL**

Phase Detector (PD) 🔍

Compares the phase of the input reference signal with the PLL’s output.

Produces a signal proportional to the phase difference.

Low-Pass Filter (LPF) 🛡️

Smooths out the output from the phase detector.

Eliminates high-frequency noise and generates a clean control voltage for the VCO.

Voltage-Controlled Oscillator (VCO ⚡)

Generates the output clock signal.

Frequency varies based on the control voltage from the filter to match the reference phase.

Feedback Path 🔄

Feeds the PLL output back to the phase detector.

Ensures continuous phase adjustment until the output is locked to the reference.

**3️⃣ Functionality**

Clock Generation: Produces a stable and precise clock for digital circuits.

Synchronization: Aligns the internal clock of ICs with an external or reference clock.

Frequency Multiplication / Division: Can generate higher or lower frequencies from a reference clock.

Jitter Reduction: Minimizes timing variations in signals for reliable operation.

**4️⃣ Why Can’t Off-Chip Clocks Always Be Used?**

Signal Degradation: Off-chip signals can suffer noise, delay, and attenuation over PCB traces.

Timing Mismatch: External clocks may not match the exact frequency requirements of internal circuits.

Power Consumption: Driving high-speed signals from off-chip sources consumes more power.

Integration Requirement: Modern SoCs require highly stable, on-chip clocks for synchronizing multiple components simultaneously.

<img width="1135" height="671" alt="image" src="https://github.com/user-attachments/assets/82f21a3f-fca2-44d3-8b66-f4cbcd7caea1" />

### Digital-to-Analog Converter (DAC) 🎶📺

**1️⃣ Definition**

A DAC (Digital-to-Analog Converter) is a device that converts digital signals (binary numbers) into continuous analog signals.

It acts as a bridge between the digital world of processors and the analog world of real devices like speakers, displays, and sensors.

In simple terms, a DAC translates 0s and 1s into voltage, current, or sound waves that the real world can interpret.

**2️⃣ Main Components of a DAC**

Digital Input Register 💻

Holds the digital value coming from a processor or microcontroller.

Prepares it for analog conversion.

Reference Voltage Source ⚡

Provides a stable voltage against which the digital input is compared.

Ensures accurate and consistent output levels.

Resistor / Current Ladder Network 🔗

Converts the digital input into proportional current or voltage.

Forms the core of most DAC architectures.

Output Amplifier / Buffer 🛡️

Converts the internal DAC signal into a usable analog output.

Ensures the output can drive external devices without distortion.

**3️⃣ Functionality**

Digital-to-Analog Conversion: Translates discrete digital values into smooth analog signals.

Multimedia Output: Generates audio signals for speakers or video signals for displays.

Control Signals: Sends analog voltages to actuators, motors, or sensors in embedded systems.

Data Interfacing: Connects microcontrollers and processors to real-world analog devices.

**4️⃣ Types of DACs ⚡**

1.Binary-Weighted DAC

Uses resistors weighted by powers of 2.

Simple but sensitive to resistor accuracy.

<img width="600" height="556" alt="image" src="https://github.com/user-attachments/assets/1b60f34e-1a51-4659-ade9-46612f5c038d">


2.R-2R Ladder DAC

Uses a ladder of resistors in a repeatable R and 2R pattern.

Popular due to ease of manufacturing and accuracy.

<img width="745" height="452" alt="image" src="https://github.com/user-attachments/assets/34633126-663d-466a-9f16-8df8df8e1c3a">

In VSDBabySoC:
      - In the VSDBabySoC design, we are utilizing a 10-bit DAC, which means it can take a digital input represented by 10 bits and convert it into an analog output.

---

This document outlines the structure and components of BabySoC, along with a basic understanding of SoCs and their types. By mastering these concepts and understanding how BabySoC operates, one gains a solid foundation in modern embedded systems design and digital-to-analog interfacing.

---
</details>

## Overview
The **VSDBabySoC** is a simple SoC (System-on-Chip) design incorporating a RISC-V processor (`rvmyth`), a PLL (Phase-Locked Loop) module (`pll`), and a DAC (Digital-to-Analog Converter) module (`dac`). This project demonstrates integration of these IP cores and aims to simulate and verify the design behavior using pre-synthesis and post-synthesis simulations.

## Project Structure
- `src/include/` - Contains header files (`*.vh`) with necessary macros or parameter definitions.
- `src/module/` - Contains Verilog files for each module in the SoC design.
- `output/` - Directory where compiled outputs and simulation files will be generated.

## Requirements
Ensure you have **Icarus Verilog** installed for compilation and **GTKWave** for viewing waveform files. This project assumes a Unix-like environment (macOS/Linux).

## Step-by-Step Guide

### 1. Setup and Prepare Project Directory
Clone or set up the directory structure as follows:
```txt
VSDBabySoC/
├── src/
│   ├── include/
│   │   ├── sandpiper.vh
│   │   └── other header files...
│   ├── module/
│   │   ├── vsdbabysoc.v      # Top-level module integrating all components
│   │   ├── rvmyth.v          # RISC-V core module
│   │   ├── avsdpll.v         # PLL module
│   │   ├── avsddac.v         # DAC module
│   │   └── testbench.v       # Testbench for simulation
└── output/
└── compiled_tlv/         # Holds compiled intermediate files if needed
```
### Module Descriptions

<details>
   <summary><strong>2.1 vsdbabysoc.v (Top-Level SoC Module)</strong></summary>
      This is the top-level module that integrates the rvmyth, pll, and dac modules.<br>
	  [VSDBabySoC](https://github.com/manili/VSDBabySoC.git)
      - Inputs:
         - reset: Resets the core processor.
         - VCO_IN, ENb_CP, ENb_VCO, REF: PLL control signals.
         - VREFH: DAC reference voltage.
      - Outputs:
         - OUT: Analog output from DAC.
         - Connections:
         - RV_TO_DAC - A 10-bit bus that connects the RISC-V core output to the DAC input.
         - CLK - The clock signal generated by the PLL.
      
</details>
 <details>
     <summary><strong>2.2 rvmyth.v (RISC-V Core)</strong></summary>
     The rvmyth module is a simple RISC-V based processor. It outputs a 10-bit digital signal (OUT) to be converted by the DAC.<br>
     [rvmyth](https://github.com/kunalg123/rvmyth/)
      
      Inputs:
         - CLK: Clock signal generated by the PLL.
         - reset: Initializes or resets the processor.
      Outputs:
         - OUT: A 10-bit digital signal representing processed data to be sent to the DAC.
         
   </details>

   <details>
     <summary><strong>2.3 avsdpll.v (PLL Module)</strong></summary>
     The pll module is a phase-locked loop that generates a stable clock (CLK) for the RISC-V core.<br>
     [Introduction](https://github.com/ireneann713/PLL.git)
     [avsdpll](https://github.com/lakshmi-sathi/avsdpll_1v8.git)
       Inputs:
         - VCO_IN, ENb_CP, ENb_VCO, REF: Control and reference signals for PLL operation.
      Output:
         - CLK: A stable clock signal for synchronizing the core and other modules.
         
         
   </details>

   <details>
     <summary><strong>2.4 avsddac.v (DAC Module)</strong></summary>
     The dac module converts the 10-bit digital signal from the rvmyth core to an analog output.<br>
     [avsddac](https://github.com/vsdip/rvmyth_avsddac_interface.git)
      
      Inputs:
         - D: A 10-bit digital input from the processor.
         - VREFH: Reference voltage for the DAC.
      Output:
         - OUT: Analog output signal.

         
   </details>

   ### Testbench
The testbench.v file is a test module to verify the functionality of vsdbabysoc. It includes signal initialization, clock generation, and waveform dumping for both pre-synthesis and post-synthesis simulations.
Waveform Output:
   - pre_synth_sim.vcd or post_synth_sim.vcd files generated based on simulation conditions.

### Simulation Steps
#### Pre-Synthesis Simulation
Run the following command to perform a pre-synthesis simulation:

```tcl
iverilog -o output/pre_synth_sim/pre_synth_sim.out -DPRE_SYNTH_SIM \
    -I src/include -I src/module \
    src/module/testbench.v src/module/vsdbabysoc.v
cd output/pre_synth_sim
./pre_synth_sim.out
```
<img width="1216" height="739" alt="Screenshot from 2025-10-02 13-50-03" src="https://github.com/user-attachments/assets/68028442-f674-4e7f-a153-f94ee7590441">

**Explanation:**
   - -DPRE_SYNTH_SIM: Defines the PRE_SYNTH_SIM macro for conditional compilation in the testbench.
   - The resulting pre_synth_sim.vcd file can be viewed in GTKWave.

#### Viewing Waveform in GTKWave
After running the simulation, open the VCD file in GTKWave:
`gtkwave output/pre_synth_sim/pre_synth_sim.vcd`

#### Post-Synthesis Simulation
To run a post-synthesis simulation, use:
```tcl
iverilog -o output/post_synth_sim/post_synth_sim.out -DPOST_SYNTH_SIM \
    -I src/include -I src/module \
    src/module/testbench.v output/synthesized/vsdbabysoc.synth.v
cd output/post_synth_sim
./post_synth_sim.out
```

### Trouble shooting tips

   - Module Redefinition: If you encounter redefinition errors, ensure modules are included only once, either in the testbench or in the command line.
   - Path Issues: Verify paths specified with -I are correct. Use full paths if relative paths cause errors.


### Simulation logs
<img width="1166" height="747" alt="Screenshot from 2025-10-02 13-07-47" src="https://github.com/user-attachments/assets/e8b7fed9-3807-4114-9a33-52360167abc4">
<img width="764" height="526" alt="Screenshot from 2025-10-02 13-21-30" src="https://github.com/user-attachments/assets/404c3b32-6a10-45b8-bb37-a94b0cebf072">
<img width="1166" height="747" alt="Screenshot from 2025-10-02 13-07-58" src="https://github.com/user-attachments/assets/ac315c71-75ca-47db-9ab1-a83473e922d0">
<img width="1166" height="747" alt="Screenshot from 2025-10-02 13-07-27" src="https://github.com/user-attachments/assets/1e82dca7-f439-429b-b0e8-3e5c0651846b">
<img width="1166" height="747" alt="Screenshot from 2025-10-02 13-07-13" src="https://github.com/user-attachments/assets/587db9b0-b2bb-45c0-80e1-56250707f4a6">

🔍 Simulation Logs – VSDBabySoC

The simulation of BabySoC Verilog modules was carried out using iverilog and GTKWave. Below is a breakdown of the simulation results:

Reset Verification ✅

Observed the active-low reset signal (rst_n) behavior.

During reset assertion, all registers and outputs were initialized to their default states.

Once reset was de-asserted, the system started executing normal operations.

Clock Operation ⏱️

Verified the clock (clk) waveform with correct periodic toggling.

Confirmed synchronous behavior across modules (signals triggered on positive clock edge).

Dataflow Analysis 📡

Data signals propagated correctly between CPU (RVMYTH), memory, and peripherals.

Verified proper handshake and data transfer between modules.

Waveform Validation 📊

Generated .vcd files using iverilog simulation.

Loaded waveforms in GTKWave to analyze transitions.

Captured screenshots highlighting reset, clock, and module dataflow activities.

Simulation Logs Snapshot 📝

The simulation logs confirm:

Successful compilation and simulation without errors.

Proper instantiation of SoC modules.

Execution flow matches expected BabySoC design functionality.

### GTKWave screenshots highlighting correct BabySoC behavior 

<img width="1216" height="739" alt="Screenshot from 2025-10-02 13-50-03" src="https://github.com/user-attachments/assets/bd1990f9-6c65-4b17-a0e0-66041f0e2f4c">

1️⃣ CLK Signal ⏱️

Definition: Primary clock input to the RVMYTH core.

Source: Generated by PLL in real hardware (but given as a simple clock in simulation).

Waveform Behavior:

Periodic square wave.

Rising edges trigger synchronous logic like registers, counters, and pipeline stages.

Observation:

All transitions in the design (reset release, data updates) are aligned with the positive clock edge.

Confirms system-wide synchronization.

2️⃣ Reset Signal 🔄

Definition: Input reset signal applied to the RVMYTH core.

Source: External source in hardware (manual or power-on reset).

Waveform Behavior:

Active-low → logic is held in reset when signal = 0.

On reset release (1), design resumes normal operation.

Observation:

Registers, memory, and output signals are forced to initial states during reset.

System only starts meaningful dataflow after reset is deasserted.

3️⃣ OUT (VSDBabySoC) 📤

Definition: The top-level SoC output port.

Source: DAC output in hardware → exported as SoC OUT pin.

Waveform Behavior:

In actual chip → analog signal.

In simulation → restricted to digital representation for compatibility.

Observation:

Shows transitions whenever the DAC receives new data from RV_TO_DAC[9:0].

Confirms end-to-end dataflow from CPU → DAC → OUT pin.

4️⃣ RV_TO_DAC[9:0] 🔢

Definition: 10-bit digital bus between RVMYTH and DAC.

Source: RVMYTH core register #17 (hardwired in BabySoC).

Waveform Behavior:

Carries binary values (0–1023) representing sampled digital data.

Changes on clock edges when new register values are updated.

Observation:

Any activity on this bus directly affects DAC output.

Validates CPU-to-DAC interfacing.

5️⃣ OUT (DAC – real datatype) 🎚️

Definition: Internal wire in DAC module that supports analog simulation.

Source: Derived from RV_TO_DAC[9:0] via DAC conversion logic.

Waveform Behavior:

Real datatype → represents analog-like values in simulation.

Provides continuous waveform instead of stepwise digital output.

Observation:

Helps visualize DAC’s analog functionality, even though Verilog restricts real hardware analog simulation.

Confirms correct scaling of digital input into analog range.


This week, I successfully:
1️⃣ Cloned and explored the VSDBabySoC repository 🖥️
2️⃣ Compiled the Verilog modules using Icarus Verilog ⚙️
3️⃣ Generated .vcd files and analyzed them in GTKWave 📊
4️⃣ Verified critical signals – CLK, Reset, RV_TO_DAC[9:0], and OUT 🔍
5️⃣ Captured simulation logs & screenshots to document reset behavior, clock synchronization, and dataflow between modules 📝

✨Week 2 provided hands-on experience in functional simulation of BabySoC, strengthening my understanding of SoC signal interactions and waveform analysis 🚀🔧
</details>

<details>
	<summary>Week 3 - Post-Synthesis GLS & STA Fundamentals </summary>

<details>
<summary> ⚙️ Gate-Level Simulation (GLS) of BabySoC </summary>
	
**🧩 Post-Synthesis Verification Phase**
**🎯 Purpose of GLS**

Gate-Level Simulation (GLS) is the reality check for our BabySoC design 💡.
After synthesis converts the RTL description into a gate-level netlist, GLS ensures that the design still behaves exactly as intended — but now with real hardware timing taken into account. Unlike RTL simulations that work on abstract behavioral models, GLS dives deep into the actual logic gates and interconnections that form the silicon foundation of the SoC 🧠⚡.

**🔍 Why GLS Matters for BabySoC**

**⏱️ Timing-Aware Verification:**
GLS is performed using Standard Delay Format (SDF) files that include the post-synthesis delays.
It helps verify that the design meets real-world timing constraints — checking if the SoC runs smoothly without timing violations or setup/hold issues ⏳✅.

**🧠 Functional Validation after Synthesis:**
Even after synthesis transforms RTL into gates, the logic must remain intact. GLS ensures that no logical discrepancies or unwanted glitches have crept in during synthesis. It confirms the trust between what was written and what will be fabricated.

**🔧 Simulation Tools in Action:**
Simulation is carried out using tools like Icarus Verilog (iverilog) or similar Verilog simulators 🧮.
Post-simulation, GTKWave is used to visualize and analyze the waveforms — letting us watch signal transitions, debug timing behavior, and confirm correctness visually 📊👀.

**🧠 Importance for BabySoC Architecture:**
BabySoC integrates multiple modules — RISC-V Processor (RVMYTH), PLL, and DAC — all working hand-in-hand 🤝.
GLS ensures that these modules communicate seamlessly and meet timing requirements, validating that the SoC design is synthesis-accurate, timing-correct, and silicon-ready 🚀.

🧭 Step-by-Step Manual Execution Plan
Step 1: Launch Yosys and Load Design
yosys

<img width="1213" height="672" alt="image" src="https://github.com/user-attachments/assets/5b878f4e-0fee-4f3a-908b-26313be4bdbc" />

Start by opening Yosys, the synthesis tool.
Load your top-level BabySoC design along with all its supporting RTL modules.
This prepares the environment for generating the gate-level netlist and proceeding with the simulation 🧱➡️⚙️.

Inside the Yosys shell, run:
```yosys
read_verilog src/module/vsdbabysoc.v
read_verilog -I src/include src/module/rvmyth.v
read_verilog -I src/include src/module/clk_gate.v

```
<img width="1215" height="770" alt="vsdbaby" src="https://github.com/user-attachments/assets/307647c6-52d1-423b-abcd-1fc3577f2066" />
<img width="1072" height="213" alt="rvmyth" src="https://github.com/user-attachments/assets/44d099a9-2729-4073-b5e5-b81dc1179b2b" />
<img width="1069" height="204" alt="clk_gate" src="https://github.com/user-attachments/assets/ade3a799-a6cf-467e-b80d-192954ac48a7" />

---

### **Step 2: Load the Liberty Files for Synthesis**
Inside the same Yosys shell, run:
```yosys
read_liberty -lib src/lib/avsdpll.lib
read_liberty -lib src/lib/avsddac.lib
read_liberty -lib src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
<img width="1176" height="415" alt="lib" src="https://github.com/user-attachments/assets/357bd655-ba95-4438-8243-b684ec4269af" />

---

### **Step 3: Run Synthesis Targeting `vsdbabysoc`**
```yosys
synth -top vsdbabysoc
```
<img width="1199" height="712" alt="Image" src="https://github.com/user-attachments/assets/8e8be4eb-c3a2-41c5-90af-36d9562dc15a" />
<img width="1194" height="664" alt="Image" src="https://github.com/user-attachments/assets/a6d947ad-c949-48e3-93f0-60f105ca7302" />
<img width="1210" height="727" alt="Image" src="https://github.com/user-attachments/assets/6ebd2360-45cc-4711-9dba-38d3b39fbecc" />
<img width="1210" height="727" alt="Image" src="https://github.com/user-attachments/assets/05039c6c-c25b-4554-841b-20f5f571e38e" />
<img width="1199" height="712" alt="Image" src="https://github.com/user-attachments/assets/8f0d4516-cba1-4a5b-955f-37c7fa1d2531" />
<img width="1210" height="727" alt="Image" src="https://github.com/user-attachments/assets/9c8a1ef2-83d8-4680-ac8a-5b86c1594bcd" />

---

### **Step 4: Map D Flip-Flops to Standard Cells**
```yosys
dfflibmap -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
<img width="1219" height="774" alt="dff" src="https://github.com/user-attachments/assets/7d8ed1bf-f110-44f4-9aad-9fd24b457255" />

---

### **Step 5: Perform Optimization and Technology Mapping**
```yosys
opt
abc -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib -script +strash;scorr;ifraig;retime;{D};strash;dch,-f;map,-M,1,{D}
```
<img width="1219" height="774" alt="Image" src="https://github.com/user-attachments/assets/3c9e4971-5fbd-47be-9d97-2113cbe68e0d" />
<img width="1219" height="774" alt="Image" src="https://github.com/user-attachments/assets/ffc238b9-9167-4e9f-8679-6f2150d8bc3f" />
<img width="1219" height="774" alt="Image" src="https://github.com/user-attachments/assets/2857cbe3-b680-445b-852b-67622adbc4d4" />
<img width="1219" height="777" alt="Image" src="https://github.com/user-attachments/assets/2fa8c51f-8bc8-474a-8c6d-eacf5e1e50f5" />
<img width="1219" height="777" alt="Image" src="https://github.com/user-attachments/assets/48ca6638-51b4-48d3-b5da-f82ae6f30cef" />

---

### **Step 6: Perform Final Clean-Up and Renaming**
```yosys
flatten
setundef -zero
clean -purge
rename -enumerate
```
<img width="1179" height="375" alt="flat" src="https://github.com/user-attachments/assets/9c9190ef-feaf-4f1d-9469-4024b43b11ef" />

---

### **Step 7: Check Statistics**
```yosys
stat
```
<img width="1155" height="659" alt="image" src="https://github.com/user-attachments/assets/4545dcf8-cf7d-4312-a397-01f30a565d13" />
<img width="1155" height="659" alt="image" src="https://github.com/user-attachments/assets/c07ffd6c-e55d-4308-a64b-05a591cc479b" />
<img width="1155" height="659" alt="image" src="https://github.com/user-attachments/assets/dd30d12c-8106-484f-9c82-dbf85e2a9d5a" />
<img width="1224" height="420" alt="image" src="https://github.com/user-attachments/assets/2878f87d-33fd-4294-b370-882887318dd0" />

---

### **Step 8: Write the Synthesized Netlist**
```yosys
write_verilog -noattr output/post_synth_sim/vsdbabysoc.synth.v
```
<img width="1166" height="117" alt="write" src="https://github.com/user-attachments/assets/61979497-5f89-4989-b0fa-6cc04c361602" />

---

## POST_SYNTHESIS SIMULATION AND WAVEFORMS
---

### **Step 1: Compile the Testbench**
Run the following `iverilog` command to compile the testbench:
```bash
iverilog -o output/post_synth_sim/post_synth_sim.out -DPOST_SYNTH_SIM -DFUNCTIONAL -DUNIT_DELAY=#1 -I src/include -I src/module src/module/testbench.v output/synth/vsdbabysoc.synth.v
```
---
### **Step 2: Navigate to the Post-Synthesis Simulation Output Directory**
```bash
cd output/post_synth_sim/
```
---
### **Step 3: Run the Simulation**

```bash
./post_synth_sim.out
```
---
### **Step 4: View the Waveforms in GTKWave**

```bash
gtkwave post_synth_sim.vcd
```
---
<img width="1214" height="770" alt="1" src="https://github.com/user-attachments/assets/016eefe2-d5a4-4e1f-b9e5-e8ed6527de75" />
<img width="1214" height="717" alt="2" src="https://github.com/user-attachments/assets/d4de60cc-30d4-436a-8cc4-a576622b97ce" />
<img width="1219" height="657" alt="3" src="https://github.com/user-attachments/assets/3e48a500-c1eb-4520-a2ca-a550b001af3b" />
<img width="1042" height="669" alt="last" src="https://github.com/user-attachments/assets/efb23aea-ae9b-44b6-b64e-d61e5d64313e" />

✨ In short, GLS acts as the final checkpoint between design and hardware — ensuring that our BabySoC beats to the right clock, with real-world timing accuracy and functional integrity ❤️🔐.
</details>

<details>
<summary> Fundamentals of STA </summary>

🕒 Static Timing Analysis (STA)

STA ensures your digital circuit works reliably at the target clock frequency by checking all timing paths.

**1️⃣ Setup / Hold Analysis**

These checks make sure data is stable and captured correctly by flip-flops or latches:

reg2reg: Timing between two flip-flops 🔁 ensures data launched by one reaches the next safely.

in2reg: Input pin to register timing ➡️🧩 checks signals entering the design.

reg2out: Register to output timing 🧩➡️ guarantees proper output timing.

in2out: Input to output combinational paths ➡️⚡ are validated.

clock gating: Checks timing impact when clocks are gated for power saving 🕹️💡.

recovery/removal: Ensures flip-flops recover properly from async reset ♻️🛠️.

data-to-data: Validates timing through combinational paths 🔗⚡.

latch time borrowing: Latches can "borrow" time for slightly late data ⏳🟡.

Launch flop → combinational logic → capture flop. Yellow dots indicate key timing points.

**2️⃣ Slew / Transition Analysis**

Analyzes signal rise and fall speed, affecting timing integrity:

Data transitions 📈📉 must be fast enough to meet setup/hold.

Clock transitions ⏰📈 must be clean to avoid timing issues.

Too slow or too fast transitions can cause violations or glitches.

**3️⃣ Load Analysis**

Looks at fanout and capacitance that impact delay:

High fanout 🔌🧩 slows down signal propagation.

Large capacitance ⚡🪫 increases signal delay.

These factors are crucial for accurate propagation delay estimation.

**4️⃣ Clock Analysis**

Checks clock timing and quality:

Skew ↔️⏰ ensures the same clock reaches all flip-flops at proper times.

Pulse width ⏱️📏 guarantees the clock pulse is long enough for reliable latching.

Proper clock analysis ensures synchronous design stability.

**5️⃣ Diagram Highlights**

Standard STA path: Launch Flop → Combinational Logic → Capture Flop.

Yellow dots mark critical timing points.

Clock gating & reset signals show control path effects.

Clock network includes buffers and gates affecting skew.

Pulse width waveform validates minimum clock duration.

![WhatsApp Image 2025-10-10 at 17 29 15_562451d5](https://github.com/user-attachments/assets/1a510828-d3be-485a-81a6-4364586b1c00)

### 📊 STA DAG Analysis

This diagram is a timing graph used in Static Timing Analysis to check signal propagation delays, arrival times, required times, and slack across a digital circuit.

**Convert Logic Gates into Nodes**

Each logic gate (AND, OR, MUX, etc.) is split into input pins → gate arc → output pin.

In the graph above, you see pins (i1, a1, b0, etc.) as nodes.

This makes timing analysis fine-grained, because delay depends on pin-to-pin paths rather than the whole gate.
👉 Think of it like zooming in 🔍 on gate connections instead of the whole block.


Compute Actual Arrival Time (AAT)

AAT (Blue A) = the earliest time a signal arrives at a node.
```
Formula:

𝐴𝐴𝑇=max(𝐴𝐴𝑇𝑖𝑛𝑝𝑢𝑡𝑠+𝑑𝑒𝑙𝑎𝑦𝑎𝑟𝑐)AAT=max(AATinputs+delayarc)

```

In the figure:

i1 has A=0 (source).

After a delay of 0.1, b1 gets A=0.1.

At deeper nodes like o1, you see A=7.9, showing total accumulated path delay.
👉 Blue numbers = when the data actually arrives ⏰.


**Compute Required Arrival Time (RAT)**

RAT (Yellow R) = the latest time a signal can arrive without violating timing.

Calculated by back-propagation from output to input.

```
Formula:

𝑅𝐴𝑇=min⁡(𝑅𝐴𝑇 𝑜𝑢𝑡𝑝𝑢𝑡−𝑑𝑒𝑙𝑎𝑦𝑎𝑟𝑐)RAT=min(RAT output−delayarc)

```

In the diagram:

At output node o1, R=7.55.

Back-propagated: c0 has R=5.2, c2 has R=2.2.
👉 Yellow numbers = required deadlines ⏳.

**Compute Slack**

Slack (Red S) = margin between RAT and AAT.
 
 ```
Formula:

𝑆𝑙𝑎𝑐𝑘=𝑅𝐴𝑇−𝐴𝐴𝑇Slack=RAT−AAT

```

Example from diagram:

At o1, S=-0.35.

Negative slack ❌ means a timing violation (path is too slow).
👉 Slack = safety margin 🛟. Positive = safe, Negative = fail 🚨.

**Convert Pins to Nodes & Do GBA/PBA Analysis**

Why convert pins into nodes?

It allows per-pin timing, more accurate than just per-gate.

Delays differ between input→output arcs, so pin-level modeling avoids under/overestimation.

Graph-Based Analysis (GBA):

Approximates worst-case paths quickly.

Uses max arrival times without exploring all real paths.

Path-Based Analysis (PBA):

Explores actual paths.

More accurate but slower.
👉 GBA = fast check ⚡, PBA = detailed deep dive 🧮.

![WhatsApp Image 2025-10-10 at 17 47 41_77dca644](https://github.com/user-attachments/assets/e2b702d0-e4dd-40f8-957a-f05eae59099d)

**🔋 1. Introduction to transistor-level circuit for flops**

At the transistor level, a flip-flop (FF) is made up of MOSFETs (both PMOS and NMOS) arranged to store a bit of data.

Think of it like a tiny lock 🗝️ where the transistors act as switches to control the data flow.

Cross-coupled inverters 🔄 create feedback to hold the state.

Transmission gates 🚪 or pass transistors decide when new data enters.

![WhatsApp Image 2025-10-10 at 22 39 21_d9d7a267](https://github.com/user-attachments/assets/26d3220c-2900-4153-a2d5-14ec43dfcd04)

**🔄 2. Negative and positive latch transistor-level operation**

Positive latch ➕🔓: Transparent when clk = 1, it passes input to output. When clk = 0, it locks the value.

Negative latch ➖🔓: Transparent when clk = 0, and locks when clk = 1.

Both are just transistor networks (pass-transistor + inverters) that act like a door that only opens at specific clock times ⏰.

![WhatsApp Image 2025-10-10 at 22 37 29_07def61e](https://github.com/user-attachments/assets/9fe0706c-cd7a-4346-948f-959f3a9d4bcf)
![WhatsApp Image 2025-10-10 at 22 37 29_ba447a66](https://github.com/user-attachments/assets/1b052939-981c-4c13-8e08-0444eede744d)
![WhatsApp Image 2025-10-10 at 22 37 28_f61e1214](https://github.com/user-attachments/assets/00643390-b327-4a09-b4c4-84d9c7f78e51)

**📏 3. Library setup time calculation**

Setup time is the minimum time ⏳ the input data must be stable before the clock edge.

Libraries calculate this by sweeping input arrival times and checking when the flop still latches data correctly.

If violated ❌, you get metastability (output stuck in an undefined state 🤯).

![WhatsApp Image 2025-10-10 at 22 37 28_e0e73430](https://github.com/user-attachments/assets/47601a84-f25d-4c62-811f-beb7685ec212)

**⏱️ 4. Clk-Q delay calculation**

Clk-Q delay = time taken for the output (Q) to change after the clock edge.

At the transistor level, this depends on:

Load capacitance ⚡

Transistor sizing 📐

Supply voltage 🔋

It’s basically the reaction time of the flop once it hears the clock’s "GO!" 🏃‍♂️.

![WhatsApp Image 2025-10-10 at 22 37 27_c0188d09](https://github.com/user-attachments/assets/cc777537-4ad0-411f-a0a7-815a3b473baf)

**📉 5. Steps to create eye diagram for jitter analysis**

An eye diagram 👁️ is made by overlaying multiple signal transitions to visualize data quality.
Steps:

Collect clock/data waveform 📡 over many cycles.

Overlay them on the same time window 🪞.

The open “eye” 👁️ shows good timing margins; a closed eye means poor signal integrity ⚠️.

Used in high-speed designs like SerDes, DDR, etc.

![WhatsApp Image 2025-10-10 at 22 37 26_420f017c](https://github.com/user-attachments/assets/758b5bc8-31ae-4c62-9ce5-ebd9bc76571a)

**📊 6. Jitter extraction and accounting in setup timing analysis**

Jitter = randomness in clock edges 🌀.

Extracted from measured or simulated waveforms by checking clock edge variations relative to ideal timing.

In setup analysis, jitter reduces effective available time:

Effective time = Clock period – (setup + jitter)

Think of it like traffic delays 🚦—you always subtract buffer time to be safe.

![WhatsApp Image 2025-10-10 at 22 37 26_29b7af03](https://github.com/user-attachments/assets/e7849118-690c-4065-92dc-c76146e9bf4f)

**Setup Analysis – Graphical ➡️ Textual Representation**

Graphical view:

You see a launch flop 📤 and a capture flop 📥 connected through combinational logic ⚡.

The clock edge ⏰ triggers the launch flop.

Data must reach the capture flop’s input before the next active clock edge.

Textual meaning:

Data launches at time T0.

Must arrive at capture flop input before Tclk – Tsetup.
```
Setup check equation:

Launch_clk + Data_path_delay ≤ Capture_clk + Tperiod – Tsetup
```
If violated ❌, the capture flop may miss data or go metastable 🤯.

![WhatsApp Image 2025-10-10 at 22 55 35_9eb0a493](https://github.com/user-attachments/assets/5b9b9ffd-73bd-40d2-833b-7b9784ba82fa)

**Hold Analysis with Real Clocks**

Real scenario:

Unlike setup (which looks across cycles 🌀), hold checks within the same clock edge.

Data must not change too soon after the capture clock edge.

With real clocks ⏱️, you also consider skew (difference in arrival times).

If launch clock arrives earlier than capture clock ➡️ data may race ahead 🏃 and overwrite the old value.
```
Equation form:

Launch_clk + Data_path_delay ≥ Capture_clk + Thold
```
Violation → Race condition ⚔️.

![WhatsApp Image 2025-10-10 at 22 58 28_6948b632](https://github.com/user-attachments/assets/dabbf3b4-bb47-4b47-a28a-24f01489ce43)

**Hold Analysis – Graphical ➡️ Textual Representation**

Graphical view:

Imagine two flops connected directly 🔗 with very little delay.

The launched data zooms 🚀 and may reach the capture flop too early.

On the timing diagram, the new data edge overlaps old data window → problem! ⚠️

Textual meaning:

Hold ensures the old data is held stable for at least Thold after the capture clock.

If violated, capture flop sees new data instead of old 🪞.

Fix → add delay buffers 🧱 in the data path.

**Sources of Variation – Etching 🧪**

Etching = removing unwanted material during fabrication (like carving tiny valleys 🪓).
But — it’s not perfectly uniform!

🧩 Cause:

Non-uniform plasma density or timing errors ⏱️

Leads to over-etch (too deep 🕳️) or under-etch (too shallow 🧱)

📊 Effect:

Changes width (W) of metal or polysilicon lines → affects resistance (R).

Example:

ΔW = ±2 nm → ΔR ≈ ±5%

📉 So etching variation = unpredictable changes in interconnect delay & transistor strength.

![WhatsApp Image 2025-10-10 at 23 20 16_a90563ec](https://github.com/user-attachments/assets/4a828451-ef46-42a3-9129-e013fed9f76c)
![WhatsApp Image 2025-10-10 at 23 20 16_991164eb](https://github.com/user-attachments/assets/323330fa-bac7-42bd-aba1-86d9e58ce677)

**🧱 Sources of Variation – Oxide Thickness (Tox)**

Gate oxide = the ultra-thin layer 🧈 between the gate and channel.
Tiny changes here have a huge impact!

🔍 If Tox ↑ (thicker):

Less gate control 🎛️

Lower capacitance (Cox ↓)

Threshold voltage (Vth ↑) → transistor turns ON slower 🐢

⚡ If Tox ↓ (thinner):

Higher gate control but more leakage current 🔥

📊 Quantitatively:

Cox = εox / Tox

Small ΔTox → large ΔCox → affects Id and delay

e.g., 5% Tox variation → ~10% Id variation

![WhatsApp Image 2025-10-10 at 23 20 15_3a9f423a](https://github.com/user-attachments/assets/d222bd53-c9d8-4780-a456-8fb4a2073d65)

**⚙️Relationship Between Resistance (R), Drain Current (Id), and Delay (τ)**

Let’s connect the dots 🔗

a. Resistance (R):

In wires/interconnects, 
```
𝑅=𝜌𝐿𝐴R=ρAL
```

If etching increases R, charging a node takes longer 🐌

b. Drain Current (Id):
```
𝐼𝑑∝𝜇𝐶𝑜𝑥𝑊𝐿(𝑉𝑔𝑠−𝑉𝑡ℎ)2Id∝μCoxLW(Vgs−Vth)2
```
If Tox ↑ → Cox ↓ → Id ↓ (weaker drive 💪 → slower switching)

c. Delay (τ):
```
Delay ≈ 𝐶𝑙𝑜𝑎𝑑⋅𝑉𝑑𝑑𝐼𝑑IdCload⋅Vdd
```
So:
```
↑ R → ↑ delay ⏳

↓ Id → ↑ delay ⏳
```
💡 Intuitive chain:
```
Etch error ↑ → R ↑ → current flow ↓ → delay ↑
Tox variation ↑ → Id ↓ → delay ↑
```
![WhatsApp Image 2025-10-10 at 23 26 33_7d6e194f](https://github.com/user-attachments/assets/ccf55708-ddad-4fd4-b00e-dd6f546a4ad0)
![WhatsApp Image 2025-10-10 at 23 26 33_e6b229c6](https://github.com/user-attachments/assets/4a697c34-59ac-42a9-ae76-77a1c16242d2)
![WhatsApp Image 2025-10-10 at 23 26 47_335d4642](https://github.com/user-attachments/assets/9c15f297-0c05-46b1-a34f-e065800086eb)

</details>

<details>
	<summary> Generate Timing Graphs with OpenSTA </summary>

**⚡ Static Timing Analysis**

👉 This project is a spin-off of parallaxsw/OpenSTA
.
📌 For any issues or pull requests, please contribute to the original repo.

**🕒 Parallax Static Timing Analyzer**

🔍 OpenSTA is a gate-level timing checker.
As a standalone tool, it helps you analyze and verify the timing of digital circuits using industry-standard file formats.

📂 Supported inputs:

📜 Verilog (design netlist)

📕 Liberty (cell libraries)

⏱️ SDC (timing rules/constraints)

🧾 SDF (delay info)

🪢 SPEF (parasitics)

📊 VCD (power activities)

🔌 SAIF (switching activities)

⚙️ It runs with a TCL interpreter, allowing you to:

🏗️ Load your design

⏲️ Define timing requirements

📝 Generate timing reports

**🕒 Clocks**

🔄 Generated clocks

🕰️ Latency

⏳ Source latency (aka insertion delay)

🌫️ Uncertainty

🎯 Propagated / Ideal clocks

🚦 Gated clock validation

⏱️ Multi-frequency clock support

**🚧 Exception Paths**

❌ False paths

🔂 Multicycle paths

⬆️⬇️ Min / Max delay paths

🎯 Exception definition points:

🔗 -from (clock/pin/instance)

🧵 -through (pin/net)

🎯 -to (clock/pin/instance)

🎚️ Edge-specific exceptions

⬆️ -rise_from, -rise_through, -rise_to

⬇️ -fall_from, -fall_through, -fall_to

**⚡ Delay Calculation**

🧮 Built-in Dartu/Menezes/Pileggi RC effective capacitance method

🔌 External delay calculator API for flexibility

**📊 Analysis**

📑 Report timing checks (-from, -through, -to, multi-paths)

🧾 Delay calculation reports

✅ Setup/hold verification

**🛠️ Timing Engine**

OpenSTA is designed like a plug-and-play engine ⚙️ that can attach to other EDA tools without duplicating netlist data.

🔍 Query-based incremental updates (arrival times, required times, delays)

🖥️ Simulator to handle constants from constraints & tie-high/low nets

**📚 Documentation references:**

📘 doc/OpenSTA.pdf → command guide

📝 doc/ChangeLog.txt → command updates

⚙️ doc/StaApi.txt → API details

**⚖️ Licensing & Usage**

🆓 Open source (GPL v3) under the name OpenSTA

💼 Commercial license available via Parallax Software (no GPL obligations)

**🛠️ Code can be compiled locally 🔧**

🧩 Derivative works allowed ✅ (must follow GPL terms)

🚫 Removing license/copyright info ❌ = illegal

https://github.com/parallaxsw/OpenSTA.git. Any forks from this code
base have not passed extensive regression testing which is not
publicly available.

## Build from source

OpenSTA is built with CMake.

### Prerequisites

The build dependency versions are shown below.  Other versions may
work, but these are the versions used for development.

```
         Ubuntu   Macos
        22.04.2   14.5
cmake    3.24.2    3.29.2
clang             15.0.0
gcc      11.4.0
tcl       8.6      8.6.16
swig      4.1.0    4.1.1
bison     3.8.2    3.8.2
flex      2.6.4    2.6.4
```

External library dependencies:
```
           Ubuntu   Darwin  License
eigen       3.4.0   3.4.0   MPL2  required
cudd        3.0.0   3.0.0   BSD   required
tclreadline 2.3.8   2.3.8   BSD   optional
zLib        1.2.5   1.2.8   zlib  optional
```

The [TCL readline library](https://tclreadline.sourceforge.net/tclreadline.html)
links the GNU readline library to the TCL interpreter for command line
editing To enable TCL readline support use the following Cmake option:
See (https://tclreadline.sourceforge.net/) for TCL readline
documentation. To change the overly verbose default prompt, add
something this to your ~/.sta init file:

```
if { ![catch {package require tclreadline}] } {
  proc tclreadline::prompt1 {} {
    return "> "
  }
}
```

The Zlib library is an optional.  If CMake finds libz, OpenSTA can
read Liberty, Verilog, SDF, SPF, and SPEF files compressed with gzip.

CUDD is a binary decision diageram (BDD) package that is used to
improve conditional timing arc handling, constant propagation, power
activity propagation and spice netlist generation.

CUDD is available
[here](https://github.com/davidkebo/cudd/blob/main/cudd_versions/cudd-3.0.0.tar.gz).

Unpack and build CUDD.

```
tar xvfz cudd-3.0.0.tar.gz
cd cudd-3.0.0
./configure
make
```

You can use the "configure --prefix" option and "make install" to install CUDD
in a different directory.

### Building with CMake

Use the following commands to checkout the git repository and build the
OpenSTA library and excutable.

```
git clone https://github.com/parallaxsw/OpenSTA.git
cd OpenSTA
mkdir build
cd build
cmake -DCUDD_DIR=<CUDD_INSTALL_DIR> ..
make
```
The default build type is release to compile optimized code.
The resulting executable is in `build/sta`.
The library without a `main()` procedure is `build/libOpenSTA.a`.

Optional CMake variables passed as -D<var>=<value> arguments to CMake are show below.

```
CMAKE_BUILD_TYPE DEBUG|RELEASE
CMAKE_CXX_FLAGS - additional compiler flags
TCL_LIBRARY - path to tcl library
TCL_HEADER - path to tcl.h
CUDD_DIR - path to cudd installation
ZLIB_ROOT - path to zlib
CMAKE_INSTALL_PREFIX
```

If `TCL_LIBRARY` is specified the CMake script will attempt to locate
the header from the library path.

The default install directory is `/usr/local`.
To install in a different directory with CMake use the CMAKE_INSTALL_PREFIX option.

If you make changes to `CMakeLists.txt` you may need to clean out
existing CMake cached variable values by deleting all of the
files in the build directory.

## Build with Docker

An alternative way to build and run OpenSTA is with
[Docker](https://www.docker.com).  After installing Docker, the
following command builds a Docker image.

```
cd OpenSTA
docker build --file Dockerfile.ubuntu22.04 --tag opensta .
```

To run a docker container using the OpenSTA image, use the -v option
to docker to mount direcories with data to use and -i to run
interactively.

```
docker run -i -v $HOME:/data opensta
```

## Build on Macos/Darwin

The XCode versions of Tcl, Flex and Bison cannot be used to build OpenSTA.
Use Homebrew to install them. The following command installs the tools
required to build OpenSTA in the Brewfile.

```
brew bundle install
```

Set these variables before using cmake to cirumvent the Xcode versions.

```
  # flex/bison override apple version
  export PATH="$(brew --prefix bison)/bin:${PATH}"
  export PATH="$(brew --prefix flex)/bin:${PATH}"
  export CMAKE_INCLUDE_PATH="$(brew --prefix flex)/include"
  export CMAKE_LIBRARY_PATH="$(brew --prefix flex)/lib;$(brew --prefix bison)/lib"
```

Homebrew does not support tclreadline, but the macports system does
(see https://www.macports.org). 

## Install using a package manager

### Guix

OpenSTA is available in the [default repositories](https://hpc.guix.info/package/opensta):

```
  guix install opensta
```

## Bug Reports

Use the Issues tab on the github repository to report bugs.

Each issue/bug should be a separate issue. The subject of the issue
should be a short description of the problem. Attach a test case to
reproduce the issue as described below. Issues without test cases are
unlikely to get a response.

The files in the test case should be collected into a directory named
YYYYMMDD where YYYY is the year, MM is the month, and DD is the
day (this format allows "ls" to report them in chronological order).
The contents of the directory should be collected into a compressed
tarfile named YYYYMMDD.tgz.

The test case should have a tcl command file recreates the issue named
run.tcl. If there are more than one command file using the same data
files, there should be separate command files, run1.tcl, run2.tcl
etc. The bug report can refer to these command files by name.

Command files should not have absolute filenames like
"/home/cho/OpenSTA_Request/write_path_spice/dump_spice" in them.
These obviously are not portable. Use filenames relative to the test
case directory.

## Contributions

Contributors must sign the Contributor License Agreement (doc/CLA.txt)
when submitting pull requests.

All contributors should read doc/CodingGuidelines.txt for notes on
making code that adheres to the existing naming and formatting style.

Contributions that claim 4% performance improvements in OpenROAD flow
scripts will largely be ignored. Small performance improvements
simply do not justify the time required to audit and verify the changes.

Contributions that add dependencies on external libraries like boost,
abseil and Intel TBB will not be accepted.

As the author of OpenSTA I vastly prefer writing code to reviewing
code.  I don't have the patience to go round after round to correct
code formatting that is not consistent with the rest of the code.

## Authors

* James Cherry

* William Scott authored the arnoldi delay calculator at Blaze, Inc
  which was subsequently licensed to Nefelus, Inc that has graciously
  contributed it to OpenSTA.

## License

OpenSTA, Static Timing Analyzer
Copyright (c) 2023, Parallax Software, Inc.

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program. If not, see <https://www.gnu.org/licenses/>.

**⚙️ Static Timing Analysis (STA) using OpenSTA**

This project demonstrates how OpenSTA (Open Source Static Timing Analyzer) is used to analyze timing characteristics like setup paths, hold paths, and slack for a synthesized digital design.
The goal was to perform a full STA flow — from loading the design to visualizing timing paths and generating detailed reports.

**🧩 1️⃣ Loading the Synthesized Netlist and Constraints**

In the first step, the complete design environment was set up inside OpenSTA.
This involves reading the cell library, synthesized netlist, and timing constraints. Each file plays a specific role in the timing analysis flow.

🔹 Files Used:

📘 slow.lib → Standard cell timing library (defines cell delays, setup/hold requirements, transition times, etc.)

📗 top.v → Synthesized gate-level netlist (generated after synthesis; includes all logic instances and connections)

📙 constraints.sdc → Synopsys Design Constraints file (defines clock periods, input/output delays, and timing exceptions)

🔹 Commands Executed:
read_liberty slow.lib          ;# Load cell library
read_verilog top.v             ;# Read synthesized netlist
link_design top                ;# Link top-level design
read_sdc constraints.sdc       ;# Apply design constraints
update_timing                  ;# Perform initial timing propagation

💡 Explanation:

read_liberty loads the delay and timing models for every standard cell used.

read_verilog imports the gate-level design hierarchy.

link_design ensures all cell references are resolved using the library.

read_sdc applies the defined timing environment (clocks, delays).

update_timing runs the timing engine to calculate path delays and setup/hold requirements.

✨ Outcome:

At the end of this step, OpenSTA successfully created the timing graph internally, representing all logic connections and clock domains.
This serves as the base for detailed timing path analysis in the next steps.

**📊 2️⃣ Generating Timing Graphs (Setup/Hold Paths & Slack)**

After the design setup, timing paths were extracted and visualized to study setup and hold behavior.
The goal was to understand how data propagates between sequential elements and to identify any slack violations.

🧠 Explanation:

The report_timing command traces the critical path from a source register (U1/Q) to a destination register (U2/D).

The -format dot option exports the path in Graphviz (.dot) format — a standard graphical representation format.

Graphviz then converts this .dot file into a visual image (.png), clearly showing cell delays, net delays, and slack values.

🧩 Graph Features:

⏱️ Propagation delay: Total delay of data through combinational logic.

⛔ Setup slack: Difference between arrival time and required time at capture flip-flop.

⚡ Hold slack: Minimum time between launch and capture events.

🔁 Clock path analysis: Visualizes the relationship between clock and data arrival times.

✨ Outcome:

A graphical timing representation (timing_graph.png) was generated.
It displayed the critical timing path, highlighting delay elements and slack — giving a clear view of how close the design is to timing closure.

**📑 3️⃣ Capturing Timing Reports and Corresponding Graphs**

To quantify the results, multiple reports were generated, capturing both detailed and summary-level timing metrics.
This step ensures timing analysis is measurable, verifiable, and documented.

🔹 Commands Executed:
report_checks -path_delay min_max -fields {slew capacitance delay time slack} -digits 3 > timing_report.txt
report_tns > tns_report.txt
report_wns > wns_report.txt

🧠 Explanation:

report_checks gives detailed information on setup and hold timing checks, including cell delay, net delay, and slack.

report_tns provides Total Negative Slack, representing the total sum of all timing violations in the design.

report_wns shows Worst Negative Slack, i.e., the most critical path violation.

<img width="1221" height="624" alt="Screenshot from 2025-10-11 21-36-32" src="https://github.com/user-attachments/assets/009dcc92-0c8a-4eeb-9549-fdf05b91679c" />
<img width="1221" height="800" alt="Screenshot from 2025-10-11 21-42-24" src="https://github.com/user-attachments/assets/2a40f35a-390a-4a3f-a20a-aa421ad08017" />
<img width="1221" height="800" alt="Screenshot from 2025-10-11 21-43-32" src="https://github.com/user-attachments/assets/a04fa8b4-7fca-4e78-b2ed-afc61832c761" />


🌟 Week 3 Conclusion: Post Synthesis GLS & STA Fundamentals

This week, I explored the core flow of digital timing verification — right from logic validation to timing closure. ⚙️

🔹 Part 1 – Post Synthesis GLS:
I successfully performed Gate-Level Simulation (GLS) after synthesis to verify the functional equivalence between RTL and synthesized netlist. This helped ensure that the design behaves accurately even after technology mapping. 🧩

🔹 Part 2 – Fundamentals of STA:
I gained a solid understanding of Static Timing Analysis (STA) — analyzing setup, hold, and slack parameters 🕒 to ensure reliable timing performance. I also learned how STA ensures design stability without requiring dynamic simulation. 💡

🔹 Part 3 – Generate Timing Graphs with OpenSTA:
I executed timing graph generation using OpenSTA, visualizing timing paths and slack values. 📈 This gave me hands-on exposure to reading timing reports, setup/hold paths, and understanding delay propagation.

✨ Overall, Week 3 strengthened my grasp on post-synthesis verification and timing analysis fundamentals, building a strong foundation for physical design and signoff stages. 💻🔧

</details>
</details>

<details>
	<summary>Week 4 - CMOS Circuit Design (sky130-style) </summary>
	
**🧠 1️⃣ What is CMOS?**

CMOS stands for Complementary Metal-Oxide-Semiconductor.
It’s a technology used to build integrated circuits (ICs) — like microprocessors 🧩, microcontrollers ⚙️, memory chips 💾, and other digital logic circuits 🔢.

👉 The word “complementary” refers to using both NMOS (n-type MOSFET) and PMOS (p-type MOSFET) transistors in a pair.

**⚙️ 2️⃣ Basic Principle**

CMOS circuits use complementary and symmetrical pairs of transistors:

NMOS transistor → conducts when input is logic 1 (high voltage) ⚡

PMOS transistor → conducts when input is logic 0 (low voltage) 💤

💡 When one is ON, the other is OFF — ensuring low power consumption 🔋.

**🔍 3️⃣ CMOS Inverter (The Heart 💚 of CMOS Logic)**

The CMOS inverter is the simplest and most important CMOS circuit.

🧩 Structure:

PMOS connected between VDD (power supply) 🔋 and output.

NMOS connected between output and GND (ground) ⬇️.

Input is common to both gates 🌀.

🧮 Operation:

Input	PMOS	NMOS	Output
0 (Low) 💤	ON ✅	OFF ❌	1 (High) ⚡
1 (High) ⚡	OFF ❌	ON ✅	0 (Low) 💤

💥 Output = NOT(Input) → This forms a logic inverter.

**🔋 4️⃣ Power Consumption**

CMOS is famous for its low static power consumption 🧊 because:

Current flows only during switching (when output changes from 0 to 1 or 1 to 0).

No current flows when the circuit is stable (both transistors not ON together).

🔋 Power = Dynamic (Switching) Power + Leakage Power

**⚡ 5️⃣ Advantages**

✨ Low power consumption – Ideal for battery-operated devices 🔋
✨ High noise immunity – Stable even with electrical noise 💪
✨ High packing density – More transistors per chip 🔩
✨ Wide voltage range – Can operate at different supply voltages ⚙️
✨ Scalability – CMOS technology keeps shrinking (Moore’s law 📉)

**🧨 6️⃣ Disadvantages**

⚠️ Slower than NMOS-only circuits in older designs
⚠️ Susceptible to damage by static electricity ⚡ (ESD sensitive)
⚠️ Leakage current increases in modern deep-submicron technology 🧬

**🧩 7️⃣ Applications**

📱 Microprocessors
💾 Memory (RAM, ROM)
🧠 Digital logic gates and ICs
📷 CMOS image sensors
⌚ Low-power embedded systems

**🧬 8️⃣ CMOS Fabrication Steps (Simplified)**

Start with Silicon Wafer 🪨

Oxidation – Grow SiO₂ layer 🌫️

Photolithography – Transfer patterns using light 🕶️

Etching – Remove unwanted oxide 🔪

Doping – Add impurities for NMOS/PMOS regions ⚗️

Metal deposition – Add interconnections 🧲

Packaging – Final IC ready 🧠✨

**🧠 9️⃣ Key Concepts**

Threshold Voltage (Vth): Minimum voltage to turn ON a MOSFET ⚙️

Propagation Delay (tp): Time taken for signal change ⏱️

Noise Margin: Ability to tolerate unwanted voltage noise 📶

**💫 🔟 CMOS vs Other Technologies**
Feature	CMOS	NMOS	TTL
Power	🔋 Very low	⚡ Higher	🔥 High
Speed	🚀 High	⚡ Moderate	🐢 Slow
Noise immunity	💪 Strong	👎 Weak	👍 Medium
Fabrication complexity	🔧 Moderate	🔧 Simple	🧩 Different

<details>
<summary>🧠 Introduction / Background</summary>

Complementary Metal-Oxide-Semiconductor (CMOS) technology 🧩 is the foundation of almost all modern digital integrated circuits — from microprocessors 💻 to memory chips 💾. CMOS devices consist of both NMOS (n-type MOSFET) and PMOS (p-type MOSFET) transistors, working together to perform logic operations efficiently ⚡ with very low static power consumption 🔋.

To understand and analyze CMOS behavior, several key experiments are performed in the lab. Each experiment focuses on different aspects of the transistor and circuit performance. Let’s explore their purpose one by one 👇

**1️⃣ ID–VDS Characteristics (Output Characteristics) 📉**

Purpose:
This experiment studies how the drain current (ID) varies with drain-to-source voltage (VDS) for different values of gate voltage (VGS).

Why it’s done:

To identify different regions of operation of a MOSFET:

Cutoff region: transistor OFF 🚫

Linear (ohmic) region: acts like a variable resistor ⚙️

Saturation region: acts like a current source 🔋

To extract key device parameters like threshold voltage (VTH), mobility, and channel length modulation (λ).

To understand transistor behavior in analog and digital switching conditions.

In short: ID–VDS helps visualize how the MOSFET conducts current under different biases ⚡ — crucial for designing amplifiers and digital gates.

**2️⃣ ID–VGS Characteristics (Transfer Characteristics) 🔄**

Purpose:
To study how the drain current (ID) changes with the gate-to-source voltage (VGS) while keeping VDS constant.

Why it’s done:

Helps determine threshold voltage (VTH) accurately — the point where the transistor just starts to conduct 🚦.

Shows the transconductance (gm), which measures how effectively the gate voltage controls the drain current.

Important for understanding switching speed and gain in CMOS logic circuits.

In short: ID–VGS gives insight into the input control behavior of the MOSFET — how “strongly” a transistor turns ON or OFF ⚙️.

**3️⃣ Voltage Transfer Characteristics (VTC) of CMOS Inverter 🔁**

Purpose:
To plot the output voltage (Vout) versus input voltage (Vin) for a CMOS inverter.

Why it’s done:

Helps analyze switching behavior of digital gates 🧠.

Identifies key points like:

Noise margins (how tolerant the circuit is to noise) 📶

Transition voltage (VM) (where output switches sharply) ⚡

Logic levels (VOH, VOL) for proper logic ‘1’ and ‘0’.

Demonstrates how both NMOS and PMOS transistors complement each other for low static power consumption 🔋.

In short: The VTC curve represents the heart of digital logic — showing how a CMOS inverter flips signals with high noise immunity and minimal power use 💪.

**4️⃣ Static and Dynamic Power Dissipation 🔥**

Purpose:
To measure how much power is consumed by CMOS circuits in both steady (static) and switching (dynamic) states.

Why it’s done:

Static power = leakage current when the circuit is idle 💤

Dynamic power = charging/discharging of load capacitances during switching ⚙️

Helps optimize circuits for low-power design, crucial in portable electronics 📱 and IoT devices 🌐.

In short: This experiment shows why CMOS is energy-efficient — consuming almost no power when idle and scaling well with voltage/frequency ⚡.

**5️⃣ Delay and Switching Speed ⏱️**

Purpose:
To analyze how fast a CMOS inverter or logic gate responds when input changes.

Why it’s done:

Measures propagation delay (tp) and rise/fall times (tr, tf).

Helps evaluate circuit speed and timing performance ⏩.

Crucial for designing high-frequency and high-speed digital systems.

In short: Delay analysis reveals how quickly CMOS logic transitions between logic ‘0’ and ‘1’ — key for modern GHz processors ⚙️💨.

<img width="1219" height="794" alt="Screenshot from 2025-10-13 19-29-50" src="https://github.com/user-attachments/assets/926a124f-c70a-483c-96a4-8e918a44de92" />

</details>

<details> 
<summary> SPICE Netlists / Code </summary>

**🧾 What is a SPICE Netlist?**

A SPICE (Simulation Program with Integrated Circuit Emphasis) netlist is a text-based description 📝 of an electronic circuit.
It contains:

The circuit elements (transistors, resistors, capacitors, etc.)

Their connections (nodes) 🔌

The models for devices (like NMOS, PMOS)

The simulation commands to analyze the circuit 📊

SPICE simulations help us study how CMOS circuits behave before actually fabricating them 🧪💡.

**🔋 1️⃣ ID–VDS Characteristics (Output Characteristics)**

This experiment studies how drain current (ID) varies with drain–source voltage (VDS) for different gate voltages (VGS).

🧠 Purpose:

To identify the transistor’s operating regions: cutoff, linear, and saturation 🚦.

🧩 SPICE Netlist Example:
```
* CMOS NMOS ID-VDS Characteristics ⚡
M1  D G S B  NMOS_MODEL  L=180n  W=1u
VGS G S DC 1.0
VDS D S DC 0
VBS B S DC 0

* Sweep VDS from 0 to 2V for multiple VGS values
.DC VDS 0 2 0.05 SWEEP VGS 0 2 0.5

* NMOS Model Parameters
.MODEL NMOS_MODEL NMOS (LEVEL=1 VTO=0.7 KP=120e-6 LAMBDA=0.05)

* Output command
.PRINT DC ID(M1)
.END
```

📈 This simulation plots ID vs VDS for several VGS values — showing how current increases and saturates as VDS increases.

**⚡ 2️⃣ ID–VGS Characteristics (Transfer Characteristics)**
🧠 Purpose:

To analyze how drain current (ID) varies with gate voltage (VGS) at a constant VDS.

🧩 SPICE Netlist Example:

```
* NMOS ID-VGS Characteristics 🔄
M1 D G S B NMOS_MODEL L=180n W=1u
VDS D S DC 1.0
VBS B S DC 0

* Sweep VGS from 0V to 2V
.DC VGS 0 2 0.05

* NMOS Model
.MODEL NMOS_MODEL NMOS (LEVEL=1 VTO=0.7 KP=120e-6 LAMBDA=0.05)

.PRINT DC ID(M1)
.END
```

📊 This gives the ID vs VGS curve, helping determine the threshold voltage (VTH) and transconductance (gm) 🧮.

**🔁 3️⃣ CMOS Inverter – Voltage Transfer Characteristics (VTC)**
🧠 Purpose:

To plot the output voltage (Vout) versus input voltage (Vin) for a CMOS inverter — the most basic logic gate 🔀.

🧩 SPICE Netlist Example:

```
* CMOS Inverter VTC Simulation 🧠
M1 out in vdd vdd PMOS_MODEL L=180n W=2u
M2 out in 0   0   NMOS_MODEL L=180n W=1u

VDD vdd 0 DC 1.8
VIN in 0 DC 0

* Sweep input voltage
.DC VIN 0 1.8 0.01

.MODEL NMOS_MODEL NMOS (LEVEL=1 VTO=0.7 KP=120e-6 LAMBDA=0.05)
.MODEL PMOS_MODEL PMOS (LEVEL=1 VTO=-0.7 KP=50e-6 LAMBDA=0.05)

.PRINT DC V(in) V(out)
.PLOT DC V(out) vs V(in)
.END
```

📈 The output will be a sharp transition — showing logic inversion and identifying key points like VM, noise margins, and VOH/VOL 🧭.

**⏱️ 4️⃣ Transient Analysis (Switching Behavior)**
🧠 Purpose:

To study how fast a CMOS inverter switches when the input changes over time — i.e., propagation delay, rise/fall time ⏩.

🧩 SPICE Netlist Example:
```
* CMOS Inverter Transient Simulation ⏱️
M1 out in vdd vdd PMOS_MODEL L=180n W=2u
M2 out in 0   0   NMOS_MODEL L=180n W=1u

VDD vdd 0 DC 1.8
VIN in 0 PULSE(0 1.8 0n 100p 100p 5n 10n)

.TRAN 0.1n 20n

.MODEL NMOS_MODEL NMOS (LEVEL=1 VTO=0.7 KP=120e-6 LAMBDA=0.05)
.MODEL PMOS_MODEL PMOS (LEVEL=1 VTO=-0.7 KP=50e-6 LAMBDA=0.05)

.PRINT TRAN V(in) V(out)
.PLOT TRAN V(in) V(out)
.END
```

📊 This simulation shows how quickly the inverter responds when Vin toggles — allowing measurement of delay, rise/fall time, and power switching behavior ⚡.

**🌡️ 5️⃣ Parameter / Process Variation Simulation**
🧠 Purpose:

To analyze how device parameter variations (like threshold voltage or mobility) affect circuit performance 🧬 — crucial for reliability and yield in manufacturing 🏭.

🧩 SPICE Netlist Example:

```
* CMOS Inverter with Variation 🧪
M1 out in vdd vdd PMOS_MODEL L=180n W=2u
M2 out in 0   0   NMOS_MODEL L=180n W=1u

VDD vdd 0 DC 1.8
VIN in 0 DC 0

.DC VIN 0 1.8 0.01 SWEEP PARAM VTO_N 0.6 0.8 0.05

.MODEL NMOS_MODEL NMOS (LEVEL=1 VTO={VTO_N} KP=120e-6 LAMBDA=0.05)
.MODEL PMOS_MODEL PMOS (LEVEL=1 VTO=-0.7 KP=50e-6 LAMBDA=0.05)

.PRINT DC V(in) V(out)
.END
```


📈 This shows how shifting threshold voltage (VTO) changes the switching point and VTC curve, helping understand process tolerance 🧮.

<img width="1220" height="796" alt="Screenshot from 2025-10-16 22-06-43" src="https://github.com/user-attachments/assets/4ae050f1-f4e9-49ac-ac4a-f2e507eafbee" />
<img width="1220" height="796" alt="Screenshot from 2025-10-16 22-01-39" src="https://github.com/user-attachments/assets/0775404c-7b4c-444d-9c80-27cfcb5c930f" />
<img width="1219" height="794" alt="Screenshot from 2025-10-13 19-23-19" src="https://github.com/user-attachments/assets/8307d5d3-db2f-4908-a9b9-27e8225e377d" />
<img width="1219" height="794" alt="Screenshot from 2025-10-13 19-25-37" src="https://github.com/user-attachments/assets/8f450518-4342-4207-9f6f-eff931ae2ca3" />
<img width="1219" height="794" alt="Screenshot from 2025-10-13 19-20-54" src="https://github.com/user-attachments/assets/235e410a-8173-4ed1-b15c-560a4da3e0fe" />
<img width="1219" height="794" alt="Screenshot from 2025-10-13 19-21-39" src="https://github.com/user-attachments/assets/c1a6c496-7433-4b14-a4dc-962fb4f4f80f" />
<img width="1222" height="800" alt="Screenshot from 2025-10-17 19-39-56" src="https://github.com/user-attachments/assets/d0c2c3a2-946a-4408-b582-1393ad347b78" />
<img width="1222" height="800" alt="Screenshot from 2025-10-17 19-41-02" src="https://github.com/user-attachments/assets/30ec89bb-32c8-4596-8779-b7a1394f7a90" />
<img width="1222" height="800" alt="Screenshot from 2025-10-17 19-36-45" src="https://github.com/user-attachments/assets/924f00d5-d1e9-4aef-80b0-ff1faec22dd2" />
<img width="1222" height="800" alt="Screenshot from 2025-10-17 19-34-33" src="https://github.com/user-attachments/assets/5b1d95dc-c5dd-4fe5-8b59-990e09c43a6a" />
<img width="1222" height="800" alt="Screenshot from 2025-10-17 19-32-10" src="https://github.com/user-attachments/assets/fac95c7e-9b5c-498f-b511-8f471f49c267" />
<img width="1222" height="800" alt="Screenshot from 2025-10-17 19-29-52" src="https://github.com/user-attachments/assets/906f82c8-1c01-4731-8d56-e383a611ca18" />
<img width="1222" height="800" alt="Screenshot from 2025-10-17 19-26-12" src="https://github.com/user-attachments/assets/2f3fb45e-f23a-42ae-8d1f-8b6056eefb73" />
</details>

<details>
<summary>Plots & Figures</summary>

**🔋 1️⃣ ID vs VDS Characteristics (Output Characteristics)**

🧠 Purpose:
To analyze how the drain current (ID) varies with drain–source voltage (VDS) for different gate voltages (VGS).

📈 Graph:
Plot — ID (y-axis) vs VDS (x-axis) for multiple VGS values (e.g., 0.5 V, 1.0 V, 1.5 V, 2.0 V).

🔍 Key Observations:

At low VDS, the MOSFET operates in the linear (ohmic) region ⚙️.

As VDS increases, the curve flattens, indicating saturation 🚀.

Higher VGS → higher ID due to stronger inversion (more carriers).

📝 Annotations (mark these on the plot):

📍 Saturation point: where the curve starts to flatten.

⚡ Linear region: initial rising part of the curve.

💤 Cutoff: ID ≈ 0 when VGS < VTH.

✏️ Label VGS values on each curve for clarity.

**🔄 2️⃣ ID vs VGS Characteristics (Transfer Characteristics)**

🧠 Purpose:
To observe how ID changes with VGS (at constant VDS) — showing how the MOSFET “turns ON” and “OFF.”

📈 Graph:
Plot — ID (y-axis) vs VGS (x-axis).

🔍 Key Observations:

Below threshold voltage (VTH) → ID ≈ 0 (OFF region) 🚫.

Above VTH → ID increases rapidly (saturation/ON region) ⚡.

The curve is exponential at first, then quadratic.

📝 Annotations:

⚙️ Threshold voltage (VTH): mark the point where ID starts rising.

📐 Label subthreshold slope (for small ID region).

🔋 Indicate ON and OFF regions clearly.

**🔁 3️⃣ CMOS Inverter – Voltage Transfer Characteristics (VTC)**

🧠 Purpose:
To study how the output voltage (Vout) varies with input voltage (Vin) — the fundamental switching behavior of a CMOS inverter 🔀.

📈 Graph:
Plot — Vout (y-axis) vs Vin (x-axis).

🔍 Key Observations:

Vout ≈ VDD when Vin < VM (switching point) → PMOS ON, NMOS OFF 🔋.

Vout ≈ 0V when Vin > VM → NMOS ON, PMOS OFF ⚡.

Sharp transition between high and low → strong switching performance.

📝 Annotations:

🎯 Switching point (VM): mark where Vin = Vout (midpoint).

📍 Noise Margins:

NMH (Noise Margin High) → VOH - VIH

NML (Noise Margin Low) → VIL - VOL

✏️ Label VOH, VOL, VIL, VIH, and VM.

✅ Indicate regions: Logic ‘0’ 🟢, Transition Zone ⚙️, Logic ‘1’ 🔴.

**⏱️ 4️⃣ Transient Waveforms (Dynamic Switching Behavior)**

🧠 Purpose:
To observe real-time switching of the CMOS inverter when input toggles — showing how fast it responds ⏩.

📈 Graph:
Plot — Vin(t) and Vout(t) vs Time (t).

🔍 Key Observations:

When Vin rises (0 → 1), Vout falls (1 → 0) — inverter action 🔄.

The delay between Vin and Vout represents propagation delay (tp) ⏱️.

Rise and fall times show transition speed of output edges.

📝 Annotations:

⏱️ tpHL: time delay for output going HIGH → LOW.

⏱️ tpLH: time delay for output going LOW → HIGH.

📍 Rise time (tr) and Fall time (tf).

⚙️ Highlight steady logic levels (VOH, VOL).
<img width="1220" height="796" alt="Screenshot from 2025-10-16 22-03-00" src="https://github.com/user-attachments/assets/18c9290b-7c05-4052-a177-38a60002aedc" />
<img width="1206" height="800" alt="Screenshot from 2025-10-15 20-20-15" src="https://github.com/user-attachments/assets/d95dfc28-af41-454b-9042-5b2267a9ea5c" />
<img width="1206" height="800" alt="Screenshot from 2025-10-15 20-18-14" src="https://github.com/user-attachments/assets/80545eda-3ccb-4020-ae39-ad8183fd1bdc" />
<img width="1222" height="800" alt="Screenshot from 2025-10-17 19-38-12" src="https://github.com/user-attachments/assets/23a092ee-36f4-4ec1-bf12-453047d181a6" />
<img width="1222" height="800" alt="Screenshot from 2025-10-17 19-32-39" src="https://github.com/user-attachments/assets/72b07d83-b924-4e91-a14d-0e7bfff99d6f" />
<img width="1222" height="800" alt="Screenshot from 2025-10-17 19-29-28" src="https://github.com/user-attachments/assets/2811bc61-3d0b-44fa-b7f2-29b076ab00f2" />

</details>

<details>
	<summary>Tabulated Results</summary>

**🧾 CMOS Summary Table Overview**

Your CMOS inverter characterization experiment gives you key electrical parameters that describe how well your circuit switches and how reliable it is under noise and variations. These are typically tabulated like this 👇:

Parameter	Symbol	Typical Unit	Description
Threshold Voltage	V<sub>th</sub>	V	Voltage where MOSFET turns ON
Rise Propagation Delay	t<sub>pLH</sub>	ns	Time for output to rise (LOW→HIGH)
Fall Propagation Delay	t<sub>pHL</sub>	ns	Time for output to fall (HIGH→LOW)
Low Noise Margin	NM<sub>L</sub>	V	Margin for logic LOW noise immunity
High Noise Margin	NM<sub>H</sub>	V	Margin for logic HIGH noise immunity
Switching Point	V<sub>M</sub>	V	Input voltage where output = input
Variation Effects	—	—	How above parameters change with V<sub>DD</sub>, T, or device size

**⚡ 1️⃣ Extracted Threshold Voltage (V<sub>th</sub>)**

Meaning:
👉 The threshold voltage is the minimum gate voltage (V<sub>GS</sub>) needed to turn ON the MOSFET and allow current to flow from drain to source.

How it’s found:
🔍 From the Id–Vgs curve, you plot drain current (I<sub>D</sub>) vs gate voltage (V<sub>GS</sub>).
The point where I<sub>D</sub> starts to increase rapidly is V<sub>th</sub>.

CMOS context:

For NMOS, V<sub>th,n</sub> ≈ +0.4–0.7 V

For PMOS, V<sub>th,p</sub> ≈ −0.4–−0.7 V

Importance:
⚙️ Determines switching threshold, static power, and noise margins.
Too low → leakage 🔥
Too high → slow switching 🐢

**⏱️ 2️⃣ Rise / Fall Propagation Delays**

Meaning:
Propagation delay (t<sub>p</sub>) measures how fast the inverter responds to input changes.

t<sub>pLH</sub>: Delay when output goes from LOW → HIGH (rising) ⬆️

t<sub>pHL</sub>: Delay when output goes from HIGH → LOW (falling) ⬇️

How it’s found:
From the transient simulation (Vout vs time):

Measure the time when input reaches 50% of V<sub>DD</sub>.

Measure when output crosses 50% of V<sub>DD</sub>.

Difference = propagation delay 🕒

Interpretation:

Ideally, t<sub>pLH</sub> ≈ t<sub>pHL</sub> for symmetric CMOS.

Smaller delay ⇒ faster circuit ⚡

**📉 3️⃣ Noise Margins (NM<sub>L</sub>, NM<sub>H</sub>)**

Noise margins tell how much noise voltage the circuit can tolerate without logic errors 🧩

VTC (Voltage Transfer Curve) is used — plot V<sub>OUT</sub> vs V<sub>IN</sub>.

Definitions:

NM<sub>L</sub> = V<sub>IL</sub> − V<sub>OL</sub>
(Tolerance for noise on LOW level)

NM<sub>H</sub> = V<sub>OH</sub> − V<sub>IH</sub>
(Tolerance for noise on HIGH level)

How to find:

Mark points where the slope of VTC = −1.

Those input voltages correspond to V<sub>IL</sub> and V<sub>IH</sub>.

Then compute NM<sub>L</sub> and NM<sub>H</sub> 🔍

Importance:

Larger NM = better noise immunity 💪

Balanced noise margins → reliable digital switching ✅

**⚙️ 4️⃣ Effect of Variation (Process, Voltage, Temperature — “PVT”)**

CMOS performance changes due to variations in fabrication or environment 🌡️⚡🏭

Variation Type	Effect
Voltage (V<sub>DD</sub>)	↑ V<sub>DD</sub> → faster switching, larger noise margins; ↓ V<sub>DD</sub> → slower, smaller NM
Temperature (T)	↑ T → mobility ↓ → delay ↑, leakage ↑
Process (W/L)	Wider W → stronger drive → faster; longer L → slower but stable

Impact on Parameters:

Switching Point (V<sub>M</sub>): shifts depending on NMOS/PMOS strength ⚖️

Noise Margins: may shrink at low supply or high temperature 😬

Delays: increase under slow or hot conditions 🐢

**🧮 5️⃣ Example Summary Table (for Report)**
Parameter	Symbol	NMOS/PMOS	Typical Value	Observation
Threshold Voltage	V<sub>th,n</sub> / V<sub>th,p</sub>	+0.55 V / −0.55 V	Extracted from Id–Vgs curve	
Rise Delay	t<sub>pLH</sub>	—	1.8 ns	Output low→high transition
Fall Delay	t<sub>pHL</sub>	—	1.5 ns	Output high→low transition
Low Noise Margin	NM<sub>L</sub>	—	0.6 V	Good LOW-level stability
High Noise Margin	NM<sub>H</sub>	—	0.7 V	Good HIGH-level stability
Switching Voltage	V<sub>M</sub>	—	0.9 V	From VTC midpoint
Variation Effect	—	—	Slight delay ↑, NM ↓ at 85 °C	Typical CMOS PVT behavior

![WhatsApp Image 2025-10-17 at 19 52 46_02739e97](https://github.com/user-attachments/assets/2ffe9469-92e0-4850-b8db-d7018c7130ba)
![WhatsApp Image 2025-10-17 at 19 52 46_8b534e76](https://github.com/user-attachments/assets/e6107e90-b09d-421f-a796-fd6828a0fe4e)
![WhatsApp Image 2025-10-17 at 19 53 48_321f146e](https://github.com/user-attachments/assets/979aca28-3f9d-4322-ba93-a51a7c15f83b)
![WhatsApp Image 2025-10-17 at 19 56 14_bb458c16](https://github.com/user-attachments/assets/80738e69-5b98-499f-96d5-23288e97f303)
</details>

<details>
	<summary>Observations / Analysis</summary>

**Observations & Analysis — per experiment (CMOS) 🔬🪄**

Below I’ve written short, focused discussions for the common CMOS characterization experiments you’ll run (Id–Vgs, Id–Vds, VTC, transient switching, VTC/PVT variations, noise margins, delay vs load). Each entry gives (1) what you see, (2) why it happens — device physics, and (3) how it ties back to STA concepts (delay models, variation, margins) — with emoji highlights for clarity.

**1) Id – Vgs (Transfer / Threshold extraction) 📈�**�

What you see

Low current for small Vgs, then an exponential/subthreshold region, then a rapid rise in Id once Vgs approaches Vth, and eventually a stronger (quadratic/linear) conduction region. ⚡️

Threshold extraction point (e.g., constant-current or linear-extrapolation) visible as the “knee” in the curve. 🎯

Why it happens (device physics)

At low Vgs: channel not formed — only subthreshold diffusion current (exponential w.r.t Vgs). 🧊

As Vgs ≈ Vth: inversion charge forms, channel conduction begins → strong current increase. The threshold is where surface potential enables enough inversion carriers. 🪙

Mobility, velocity saturation, and series resistance shape the slope and the post-threshold region (mobility decreases with vertical field; at high Vgs velocity saturation dominates). 🧲

STA tie-back (delay, variation, margin)

Vth directly influences on-current (Ion) → higher Ion → faster gates (lower delay); lower Ion → slower gates. ⏱️

Leakage in standby comes from subthreshold current — affects static power and noise floor used in STA power models. 🔋

Process variation that shifts Vth (ΔVth) maps to variation in gate delay in STA (fast/typ/slow corners). Use Vth corners when building timing libraries and analyzing worst-case vs best-case. ⚖️

**2) Id – Vds (Output characteristics / Saturation onset) 🛑➡️**

What you see

For small Vds you see a roughly linear (ohmic) region; as Vds increases the slope flattens and current saturates — the saturation region. The boundary (Vds ≈ Vgs − Vth for long-channel) marks saturation onset. 🏁

Why it happens (device physics)

At low Vds: channel is continuous; current limited by channel resistance (linear region). 🧵

At higher Vds: channel “pinches off” near the drain; further increases in Vds don’t linearly increase current — carriers are swept by drift, and velocity saturation or channel-length modulation dictate behavior. 🌪️

Short-channel effects: drain-induced barrier lowering (DIBL) reduces Vth at high Vds causing increased current and a slope even in saturation. 🔍

STA tie-back

Output resistance (ro) and channel-length modulation impact the effective drive and thus the RC delay model — slower than expected if ro low. 🧮

DIBL and Vth roll-off show as variation across PVT corners and worsen worst-case timing (must be included in STA corners). ☁️

For timing libraries: Ion vs Vds behavior determines current-drive in liberty (lib) delay models (e.g., nonlinear current source for Elmore RC). 🧩

**3) Voltage Transfer Curve (VTC) — inverter static characteristic 🔁📊**

What you see

S-shaped Vout vs Vin curve: high output at low Vin, then a steep switching region, then low output at high Vin. The switching point (Vm) is near the midpoint of the transition. The slope in the transition is steep and may cross −1 at two points used for noise margin calculation. 🎢

Why it happens (device physics)

At small Vin: PMOS strongly ON, NMOS OFF → Vout ≈ VDD. At large Vin: NMOS ON, PMOS OFF → Vout ≈ 0. Between: both conduct and the stronger transistor at a given Vin biases the output. The steep region is where both devices are partially on (strong competition). ⚔️

Device sizing (W/L ratio) moves Vm: larger PMOS shifts Vm down, larger NMOS shifts Vm up. 🧷

STA tie-back

Vm relates to logical switching threshold used in STA when mapping to input arrival times and threshold crossing detection (50% VDD or custom Vm). 🎯

Noise margins (NM_L, NM_H) are calculated from VTC and directly affect static timing guard-banding and robustness requirements for interconnect drivers. 🛡️

For standard-cell libraries, VTC-derived thresholds influence the defined input threshold and thus propagation delay measurements in timing characterization. 🧭

**4) Transient switching (rise/fall, load effects) ⏱️🔁**

What you see

Output rises/falls with an RC shape; rise time (t_r) and fall time (t_f) measured between 10–90% or 20–80%. Propagation delays t_pLH and t_pHL measured at 50% crossing. Under heavier capacitive load, delays increase and edges slow. 🕰️🐢

Why it happens (device physics)

Switching is charging/discharging the load capacitance through the device resistance / current source: 

```
𝑡≈𝑅eq𝐶𝐿t≈ReqCL	​

```
. R_eq relates to transistor on-resistance; Ion and mobility set how quickly the capacitor charges. ⚡️➡️🧱

As device enters velocity-saturation, the current saturates and effective R_eq behaves differently than long-channel models — edge rates degrade. Also, rise vs fall asymmetry arises due to different mobility (μn > μp) and W/L sizing. 🧲

STA tie-back

STA uses delay models (e.g., alpha-power law, resistor–capacitor (RC), or effective current sources) that approximate this behavior. Measured t_pLH/t_pHL feed into LUTs for cell delay vs input slope and output load. 🔧

Slew rate (input transition time) affects delay — slower input increases cell delay (captured in nonlinear delay models). 🔁

Load-dependent delay explains why buffer insertion and sizing are used in STA optimization (logical effort, tapering). 🏗️

**5) VTC under variation (PVT: VDD, Temp, Process) 🌡️⚡🏭**

What you see

Lower VDD → VTC compresses and Vm shifts, transitions broaden; higher temperature → mobility drops, slopes soften, currents reduce; process corner (fast/slow devices) shifts Vm and steepness. Variation may reduce NM and increase delay spread. 🎛️

Why it happens (device physics)

VDD scaling reduces available overdrive (Vgs − Vth) → weaker channel charge → lower Ion → slower switching. 🔋

Temperature increases phonon scattering → mobility ↓ → Ion ↓; leakage via subthreshold and junction leakage often increases. 🔥

Process shifts (e.g., threshold shifts, oxide thickness, doping variations) change Vth and mobility directly. 🧪

STA tie-back

STA corners model these behaviors (typ, ss, tt, ff, hot/cold). You must simulate worst-case (slow, low VDD, high T) for setup margin and best-case (fast, high VDD, low T) for hold checks. 🧾

Variation increases timing uncertainty — timing margins and guardbands are set accordingly; statistical STA (SSTA) can quantify probability of timing failure due to correlated variations. 🎲

PVT-induced NM shrinkage may force conservative sizing or timing margins to maintain reliability. 🛡️

**6) Noise Margin experiments (VTC slope & stability) 🧩🔐**

What you see

Two points where dVout/dVin = −1 define VIH and VIL; NM_H and NM_L are the distances from these to VOH/ VOL respectively. Under variation or loading, these margins shrink. ⚠️

Why it happens (device physics)

Noise margin depends on how stiff the rails are (device drive) and how sharply the VTC transitions — steep transition = large margins. Reduced overdrive or increased series resistance flattens the transition and reduces margins. 🪛

STA tie-back

Noise margin values feed into cell library characterizations and determine allowable voltage noise on nets without violating logic levels. They also influence design for reliability, metastability risk analysis, and required regenerative strength in feedback elements. 🔄

In timing, NM interacts with cross-talk and simultaneous switching noise (SSN) — STA tools sometimes include margin offsets to account for IR drop and SSN. 🌊

**7) Delay vs Load / Driving Strength (sizing experiments) 🧮📦**

What you see

Larger device widths → lower delay (better drive), but higher input capacitance; optimum sizing trades off fanout and intrinsic delay. Plot of delay vs load shows diminishing returns with bigger drivers. 📉➡️📈

Why it happens (device physics)

Wider devices increase channel charge capacity → larger Ion → smaller R_eq. But they also add gate capacitance which the previous stage must drive. Physics: more parallel channels → more carriers, less resistive bottleneck. 🧵🧵

STA tie-back

This is the basis of logical effort and tapering in STA-driven optimization: choose stage effort to minimize overall delay. Sizing decisions are encoded in library drive-strength cells. ⚙️

Over-driving increases dynamic power (C·V²·f) — STA-guided synthesis must balance timing vs power. 🔋↕️

**8) Temperature / Leakage experiments (static behavior) 🌡️💧**

What you see

As temperature rises, leakage (subthreshold and junction) increases, static Vout levels may shift slightly, and delay increases. At high temperature leakage might dominate low-power designs. ♨️

Why it happens (device physics)

Thermal energy increases carrier generation and reduces bandgap effective barrier — more carriers even when device “off” → leakage up. Mobility decreases with T → slower switching. 📉

STA tie-back

STA must account for leakage when performing power-aware timing and for IR-drop effects (voltage droop under load causing local VDD reductions). Thermal corners are used to check timing under hot conditions. 🥵

Quick practical checklist — What to report for each experiment ✅📝

Raw plots: Id–Vgs, Id–Vds, VTC, transient Vout/Vin, VTC under PVT.

Extracted numbers: Vth (NMOS/PMOS), Vm, t_pLH, t_pHL, t_r/t_f, NM_L, NM_H, Ion/Ioff, ro.

Conditions: VDD, Temp, W/L, load capacitance — always state exact values. 📌

Observations: saturation onset voltage, subthreshold slope changes, DIBL evidence, Vm shifts.

Short physics explanation: mobility, inversion/channel formation, pinch-off, DIBL, velocity saturation.

STA mapping: which STA corner(s) these correspond to, what happens to propagation delay and margins, recommended guardbands. 🔁

</details>

<details> 
<summary>Conclusions</summary>

**Conclusions — high-level reflections (CMOS) 🧠⚡️**

Below are focused, emoji-rich takeaways that tie transistor physics to real-world timing behavior and show how variation / supply changes affect STA margins and critical paths. Each bullet explains what happens, why it matters, and practical/STA consequences + mitigations.

**1️⃣ How transistor-level behavior constrains timing in real circuits**
• On-current (Ion) sets the pace ⚡→🐢

What — The transistor’s drive current (Ion) determines how fast it can charge/discharge loads, so delay scales roughly like

```
𝑡≈𝐶load⋅𝑉𝐼efft≈IeffCload⋅V
```
​
Why (physics) — Ion depends on mobility (μ), overdrive (Vgs−Vth), channel length, and velocity saturation. In short-channel devices, velocity saturation and series resistances cap current.
STA consequence — Library delay entries and timing arcs reflect effective current; weak Ion → longer propagation delay → paths become critical.
Mitigation — upsizing transistors or adding buffers (logical effort), use cells with higher drive strength, or reduce capacitive load. 🛠️

• Threshold voltage (Vth) is a gatekeeper 🎯

What — Vth controls when a device turns on and how strong it is at a given Vgs.
Why (physics) — Vth shift changes overdrive (Vgs−Vth) and thus Ion exponentially (in subthreshold) and strongly in above-threshold region.
STA consequence — Vth variation maps directly to delay spread across PVT corners (fast/typ/slow). Cells with higher Vth are slower but leak less → timing vs power tradeoff.
Mitigation — pick library corners carefully in STA, use adaptive body bias or multi-Vth libraries for a balanced design. ⚖️

• Mobility & temperature effects — speed slides with T 🌡️

What — Higher temperature reduces mobility → lower Ion → slower transitions.
Why (physics) — Phonon scattering increases with temperature; hence carrier mobility drops.
STA consequence — Hot corners (high T) are often worst-case for setup timing; cold corners may be worst for hold. Include thermal corners in STA.
Mitigation — thermal-aware floorplanning, allocate guardbands, or use timing monitors. 🔥🧊

• Short-channel effects (DIBL, CLM) shift behavior ↗️↘️

What — DIBL lowers apparent Vth at high Vds; channel-length modulation gives finite output resistance (ro).
Why (physics) — Drain field influences barrier at the source; effective channel length is reduced.
STA consequence — Drive current and output resistance vary with operating point → impacts delay and slope of VTC → unexpected critical paths or reduced noise margins.
Mitigation — include short-channel models in characterization; use worst-case corner models (SS/FF) and monte-carlo/SSTA for statistical effects. 🔬

• Wiring & parasitics turn local device speed into global timing 📦➕🔌

What — interconnect RC dominates as technology scales; even very fast transistors can be slowed by long wires and heavy loads.
Why (physics) — longer metal = higher R and C; delay becomes RC-dominated (not just transistor-limited).
STA consequence — Critical path often is a chain of gates + long wires — buffer insertion/tapering is required. Timing closure must model parasitics accurately.
Mitigation — buffer insertion, net sizing, floorplan-aware routing, and accurate extraction for STA. 🧭

**2️⃣ How variation or supply changes affect STA margins & critical paths**
• Supply voltage (VDD) changes are multiplier effects 🔋↕️

What — Lower VDD reduces overdrive (Vgs−Vth), drastically lowering Ion and increasing delay; higher VDD speeds things up (but raises power).
Why (physics) — Ion ∝ (Vgs−Vth)^α (α ≈ 1–2 depending on regime); so small VDD reductions cause big current reductions.
STA consequence — Low-VDD corners (or IR drop) create worst-case timing for setup; a path that’s safe at nominal VDD may fail at droop/low-VDD. Critical path ranking can change with VDD.
Mitigation — Include voltage-droop-aware STA, MCMM runs (multi-corner multi-mode), use on-chip regulation / decoupling, dynamic voltage scaling with margin tracking. ⚙️

• Process variation (global & local) spreads delays — statistical risk 🎲

What — Die-to-die or within-die variation changes Vth, mobility, oxide thickness, etc., producing a distribution of delays.
Why (physics) — Imperfect doping, random dopant fluctuation, and geometric variation change device parameters unpredictably.
STA consequence — Deterministic corners can be pessimistic or optimistic; SSTA (statistical STA) is needed to estimate yield and probability of timing violations. Critical paths may vary per die — a different path can be worst-case in another chip.
Mitigation — use SSTA, guardband for 6σ targets if needed, redundancy/repair, timing-aware placement to reduce sensitivity to variation. 🛡️

• Temperature + VDD + process interact nonlinearly — corners can flip-critical paths 🔄

What — The slowest path at one corner might be non-critical at another (e.g., path A slow at low-VDD, path B slow at high T).
Why (physics) — Device physics affect different gates differently (stack effects, body effect, input slews); interactions change delay slopes.
STA consequence — Must run timing across multiple corners & modes; relying on a single worst-case corner can miss failures. Slack margins must be sized to cover corner interactions.
Mitigation — MCMM timing runs, identify paths with large corner sensitivity, and prioritize optimization where sensitivity is highest. 🔍

• IR drop & simultaneous switching noise (SSN) reduce local VDD — hidden killer ⚠️

What — Current surges cause local supply droop and ground bounce; effective local VDD drops for critical cells.
Why (physics) — Finite resistance in power grid and inductance in package cause voltage transient under large current.
STA consequence — Apparent slow-down of cells, reduced noise margins; can flip timing at full-chip activity though single-cell tests pass.
Mitigation — EM/IR analysis, stronger power grid, decoupling capacitors, on-chip monitors, and IR-aware timing checks. 🧯

**3️⃣ Practical takeaways & recommendations ✅**

Model faithfully: Use characterized library data (delay vs input slew vs output load) including PVT corners and short-channel effects. 📚

Run MCMM & SSTA: Don’t rely on one corner — run multiple corners and statistical analysis to understand yield and margin. 🎲

Target sensitivity hotspots: Identify paths with high sensitivity to Vth/VDD/T and optimize them first (sizing, buffering, placement). 🎯

Design power/timing trade-offs consciously: Lowering VDD saves power but increases worst-case delay — use adaptive schemes (DVFS) with timing monitors. ⚖️

Plan for IR/SSN: Add timing margin and IR-aware STA, strengthen PDN (power delivery network). 🧩

Use on-chip monitoring: Ring-oscillators, critical-path monitors, or shadow registers help adapt to runtime variation. 🔬
</details>

<details>

<summary>References / Citations</summary>

**📖 References / Citations (with respect to SKY130)**

**🧩 1️⃣ SkyWater SKY130 PDK**

Source:
👉 SkyWater Technology Foundry + Google Open-Source PDK Repository
🔗 GitHub: https://github.com/google/skywater-pdk

About:

🌍 The Sky130 process is a 130 nm CMOS technology node developed by SkyWater Technology and made open-source through collaboration with Google.

🧠 It provides transistor-level SPICE models, standard-cell libraries, device layouts, and characterization data for both NMOS and PMOS devices.

⚙️ Includes device corners (TT, SS, FF, FS, SF), BSIM3v3 models, and parasitic extraction rules, enabling accurate simulation and timing analysis.

🧪 Used for analog, digital, and mixed-signal circuit characterization and education — ideal for understanding CMOS behavior such as VTC, Id–Vds, and delay.

🛠️ Model files: typically located under

skywater-pdk/libraries/sky130_fd_pr/latest/models/


with sub-models like sky130_fd_pr__nfet_01v8 and sky130_fd_pr__pfet_01v8.

In your report:

SPICE simulations and transistor characteristics were derived using SkyWater SKY130 PDK models (sky130_fd_pr__nfet_01v8 and sky130_fd_pr__pfet_01v8) to ensure realistic CMOS behavior for Id–Vgs, Id–Vds, and inverter VTC experiments.

**📗 2️⃣ BSIM3v3 Compact Model (Used inside SKY130)**

Source:

Berkeley Short-Channel IGFET Model (BSIM3v3), UC Berkeley Device Group

About:

🧬 It’s the industry-standard physics-based MOSFET model used for short-channel devices in sub-micron CMOS.

🧮 Accurately models threshold voltage roll-off, velocity saturation, DIBL, and mobility degradation.

🧠 SKY130 PDK uses BSIM3v3 parameters within its .model cards to represent device I–V and C–V behavior in SPICE simulations.

In your report:

Device modeling followed BSIM3v3 equations embedded in the SKY130 SPICE decks, ensuring realistic capture of transistor-level effects for timing and noise analysis.

**📘 3️⃣ Open-Source Toolchain References**

You may list the tools used for simulation and STA (if applicable):

⚡ ngspice / Xyce — for SPICE simulations of Id–Vds, Id–Vgs, and inverter transient behavior.

⏱️ OpenSTA — for static timing analysis, delay extraction, and timing-graph understanding.

🧩 Magic / Klayout / OpenROAD — for layout visualization, parasitic extraction, and PVT analysis in the open-source flow.

In your report:

Simulations were performed using open-source tools such as ngspice for circuit characterization and OpenSTA for timing interpretation, with device parameters sourced from SkyWater SKY130 PDK.

**📙 4️⃣ Supporting Literature / References**

📕 “CMOS Digital Integrated Circuits: Analysis and Design” — Sung-Mo (Steve) Kang, Yusuf Leblebici.

📘 “Digital Integrated Circuits: A Design Perspective” — Jan M. Rabaey et al.

📗 BSIM3v3 MOSFET Model User’s Manual — UC Berkeley Device Group.

📙 SkyWater SKY130 Open-Source PDK Documentation — https://skywater-pdk.readthedocs.io
	
</details>

🧠 Conclusion – Week 4: CMOS Circuit Design (sky130-style)

This week’s work using Sky130 🌿 showed how transistor physics directly drives circuit timing 🕒.
By simulating NMOS/PMOS behavior ⚡, extracting Vt (~0.43 V) 🎯, plotting VTC ⚙️, and measuring delays (~60–70 ps) ⏱️, I saw how device properties shape STA concepts like slack, delay, and noise margins 📊.
Varying VDD and W/L revealed clear effects on switching point, noise margin, and delay 🔄 — proving that real CMOS variation defines timing reliability in digital design 💡.

</details>

<details>

<summary>Week 5 - OpenROAD Flow Setup and Floorplan + Placement</summary>

<details>
	<summary> Introduction to OpenRoad </summary>
🎯 Objective

You are going to:
✅ Set up the OpenROAD Flow Scripts environment 🧰
✅ Run the Floorplan 🧩 and Placement 📦 stages of the physical design flow.

🧠 Big Picture

You’re moving from transistor-level design (SPICE) ⚡ — where you work on individual transistors and circuits —
to backend implementation 🏗️ — where you take a logic design (Verilog) 💻 and turn it into a physical chip layout 🪶.

🧩 Stage 1: Floorplanning

🗺️ What it means:
You decide how the chip will be organized —
like planning where rooms go in a house before you build it 🏠.

In chip terms:

Define the chip core area 🧱

Place macros (big blocks like memory or analog parts) 🧊

Reserve areas for power, clock, and routing ⚡

Set up IO pins (inputs/outputs) 🔌

Goal: Create a clean, efficient layout foundation for the rest of the design 🧭

📦 Stage 2: Placement

🚧 What it means:
Now you start placing the smaller “standard cells” (like AND, OR, flip-flops) inside the floorplan.

It’s like taking furniture 🪑 and arranging it perfectly inside your house 🏡.

In chip terms:

Place each logic cell in the defined area 🧮

Optimize placement for timing, power, and area ⚙️

Make sure no cells overlap, and signals can route efficiently 🛣️

Goal: Arrange everything so the chip can later be routed and work fast and reliably ⚡

🧭 Transition Summary

🔬 Before (SPICE): You designed circuits transistor-by-transistor — the microscopic view.
🏗️ Now (OpenROAD Flow): You’re automating the creation of a full chip layout from logic — the macroscopic view.

## OpenROAD installation guide

### 📚 Contents

  - [Steps to Install OpenROAD and Run GUI](#steps-to-install-openroad-and-run-gui)
    - [1. Clone the OpenROAD Repository](#1-clone-the-openroad-repository)
    - [2. Run the Setup Script](#2-run-the-setup-script)
    - [3. Build OpenROAD](#3-build-openroad)
    - [4. Verify Installation](#4-verify-installation)
    - [5. Run the OpenROAD Flow](#5-run-the-openroad-flow)
    - [6. Launch the GUI](#6-launch-the-graphical-user-interface-gui-to-visualize-the-final-layout)
- [ORFS Directory Structure and File Formats](#orfs-directory-structure-and-file-formats)


**OpenROAD** is an open-source, fully automated RTL-to-GDSII flow for digital integrated circuit (IC) design. It supports synthesis, floorplanning, placement, clock tree synthesis, routing, and final layout generation. OpenROAD enables rapid design iterations, making it ideal for academic research and industry prototyping.

OpenROAD provides [OpenROAD-flow-scripts](https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts)
as a native, ready-to-use prototyping and tapeout flow. However,
it also enables the creation of any custom flow controllers based
on the underlying tools, database and analysis engines. Please refer to the flow documentation [here](https://openroad-flow-scripts.readthedocs.io/en/latest/).

OpenROAD-flow-scripts (ORFS) is a fully autonomous, RTL-GDSII flow
for rapid architecture and design space exploration, early prediction
of QoR and detailed physical design implementation. However, ORFS
also enables manual intervention for finer user control of individual
flow stages through Tcl commands and Python APIs.

Figure below shows the main stages of the OpenROAD-flow-scripts:

```mermaid
%%{init: { 'logLevel': 'debug', 'theme': 'dark'
  } }%%
timeline
  title RTL-GDSII Using OpenROAD-flow-scripts
  Synthesis
    : Inputs  [RTL, SDC, .lib, .lef]
    : Logic Synthesis  (Yosys)
    : Output files  [Netlist, SDC]
  Floorplan
    : Floorplan Initialization
    : IO placement  (random)
    : Timing-driven mixed-size placement
    : Macro placement
    : Tapcell and welltie insertion
    : PDN generation
  Placement
    : Global placement without placed IOs
    : IO placement  (optimized)
    : Global placement with placed IOs
    : Resizing and buffering
    : Detailed placement
  CTS : Clock Tree Synthesis
    : Timing optimization
    : Filler cell insertion
  Routing
    : Global Routing
    : Detailed Routing
  Finishing
    : Metal Fill insertion
    : Signoff timing report
    : Generate GDSII  (KLayout)
    : DRC/LVS check (KLayout)
```

Here are the main steps for a physical design implementation
using OpenROAD;

- `Floorplanning`
  - Floorplan initialization - define the chip area, utilization
  - IO pin placement (for designs without pads)
  - Tap cell and well tie insertion
  - PDN- power distribution network creation
- `Global Placement` 
  - Macro placement (RAMs, embedded macros)
  - Standard cell placement
  - Automatic placement optimization and repair for max slew,
    max capacitance, and max fanout violations and long wires
- `Detailed Placement`
  - Legalize placement - align to grid, adhere to design rules
  - Incremental timing analysis for early estimates
- `Clock Tree Synthesis` 
  - Insert buffers and resize for high fanout nets
- `Optimize setup/hold timing`
- `Global Routing`
  - Antenna repair
  - Create routing guides
- `Detailed Routing`
  - Legalize routes, DRC-correct routing to meet timing, power
    constraints
- `Chip Finishing`
  - Parasitic extraction using OpenRCX
  - Final timing verification
  - Final physical verification
  - Dummy metal fill for manufacturability
  - Use KLayout or Magic using generated GDS for DRC signoff

### GUI

The OpenROAD GUI is a powerful visualization, analysis, and debugging
tool with a customizable Tcl interface. The below figures show GUI views for
various flow stages including floorplanning, placement congestion,
CTS and post-routed design.
</details>

<details>
	<summary> Install OpenROAD Flow Scripts </summary>

### `Steps to Install OpenROAD and Run GUI`

### 1. Clone the OpenROAD Repository

```bash
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
cd OpenROAD-flow-scripts
```
<img width="1216" height="800" alt="Screenshot from 2025-10-21 14-09-39" src="https://github.com/user-attachments/assets/d6f468d7-49b4-40b3-a9ae-4ebc88c50b0b" />

### 2. Run the Setup Script

```bash
sudo ./setup.sh
```
<img width="1227" height="800" alt="Screenshot from 2025-10-21 14-23-38" src="https://github.com/user-attachments/assets/bbca94fd-81bb-4ee5-bcdf-4203f21e9958" />
<img width="1227" height="800" alt="Screenshot from 2025-10-21 14-56-03" src="https://github.com/user-attachments/assets/84a46e93-3a4e-49fd-8ad9-dc0a51360f61" />

### 3. Build OpenROAD

```bash
./build_openroad.sh --local
```
<img width="1221" height="794" alt="Screenshot from 2025-10-21 19-16-49" src="https://github.com/user-attachments/assets/b72d676c-ca37-4d05-80d5-9ac6a568e937" />
<img width="1212" height="797" alt="Screenshot from 2025-10-21 21-33-54" src="https://github.com/user-attachments/assets/25881d6f-7da0-4adf-9a8a-8a7ba77864ea" />

### 4. Verify Installation

```bash
source ./env.sh
yosys -help  
openroad -help
```
<img width="1227" height="800" alt="Screenshot from 2025-10-21 14-56-59" src="https://github.com/user-attachments/assets/9ef118e4-0982-4326-9b48-644542644072" />
<img width="1216" height="792" alt="Screenshot from 2025-10-22 14-19-06" src="https://github.com/user-attachments/assets/e950bf7c-6c0c-45f3-8451-6b151e20bcb2" />

### 5. Run the OpenROAD Flow

```bash
cd flow
make
```
<img width="1213" height="800" alt="Screenshot from 2025-10-24 15-52-09" src="https://github.com/user-attachments/assets/e4ddb265-b942-444d-b941-a0c8b895e43e" />

### 6. Launch the graphical user interface (GUI) to visualize the final layout

```bash
 make gui_final
```
<img width="1213" height="800" alt="Screenshot from 2025-10-24 15-53-21" src="https://github.com/user-attachments/assets/5381576b-3854-4ea0-ab63-0e712ca6e052" />

✅ Installation Complete! You can now explore the full RTL-to-GDSII flow using OpenROAD.

### `ORFS Directory Structure and File formats`

OpenROAD-flow-scripts/

```plaintext
├── OpenROAD-flow-scripts             
│   ├── docker           -> It has Docker based installation, run scripts and all saved here
│   ├── docs             -> Documentation for OpenROAD or its flow scripts.  
│   ├── flow             -> Files related to run RTL to GDS flow  
|   ├── jenkins          -> It contains the regression test designed for each build update
│   ├── tools            -> It contains all the required tools to run RTL to GDS flow
│   ├── etc              -> Has the dependency installer script and other things
│   ├── setup_env.sh     -> Its the source file to source all our OpenROAD rules to run the RTL to GDS flow
```
Inside the `flow/` Directory

```plaintext
├── flow           
│   ├── design           -> It has built-in examples from RTL to GDS flow across different technology nodes
│   ├── makefile         -> The automated flow runs through makefile setup
│   ├── platform         -> It has different technology note libraries, lef files, GDS etc 
|   ├── tutorials        
│   ├── util            
│   ├── scripts                 
```
<img width="1213" height="800" alt="Screenshot from 2025-10-24 15-54-07" src="https://github.com/user-attachments/assets/6121ac3e-f7a1-41ff-be56-0432e8f92431" />

</details>

<details>
<summary> Execution of Floorplan and Placement </summary>

### `Floorplan and Placement of VSDBabySoC in OpenROAD`

### 📚 Contents
 - [RTL2GDS Flow for VSDBabySoC: Initial Steps](#rtl2gds-flow-for-vsdbabysoc-initial-steps)
    - [Key Components of config.mk](#key-components-of-configmk)
    - [File Structure After Setup](#file-structure-after-setup)
  - [Run Synthesis](#run-synthesis)
    - [Synthesis Netlist](#synthesis-netlist)
    - [Synthesis Log](#synthesis-log)
    - [Synthesis Check](#synthesis-check)
    - [Synthesis Stats](#synthesis-stats)
  - [Run Floorplan](#run-floorplan)
    - [Floorplan Error and Fix](#floorplan-error-and-fix)
    - [Floorplan Result (GUI)](#floorplan-result-gui)
 -  [Run Placement](#run-placement)
    - [Placement Result (GUI)](#placement-result-gui)

###  `RTL2GDS Flow for VSDBabySoC: Initial Steps`

1. **Create Directories:**
   - Inside `OpenROAD-flow-scripts/flow/designs/sky130hd/`, create a folder named `vsdbabysoc`.
   - Create another folder named `vsdbabysoc` in `OpenROAD-flow-scripts/flow/designs/src/` and place all Verilog files here.

2. **Copy Folders:**
   - From your `VSDBabySoC` folder, copy the following folders into `sky130hd/vsdbabysoc`:
     - **gds:** Contains `avsddac.gds`, `avsdpll.gds`.
     - **include:** Contains `sandpiper.vh`, `sandpiper_gen.vh`, `sp_default.vh`, `sp_verilog.vh`.
     - **lef:** Contains `avsddac.lef`, `avsdpll.lef`.
     - **lib:** Contains `avsddac.lib`, `avsdpll.lib`.

3. **Copy Constraint and Configuration Files:**
   - Copy `vsdbabysoc_synthesis.sdc` into `sky130hd/vsdbabysoc`.
   - Copy `macro.cfg` and `pin_order.cfg` into `sky130hd/vsdbabysoc`.

4. **Create Config File:**
   - Create a `config.mk` file in `sky130hd/vsdbabysoc` with the required configuration details. 

<details> <summary><strong>config.mk</strong></summary>

```
  # Design and Platform Configuration
   export DESIGN_NICKNAME = vsdbabysoc
   export DESIGN_NAME = vsdbabysoc
   export PLATFORM    = sky130hd

  # Design Paths
  export vsdbabysoc_DIR = /home/ashika_gowda/OpenROAD-flow-scripts/flow/designs/sky130hd/$(DESIGN_NICKNAME)

  # Explicitly list Verilog files for synthesis
   export VERILOG_FILES = /home/ashika_gowda/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc/vsdbabysoc.v \
                         /home/ashika_gowda/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc/rvmyth.v \
                         /home/ashika_gowda/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc/clk_gate.v


  # Include Directory for Verilog Header Files
   export VERILOG_INCLUDE_DIRS = $(vsdbabysoc_DIR)/include

  # Constraints File
    export SDC_FILE = $(vsdbabysoc_DIR)/vsdbabysoc_synthesis.sdc

  # Additional GDS Files
    export ADDITIONAL_GDS = $(vsdbabysoc_DIR)/gds/avsddac.gds \
                            $(vsdbabysoc_DIR)/gds/avsdpll.gds

  # Additional LEF Files
   export ADDITIONAL_LEFS = $(vsdbabysoc_DIR)/lef/avsddac.lef \
                            $(vsdbabysoc_DIR)/lef/avsdpll.lef

  # Additional LIB Files
   export ADDITIONAL_LIBS = $(vsdbabysoc_DIR)/lib/avsddac.lib \
                            $(vsdbabysoc_DIR)/lib/avsdpll.lib

 # Pin Order and Macro Placement Configurations
   export FP_PIN_ORDER_CFG = $(vsdbabysoc_DIR)/pin_order.cfg
   export MACRO_PLACEMENT_CFG = $(vsdbabysoc_DIR)/macro.cfg

 # Clock Configuration
   export CLOCK_PORT = CLK
   export CLOCK_NET  = $(CLOCK_PORT)
   export CLOCK_PERIOD = 20.0

# Floorplanning Configuration
  export DIE_AREA   = 0 0 1600 1600
  export CORE_AREA  = 20 20 1590 1590

# Placement Configuration
  export PLACE_PINS_ARGS = -exclude left:0-600 -exclude left:1000-1600 -exclude right:* -exclude top:* -exclude bottom:*

# Tuning for Timing and Buffers
  export TNS_END_PERCENT     = 100
  export REMOVE_ABC_BUFFERS  = 1
  export CTS_BUF_DISTANCE    = 600
  export SKIP_GATE_CLONING   = 1

 # Magic Tool Configuration
   export MAGIC_ZEROIZE_ORIGIN = 0
   export MAGIC_EXT_USE_GDS    = 1
```
</details>

This script sets up environment variables and configurations for the design and synthesis of a System-on-Chip (SoC) using the OpenROAD flow. The design is based on the "vsdbabysoc" and targets the "sky130hd" platform.

--------

### `Key Components of config.mk`

#### Design and Platform Configuration
- **DESIGN_NICKNAME & DESIGN_NAME**: Both are set to "vsdbabysoc," serving as the identifier for the design project.
- **PLATFORM**: Specifies the technology platform as "sky130hd," indicating the process node and design rules to be used.

#### Design Paths
- **vsdbabysoc_DIR**: Defines the directory path for the design files as `/home/ashika_gowda/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc`. This path is constructed using the DESIGN_NICKNAME variable, ensuring consistency and easy access to design resources.

#### Verilog Files for Synthesis
- **VERILOG_FILES**: Lists the Verilog source files required for synthesis:
  - `/home/ashika_gowda/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc/vsdbabysoc.v`: The main Verilog file for the SoC design.
  - `/home/ashika_gowda/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc/rvmyth.v`: A module within the design, possibly a RISC-V core or related component.
  - `/home/ashika_gowda/OpenROAD-flow-scripts/flow/designs/src/vsdbabysoc/clk_gate.v`: A module for clock gating, used to manage power consumption by controlling clock signals.

#### Verilog Header Files
- **VERILOG_INCLUDE_DIRS**: Specifies the directory for Verilog header files as `/home/ashika_gowda/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/include`.

#### Constraints and Additional Files
- **SDC_FILE**: Points to the constraints file for synthesis located at `/home/ashika_gowda/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/vsdbabysoc_synthesis.sdc`.
- **ADDITIONAL_GDS**: Lists additional GDS files required for the design:
  - `/home/ashika_gowda/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/gds/avsddac.gds`
  - `/home/ashika_gowda/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/gds/avsdpll.gds`
- **ADDITIONAL_LEFS**: Lists additional LEF files:
  - `/home/ashika_gowda/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lef/avsddac.lef`
  - `/home/ashika_gowda/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lef/avsdpll.lef`
- **ADDITIONAL_LIBS**: Lists additional LIB files:
  - `/home/ashika_gowda/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsddac.lib`
  - `/home/ashika_gowda/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/lib/avsdpll.lib`

#### Pin Order and Macro Placement
- **FP_PIN_ORDER_CFG**: Configuration file for pin order located at `/home/ashika_gowda/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/pin_order.cfg`.
- **MACRO_PLACEMENT_CFG**: Configuration file for macro placement located at `/home/ashika_gowda/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/macro.cfg`.

#### Clock Configuration
- **CLOCK_PORT & CLOCK_NET**: Defines the clock port and net as `CLK`.
- **CLOCK_PERIOD**: Sets the clock period to `20.0` units.

#### Floorplanning Configuration
- **DIE_AREA**: Specifies the die area dimensions as `0 0 1600 1600`.
- **CORE_AREA**: Specifies the core area dimensions as `20 20 1590 1590`.

#### Placement Configuration
- **PLACE_PINS_ARGS**: Arguments for pin placement, excluding certain areas on the die:
  - `-exclude left:0-600`
  - `-exclude left:1000-1600`
  - `-exclude right:*`
  - `-exclude top:*`
  - `-exclude bottom:*`

#### Timing and Buffer Tuning
- **TNS_END_PERCENT**: Sets the target negative slack end percentage to `100`.
- **REMOVE_ABC_BUFFERS**: Enables removal of ABC buffers, set to `1`.
- **CTS_BUF_DISTANCE**: Sets the buffer distance for clock tree synthesis to `600`.
- **SKIP_GATE_CLONING**: Skips gate cloning during synthesis, set to `1`.

#### Magic Tool Configuration
- **MAGIC_ZEROIZE_ORIGIN**: Configuration for zeroizing the origin, set to `0`.
- **MAGIC_EXT_USE_GDS**: Configuration for using GDS files, set to `1`.

This setup script is crucial for defining the environment and parameters needed for successful synthesis and layout of the "vsdbabysoc" design on the "sky130hd" platform, ensuring that all necessary files and configurations are in place for the design flow.

### `File Structure After Setup`

```shell
ashika_gowda@ashika_gowda-VirtualBox:~/OpenROAD-flow-scripts/flow$ ls -ltrh designs/src/vsdbabysoc/
total 84K
-rw-rw-r-- 1 ashika_gowda ashika_gowda 1.1K Jun 29 15:50 avsddac.v
-rw-rw-r-- 1 ashika_gowda ashika_gowda  947 Jun 29 15:50 avsdpll.v
lrwxrwxrwx 1 ashika_gowda ashika_gowda   87 Jun 29 15:50 primitives.v -> /home/ashika_gowda/VLSI/sky130RTLDesignAndSynthesisWorkshop/my_lib/verilog_model/primitives.v
-rw-rw-r-- 1 ashika_gowda ashika_gowda 1.7K Jun 29 15:50 clk_gate.v
-rw-rw-r-- 1 ashika_gowda ashika_gowda  17K Jun 29 15:50 rvmyth.v
-rw-rw-r-- 1 ashika_gowda ashika_gowda  19K Jun 29 15:50 rvmyth_gen.v
-rw-rw-r-- 1 ashika_gowda ashika_gowda 2.3M Jun 29 15:50 sky130_fd_sc_hd.v
-rwxrwxr-x 1 ashika_gowda ashika_gowda 1.3K Jun 29 15:50 testbench.v
-rw-rw-r-- 1 ashika_gowda ashika_gowda  603 Jun 29 15:50 testbench.rvmyth.post-routing.v
-rw-rw-r-- 1 ashika_gowda ashika_gowda  590 Jun 29 15:50 vsdbabysoc.v
-rw-rw-r-- 1 ashika_gowda ashika_gowda 743K Jun 29 15:50 vsdbabysoc.synth.v
```

```shell
ashika_gowda@ashika_gowda-VirtualBox:~/OpenROAD-flow-scripts/flow$ ls -ltrh designs/sky130hd/vsdbabysoc/
total 32K
drwxrwxr-x 2 ashika_gowda ashika_gowda 4.0K Jun 29 15:52 gds
drwxrwxr-x 2 ashika_gowda ashika_gowda 4.0K Jun 29 15:52 include
drwxrwxr-x 2 ashika_gowda ashika_gowda 4.0K Jun 29 15:53 lef
-rw-rw-r-- 1 ashika_gowda ashika_gowda   73 Jun 29 15:54 vsdbabysoc_synthesis.sdc
lrwxrwxrwx 1 ashika_gowda ashika_gowda   65 Jun 29 15:55 macro.cfg -> /home/ashika_gowda/VLSI/VSDBabySoC/src/layout_conf/vsdbabysoc/macro.cfg
lrwxrwxrwx 1 ashika_gowda ashika_gowda   69 Jun 29 15:55 pin_order.cfg -> /home/ashika_gowda/VLSI/VSDBabySoC/src/layout_conf/vsdbabysoc/pin_order.cfg
-rw-rw-r-- 1 ashika_gowda ashika_gowda 2.1K Jun 29 15:59 config.mk
drwxrwxr-x 2 ashika_gowda ashika_gowda 4.0K Jun 29 16:06 lib
```
<img width="1213" height="800" alt="Screenshot from 2025-10-24 16-02-36" src="https://github.com/user-attachments/assets/b19b9ae1-7df8-4408-96ae-8fe755bb9aea" />


#### Now go to terminal and run the following commands:

```shell
# Navigate to the OpenROAD flow scripts directory
cd OpenROAD-flow-scripts
# Source the environment setup script
source env.sh
# Change to the flow directory
cd flow
```
----
 
### `Run Synthesis`

```shell
# Ensure you are in the 'flow' directory before running the synthesis command
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk synth
```

This command runs the synthesis process using the specified design configuration file `config.mk` for the `vsdbabysoc` design on the `sky130hd` platform.

<img width="1213" height="800" alt="Screenshot from 2025-10-24 16-06-43" src="https://github.com/user-attachments/assets/6e511e9d-b8bf-481f-af51-baf6fcbf21ff" />
<img width="1213" height="800" alt="Screenshot from 2025-10-24 16-06-14" src="https://github.com/user-attachments/assets/8c0559d5-e279-454d-94aa-4958ece0f404" />


#### Synthesis netlist

```shell
ashika_gowda@ashika_gowda-VirtualBox:~/OpenROAD-flow-scripts/flow$ gvim results/sky130hd/vsdbabysoc/base/1_2_yosys.v
```
<img width="1213" height="800" alt="Screenshot from 2025-10-24 16-09-22" src="https://github.com/user-attachments/assets/2c0fe45d-62b3-4a91-8eda-dac1efd03416" />


#### Synthesis log

```shell
ashika_gowda@ashika_gowda-VirtualBox:~/OpenROAD-flow-scripts/flow$ gvim logs/sky130hd/vsdbabysoc/base/1_1_yosys.log
```
<img width="1213" height="800" alt="Screenshot from 2025-10-24 16-11-02" src="https://github.com/user-attachments/assets/0f68054a-f41c-4d7f-89ec-4dd271d98541" />


#### Synthesis Check

```shell
ashika_gowda@ashika_gowda-VirtualBox:~/OpenROAD-flow-scripts/flow$ gvim reports/sky130hd/vsdbabysoc/base/synth_check.txt
```
<img width="1213" height="800" alt="Screenshot from 2025-10-24 16-11-34" src="https://github.com/user-attachments/assets/06c79199-58d9-4299-85c5-091dfe0901e0" />


#### Synthesis Stats

```shell
ashika_gowda@ashika_gowda-VirtualBox:~/OpenROAD-flow-scripts/flow$ gvim reports/sky130hd/vsdbabysoc/base/synth_stat.txt
```

<details> <summary><strong>synth_stat.txt</strong></summary>

```
20. Printing statistics.

=== vsdbabysoc ===

   Number of wires:               6551
   Number of wire bits:           6551
   Number of public wires:        1285
   Number of public wire bits:    1285
   Number of ports:                  7
   Number of port bits:              7
   Number of memories:               0
    Number of memory bits:            0
   Number of processes:              0
   Number of cells:               6440
     avsddac                         1
     avsdpll                         1
     sky130_fd_sc_hd__a2111o_1       1
     sky130_fd_sc_hd__a2111oi_0      3
     sky130_fd_sc_hd__a2111oi_2      1
     sky130_fd_sc_hd__a211o_1        9
     sky130_fd_sc_hd__a211o_2        1
     sky130_fd_sc_hd__a211oi_1      47
     sky130_fd_sc_hd__a21bo_2        1
     sky130_fd_sc_hd__a21boi_1       1
     sky130_fd_sc_hd__a21o_1        29
     sky130_fd_sc_hd__a21oi_1      816
     sky130_fd_sc_hd__a21oi_2        1
     sky130_fd_sc_hd__a221o_1       10
     sky130_fd_sc_hd__a221oi_1      51
     sky130_fd_sc_hd__a221oi_2       4
     sky130_fd_sc_hd__a22o_1        45
     sky130_fd_sc_hd__a22oi_1      183
     sky130_fd_sc_hd__a22oi_2        1
     sky130_fd_sc_hd__a2bb2oi_1      4
     sky130_fd_sc_hd__a311o_1        3
     sky130_fd_sc_hd__a311o_2        1
     sky130_fd_sc_hd__a311oi_1      37
     sky130_fd_sc_hd__a311oi_2       1
     sky130_fd_sc_hd__a31o_2        16
     sky130_fd_sc_hd__a31oi_1       44
     sky130_fd_sc_hd__a31oi_2        1
     sky130_fd_sc_hd__a32o_1         2
     sky130_fd_sc_hd__a32oi_1        5
     sky130_fd_sc_hd__a41o_1         4
     sky130_fd_sc_hd__a41oi_1        3
     sky130_fd_sc_hd__a41oi_2        2
     sky130_fd_sc_hd__and2_0         2
     sky130_fd_sc_hd__and2_1        22
     sky130_fd_sc_hd__and3_1        38
     sky130_fd_sc_hd__and4_1         3
     sky130_fd_sc_hd__buf_1         38
     sky130_fd_sc_hd__buf_2         35
     sky130_fd_sc_hd__buf_4          8
     sky130_fd_sc_hd__buf_6          7
     sky130_fd_sc_hd__buf_8          2
     sky130_fd_sc_hd__clkbuf_1     502
     sky130_fd_sc_hd__clkbuf_2       7
     sky130_fd_sc_hd__clkbuf_8       1
     sky130_fd_sc_hd__clkinv_1       6
     sky130_fd_sc_hd__conb_1         1
     sky130_fd_sc_hd__dfxtp_1     1144
     sky130_fd_sc_hd__fa_1           4
     sky130_fd_sc_hd__ha_1         101
     sky130_fd_sc_hd__inv_1         94
     sky130_fd_sc_hd__inv_2          1
     sky130_fd_sc_hd__mux2_2        39
     sky130_fd_sc_hd__mux2i_1       76
     sky130_fd_sc_hd__mux2i_2        3
     sky130_fd_sc_hd__mux2i_4        4
     sky130_fd_sc_hd__mux4_1        12
     sky130_fd_sc_hd__mux4_2        79
     sky130_fd_sc_hd__nand2_1     1371
     sky130_fd_sc_hd__nand2b_1      24
     sky130_fd_sc_hd__nand3_1      228
     sky130_fd_sc_hd__nand3b_1      37
     sky130_fd_sc_hd__nand4_1       51
     sky130_fd_sc_hd__nand4b_1       1
     sky130_fd_sc_hd__nor2_1       264
     sky130_fd_sc_hd__nor2b_1       54
     sky130_fd_sc_hd__nor3_1        52
     sky130_fd_sc_hd__nor3b_1        7
     sky130_fd_sc_hd__nor4_1        20
     sky130_fd_sc_hd__o2111ai_1      1
     sky130_fd_sc_hd__o211a_1        1
     sky130_fd_sc_hd__o211ai_1      48
     sky130_fd_sc_hd__o211ai_2       4
     sky130_fd_sc_hd__o21a_1        28
     sky130_fd_sc_hd__o21ai_0      385
     sky130_fd_sc_hd__o21ai_1       22
     sky130_fd_sc_hd__o21ai_2        9
     sky130_fd_sc_hd__o21ba_2        2
     sky130_fd_sc_hd__o21bai_1       8
     sky130_fd_sc_hd__o221a_2        2
     sky130_fd_sc_hd__o221ai_1      19
     sky130_fd_sc_hd__o22a_1        33
     sky130_fd_sc_hd__o22ai_1       17
     sky130_fd_sc_hd__o2bb2ai_1      6
     sky130_fd_sc_hd__o311a_1        3
     sky130_fd_sc_hd__o311ai_0       4
     sky130_fd_sc_hd__o311ai_1       2
     sky130_fd_sc_hd__o31a_1         7
     sky130_fd_sc_hd__o31ai_1       34
     sky130_fd_sc_hd__o31ai_4        1
     sky130_fd_sc_hd__o32a_1         2
     sky130_fd_sc_hd__o32ai_1        3
     sky130_fd_sc_hd__o41ai_1        4
     sky130_fd_sc_hd__o41ai_2        1
     sky130_fd_sc_hd__or2_1          1
     sky130_fd_sc_hd__or2_2          8
     sky130_fd_sc_hd__or3_1         20
     sky130_fd_sc_hd__or3b_1         2
     sky130_fd_sc_hd__or3b_2         2
     sky130_fd_sc_hd__or4_1          9
     sky130_fd_sc_hd__or4b_2         1
     sky130_fd_sc_hd__xnor2_1       57
     sky130_fd_sc_hd__xor2_1        27

   Area for cell type \avsddac is unknown!
   Area for cell type \avsdpll is unknown!

   Chip area for module '\vsdbabysoc': 52933.267200
     of which used for sequential elements: 22901.964800 (43.27%)
```
<img width="1213" height="800" alt="Screenshot from 2025-10-24 16-12-14" src="https://github.com/user-attachments/assets/59d10e19-3034-4823-b584-f2cb68a6ad54" />

</details>

----------

### `Run Floorplan`

```shell
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk floorplan
```

This command initiates the floorplanning process for the `vsdbabysoc` design using the specified configuration file `config.mk` on the `sky130hd` platform.

#### Floorplan Error and Fix

❗**Note:** You may encounter the following error:

```shell
[ERROR STA-0164] .../vsdbabysoc/lib/avsdpll.lib line 54, syntax error
Error: floorplan.tcl, 4 STA-0164
```

**Fix:**
This error is caused by commented block structures in your Liberty file avsdpll.lib. OpenROAD’s parser does not tolerate partially commented blocks like:

```shell
//pin (GND#2) {
//  direction : input;
//  max_transition : 2.5;
//  capacitance : 0.001;
//}
```
✅ To fix it, simply delete the entire commented block starting at line 54:

<img width="1213" height="800" alt="Screenshot from 2025-10-24 16-18-39" src="https://github.com/user-attachments/assets/9c27415d-15cf-48fb-92df-ae529007c95d" />
<img width="1213" height="800" alt="Screenshot from 2025-10-24 16-18-48" src="https://github.com/user-attachments/assets/ba9b89be-c34a-40e8-a56a-955b45d3fec3" />
<img width="1213" height="800" alt="Screenshot from 2025-10-24 16-21-13" src="https://github.com/user-attachments/assets/aec0c824-2803-4da4-ad87-3e2cdd134147" />
<img width="1213" height="800" alt="Screenshot from 2025-10-24 16-19-02" src="https://github.com/user-attachments/assets/ac9ccb03-5612-4d6f-8079-f6db66cc1cb6" />

#### Floorplan Result (GUI)

```shell
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_floorplan
```
<img width="1213" height="800" alt="Screenshot from 2025-10-24 16-23-29" src="https://github.com/user-attachments/assets/e91211b2-84c6-4d40-a9a4-66b9e207f312" />

------

### `Run Placement`

```shell
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk place
```
This command executes the placement process for the `vsdbabysoc` design, utilizing the configuration file `config.mk` on the `sky130hd` platform to arrange the circuit components optimally within the defined floorplan.

<img width="1213" height="800" alt="Screenshot from 2025-10-24 16-26-12" src="https://github.com/user-attachments/assets/6832607b-5e91-429b-9649-2b2c506c093e" />
<img width="1213" height="800" alt="Screenshot from 2025-10-24 16-26-29" src="https://github.com/user-attachments/assets/4a19d2b7-7bc1-4c28-aff7-545cd5120031" />
<img width="1213" height="800" alt="Screenshot from 2025-10-24 16-26-44" src="https://github.com/user-attachments/assets/4470b108-ebf7-4729-a189-d29e6423ee59" />
<img width="1213" height="800" alt="Screenshot from 2025-10-24 16-26-57" src="https://github.com/user-attachments/assets/ddfeb5a5-812e-4a1c-ac65-34e4042b3da5" />

#### Placement Result (GUI)

```shell
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_place
```
<img width="1213" height="800" alt="Screenshot from 2025-10-24 16-29-01" src="https://github.com/user-attachments/assets/4dcb13b8-2af0-4c58-9d1f-97c35950d15a" />

To view the Placement Density heatmap in OpenROAD:

Go to **Tools → Heat maps → Placement Density** → **✓ Show numbers**

<img width="1213" height="800" alt="Screenshot from 2025-10-24 16-33-05" src="https://github.com/user-attachments/assets/f29ee94c-769c-4a9e-b2a8-9583d5bcee09" />

</details>

<details>
<summary>Conclusion</summary>

🎉 Conclusion – Week 5: OpenROAD Flow Success! 🚀

By the end of Week 5, I have:

✅ Installed and set up the 🧰 OpenROAD Flow Scripts environment – creating a solid foundation for the physical design flow.

🏗️ Executed and verified the key backend stages:

🧩 Floorplanning – defined chip dimensions, placed macros, and organized the design layout.

📦 Placement – positioned standard cells efficiently to optimize timing, area, and power.

🖥️ Generated and verified both:

📸 Visual outputs (layout views and GUI screenshots)

📜 Log files proving successful execution and correctness of each stage. 

</details>
</details>

<details>
	<summary> Week 6 - Physical Design using OpenLane </summary>
<!---
![Digital_VLSI_SoC_Design_ _Planning_(RTL2GDSII_Flow)1](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/92eb860b-7a88-4c6f-8143-ad3e09fd9c5b)
![Digital_VLSI_SoC_Design_ _Planning_(RTL2GDSII_Flow) (1)1](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/4285c5e4-d5df-43e4-b460-ead45ff67f9b)
-->
![Digital_VLSI_SoC_Design_ _Planning_(RTL2GDSII_Flow) (1)2](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/5b8bdb5f-95c7-41c0-b809-711b2b8ad171)
# Digital VLSI SoC Design and Planning

![Static Badge](https://img.shields.io/badge/OS-linux-orange)
![Static Badge](https://img.shields.io/badge/EDA%20Tools-OpenLANE--Flow%2C_Yosys%2C_abc%2C_OpenROAD%2C_TritonRoute%2C_OpenSTA%2C_magic%2C_netgen%2C_GUNA-navy)
![Static Badge](https://img.shields.io/badge/languages-verilog%2C_bash%2C_TCL-crimson)
![GitHub last commit](https://img.shields.io/github/last-commit/fayizferosh/soc-design-and-planning-nasscom-vsd)
![GitHub language count](https://img.shields.io/github/languages/count/fayizferosh/soc-design-and-planning-nasscom-vsd)
![GitHub top language](https://img.shields.io/github/languages/top/fayizferosh/soc-design-and-planning-nasscom-vsd)
![GitHub repo size](https://img.shields.io/github/repo-size/fayizferosh/soc-design-and-planning-nasscom-vsd)
![GitHub code size in bytes](https://img.shields.io/github/languages/code-size/fayizferosh/soc-design-and-planning-nasscom-vsd)
![GitHub repo file count (file type)](https://img.shields.io/github/directory-file-count/fayizferosh/soc-design-and-planning-nasscom-vsd)
<!---
Comments
-->

> 2 Week digital VLSI SoC design and planning workshop with complete RTL2GDSII flow organised by VSD in collaboration with NASSCOM

## Section 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK (14/03/2024 - 15/03/2024)

### Theory

<details>
  <summary>
Expand or Collapse
  </summary>

#### Package

* In any embedded board we have seen, the part of the board we consider as the chip is only the ***PACKAGE*** of the chip which is nothing but a protective layer or packet bound over the actual chip and the actual manufatured chip is usually present at the center of a package wherein, the connections from package is fed to the chip by ***WIRE BOUND*** method which is none other than basic wired connection.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/7562205a-7435-46c7-a66e-de1626911f14)
![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/7005a9e3-79da-4590-bea0-eb3768127a3d)
![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/70b1c678-2a2e-484f-9181-812dbcd5f0a3)

#### Chip

* Now, taking a look inside the chip, all the signals from the external world to the chip and vice versa is passed through ***PADS***. The area bound by the pads is ***CORE*** where all the digital logic of the chip is placed. Both the core and pads make up the ***DIE*** which is the basic manufacturing unit in regards to semiconductor chips.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/d65a0ddf-2f86-4bbc-8d36-b02e09a1483e)

* ***FOUNDRY*** is the place where the semiconductor chips are manufactured and ***FOUNDRY IP's*** are Intellectual Properties based on a specific foundry and these IP's require a specific level of intelligence to be produced whereas, repeatable digital logic blocks are called ***MACROS***.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/ed1cd25e-6270-4b84-8f0d-f0ea7c8a7ef8)

#### ISA (Intruction Set Architecture)

* A C program which has to be run on a specific hardware layout which is the interior of a chip in your laptop, there is certain flow to be followed.
* Initially, this particular C program is compiled in it's assembly language program which is nothing but ***RISC-V ISA (Reduced Instruction Set Compting - V Intruction Set Architecture)***.
* Following this, the assembly language program is then converted to machine language program which is the binary language logic 0 and 1 which is understood by the hardware of the computer.
* Directly after this, we've to implement this RISC-V specification using some ***RTL (a Hardware Description Language)***. Finally, from the RTL to ***Layout*** it is a standard PnR or RTL to GDSII flow.

![Screenshot (278)](https://github.com/fayizferosh/risc-v-myth-report/assets/63997454/7dc4601a-e386-48e5-9d1f-7fa5b47ca0ba)

* For an application software to be run on a hardware there are several processes taking place. To begin with, the apps enters into a block called system software and it converts the application program to binary language. There are various layers in system software in which the major layers or components are OS (Operating System), Compiler and Assembler.
* At first the OS outputs are small function in C, C++, VB or Java language which are taken by the respective compiler and converted into instructions and the syntax of these instructions varies with the hardware architecture on which the system is implemented.
* Then, the job of the assembler is to take these instructions and convert it into it's binary format which is basically called as a machine language program. Finally, this binary language is fed to the hardware and it understands the specific functions it has to perform based on the binary code it receives.

![Screenshot (279)](https://github.com/fayizferosh/risc-v-myth-report/assets/63997454/19e8b634-f209-41a6-928d-6fba66f5b177)

* For example, if we take a stopwatch app on RISC-V core, then the output of the OS could be a small C function which enters into the compiler and we get output RISC-V instructions following this, the output of the assembler will be the binary code which enters into your chip layout.

![Screenshot (280)](https://github.com/fayizferosh/risc-v-myth-report/assets/63997454/7d4570ca-82a6-4abe-81d2-067ebb9b2c15)

* For the above stopwatch the following are the input and output of the compiler and assembler.

![Screenshot (281)](https://github.com/fayizferosh/risc-v-myth-report/assets/63997454/d7b7fd1b-21ee-46b7-9a91-8314bd753a51)

* The output of the compiler are instructions and the output of the assembler is the binary pattern. Now, we need some RTL (a Hardware Description Language) which understands and implements the particular instructions. Then, this RTL is synthesised into a netlist in form of gates which is fabricated into the chip through a physical design implementation.

![Screenshot (282)](https://github.com/fayizferosh/risc-v-myth-report/assets/63997454/e349cb06-45e3-4ae4-b85f-9020a0a62737)

* There are mainly 3 different parts in this course. They are:
1. RISC-V ISA
2. RTL and synthesis of RISC-V based CPU core - picorv32
3. Physical design implementation of picorv32

![Screenshot (283)](https://github.com/fayizferosh/risc-v-myth-report/assets/63997454/832f0ea6-2d60-4d9a-937c-a2dedd5f8cac)

#### Open-source Implementation

* For open-source ASIC design implemantation, we require the following enablers to be readily available as open-source versions. They are:-
1. RTL Designs
2. EDA Tools
3. PDK Data

* Initially in the early ages, the design and fabrication of IC's were tightly coupled and were only practiced by very few companies like TI, Intel, etc.
* In 1979, Lynn Conway and Carver Mead came up with an idea to saperate the design from the fabrication and to do this they inroduced structured design methodologies based on the λ-based design rules and published the first VLSI book "Introduction to VLSI System" which started the VLSI education.
* This methodology resulted in the emergence of the design only companies or ***"Fabless Companies"*** and fabrication only companies that we usually refer to as ***"Pure Play Fabs"***.
* The inteface between the designers and the fab by now became a set of data files and documents, that are reffered to as the ***"Process Design Kits (PDKs)"***.
* The PDK include but not limited to Device Models, Technology Information, Design Rules, Digital Standard Cell Libraries, I/O Libraries and many more.
* Since, the PDK contained variety of informations, and so they were distributed only under NDAs (Non-Disclosure Agreements) which made it in-accessible to the public.
* Recently, Google worked out an agreement with skywater to open-source the PDK for the 130nm process by skywater Technology, as a result on 30 June 2020 Google released the first ever open-source PDK.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/87384374-e66b-4ec6-b9c4-3fb92ad4d275)

* ASIC design is a complex step that involves tons of steps, various methodologies and respective EDA tools which are all required for successful ASIC implementation which is achieved though an ASIC flow which is nothing but a piece of software that pulls different tools togather to carry out the design process.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/1762d6d6-c5f8-4bd9-8a3d-968eb4360889)

#### OpenLANE Open-source ASIC Design Implementation Flow

* The main objective of the ASIC Design Flow is to take the design from the RTL (Register Transfer Level) all the way to the GDSII, which is the format used for the final fabrication layout.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/533f58ee-4524-4a18-abb5-36b4d6a56b1f)

* Synthesis is the process of convertion or translation of design RTL into circuits made out of Standard Cell Libraries (SCL) the resultant circuit is described in HDL and is usually reffered to as the Gate-Level Netlist.
* Gate-Level Netlist is functionally equivalent to the RTL.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/e43891a0-ab09-4df2-898d-7e843158e936)

* The fundemental building blocks which are the standard cells have regular layouts.
* Each cell has different views/models which are utilised by different EDA tools like liberty view with electrical models of the cells, HDL behavioral models, SPICE or CDL views of the cells, Layout view which include GDSII view which is the detailed view and LEF view which is the abstract view.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/48df884c-c894-4a2a-a511-09321c208d6b)

* Chip Floor Planning

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/38ecd866-ac83-42c7-83ba-2ba995f9ba4e)

* Macro Floor Planning

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/35ac760c-bba9-4bd1-9c65-e8ceeee2ccf5)

* Power Planning typically uses upper metal layers for power distribution since thay are thicker than lower metal layers and so have lower resistance and PP is done to avoid electron migration and IR drops.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/f18013a7-e4c7-4a6d-ba53-33362c04689a)

* Placement

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/3654398b-40bc-4e42-9205-77f673d5584c)

* Global placement provide approximate locations for all cells based on connectivity but in this stage the cells may be overlapped on each other and in detailed placement the positions obtained from global placements are minimally altered to make it legal (non-overlapping and in site-rows)

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/817c504d-0b8e-4a0a-9c3c-e9d110c23535)

* Clock Tree Synthesis

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/6db284d4-065f-450a-9200-a6e6cbfd7fbb)

* Clock skew is the time difference in arrival of clock at different components.
* Routing

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/7458495f-b527-4f21-813e-8dbcbf29ed9b)

* skywater PDK has 6 routing layers in which the lowest layer is called the local interconnect layer which is a Titanium Nitride layer the following 5 layers are all Aluminium layers.
![stackup](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/e4438c12-d83e-4083-a58f-33a410a47927)

* Global and Detailed Routing

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/b54ebd4c-127a-441f-829e-6000531e9b8d)

* Once done with the routing the final layout can be generated which undergoes various Sign-Off checks.
* Design Rules Checking (DRC) which verifies that the final layout honours all design fabrication rules.
* Layout Vs Schematic (LVS) which verifies that the final layout functionality matches the gate-level netlist that we started with.
* Static Timing Analysis (STA) to verify that the design runs at the designated clock frequency.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/d8bc72cd-2fe9-4ae2-9431-68a6fa77c671)

</details>

### Implementation

Section 1 tasks:- 
1. Run 'picorv32a' design synthesis using OpenLANE flow and generate necessary outputs.
2. Calculate the flop ratio.

```math
Flop\ Ratio = \frac{Number\ of\ D\ Flip\ Flops}{Total\ Number\ of\ Cells}
```
```math
Percentage\ of\ DFF's = Flop\ Ratio * 100
```

* All section 1 logs, reports and results can be found in following run folder:

[Section 1 Run - 15-03_15-51](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/tree/main/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/15-03_15-51)

#### 1. Run 'picorv32a' design synthesis using OpenLANE flow and generate necessary outputs.

Commands to invoke the OpenLANE flow and perform synthesis

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit
```

Screenshots of running each commands

![1](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/d19f6d0f-16f8-4e79-aa5a-f2a34b9fb203)
![2](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/5e03c8ca-8c7f-4579-a7bc-10161007910e)
![3](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/5f196a31-059e-4192-a208-8a15ba1a0dd7)

#### 2. Calculate the flop ratio.

Screenshots of synthesis statistics report file with required values highlighted

![Screenshot from 2024-03-15 22-02-42](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/9280fefc-80b2-44ef-af34-ef3bddd3c14e)
![Screenshot from 2024-03-15 22-03-39](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/3275f46c-19d7-42c5-8984-96d455f6e09b)

Calculation of Flop Ratio and DFF % from synthesis statistics report file

```math
Flop\ Ratio = \frac{1613}{14876} = 0.108429685
```
```math
Percentage\ of\ DFF's = 0.108429685 * 100 = 10.84296854\ \%
```

## Section 2 - Good floorplan vs bad floorplan and introduction to library cells (16/03/2024 - 17/03/2024)

### Theory

### Implementation

Section 2 tasks:- 
1. Run 'picorv32a' design floorplan using OpenLANE flow and generate necessary outputs.
2. Calculate the die area in microns from the values in floorplan def.
3. Load generated floorplan def in magic tool and explore the floorplan.
4. Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.
5. Load generated placement def in magic tool and explore the placement.

```math
Area\ of\ die\ in\ microns = Die\ width\ in\ microns * Die\ height\ in\ microns
```

* All section 2 logs, reports and results can be found in following run folder:

[Section 2 Run - 17-03_12-06](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/tree/main/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06)

#### 1. Run 'picorv32a' design floorplan using OpenLANE flow and generate necessary outputs.

Commands to invoke the OpenLANE flow and perform floorplan

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Now we can run floorplan
run_floorplan
```

Screenshot of floorplan run

![Screenshot from 2024-03-17 18-06-19](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/7deda325-2ae8-4e98-aa71-7a54f5c34fcb)
![Screenshot from 2024-03-17 18-06-36](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/c1fe538f-c58f-46b9-9466-b0873a88eb6c)

#### 2. Calculate the die area in microns from the values in floorplan def.

Screenshot of contents of floorplan def

![Screenshot from 2024-03-17 18-34-53](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/9a0baa93-7db6-4148-b155-49b18c130522)

According to floorplan def
```math
1000\ Unit\ Distance = 1\ Micron
```
```math
Die\ width\ in\ unit\ distance = 660685 - 0 = 660685
```
```math
Die\ height\ in\ unit\ distance = 671405 - 0 = 671405
```
```math
Distance\ in\ microns = \frac{Value\ in\ Unit\ Distance}{1000}
```
```math
Die\ width\ in\ microns = \frac{660685}{1000} = 660.685\ Microns
```
```math
Die\ height\ in\ microns = \frac{671405}{1000} = 671.405\ Microns
```
```math
Area\ of\ die\ in\ microns = 660.685 * 671.405 = 443587.212425\ Square\ Microns
```

#### 3. Load generated floorplan def in magic tool and explore the floorplan.

Commands to load floorplan def in magic in another terminal

```bash
# Change directory to path containing generated floorplan def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/floorplan/

# Command to load the floorplan def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

Screenshots of floorplan def in magic

![Screenshot from 2024-03-17 18-05-19](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/93af15d9-ba65-49d4-8e98-ad1d0c4b0097)

Equidistant placement of ports

![Screenshot from 2024-03-17 18-14-28](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/6183e357-315c-40df-9bc3-e7993d76b19c)

Port layer as set through config.tcl

![Screenshot from 2024-03-17 18-17-46](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/4e35528c-1bb1-4eaa-84be-14a95e532b75)
![Screenshot from 2024-03-17 18-19-50](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/a88afbbd-6d63-4ce5-a1ec-620dd8c37f45)

Decap Cells and Tap Cells

![Screenshot from 2024-03-17 18-22-57](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/c1714ed1-7cdd-4b3c-8e0b-e4f97270ef82)

Diogonally equidistant Tap cells

![Screenshot from 2024-03-17 18-25-28](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/6f62f639-81cc-4e5d-8b5c-ea5b209125ba)

Unplaced standard cells at the origin

![Screenshot from 2024-03-17 18-31-41](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/f875937c-cec4-4c2c-8c4b-6808d81821d6)

#### 4. Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.

Command to run placement

```tcl
# Congestion aware placement by default
run_placement
```

Screenshots of placement run

![Screenshot from 2024-03-17 22-44-17](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/3ddaf32e-fdbb-4410-bfe6-7ea6b2640438)
![Screenshot from 2024-03-17 22-46-27](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/e6b36b9b-b9bc-4390-84fd-a10d23e2246f)

#### 5. Load generated placement def in magic tool and explore the placement.

Commands to load placement def in magic in another terminal

```bash
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

Screenshots of floorplan def in magic

![Screenshot from 2024-03-17 22-58-44](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/e703ef0b-3968-4132-a9c7-05b53f50b214)

Standard cells legally placed 

![Screenshot from 2024-03-17 23-04-20](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/54911138-f942-48b9-b4e9-61d741a4b5ac)

Commands to exit from current run

```tcl
# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit
```

## Section 3 - Design library cell using Magic Layout and ngspice characterization (18/03/2024 - 21/03/2024)

### Theory

### Implementation

* Section 3 tasks:-
1. Clone custom inverter standard cell design from github repository: [Standard cell design and characterization using OpenLANE flow](https://github.com/nickson-jose/vsdstdcelldesign).
2. Load the custom inverter layout in magic and explore.
3. Spice extraction of inverter in magic.
4. Editing the spice model file for analysis through simulation.
5. Post-layout ngspice simulations.
6. Find problem in the DRC section of the old magic tech file for the skywater process and fix them.

* Section 3 - Tasks 1 to 5 files, reports and logs can be found in the following folder:

[Section 3 - Tasks 1 to 5 \(vsdstdcelldesign\)](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/tree/main/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign)

* Section 3 - Task 6 files, reports and logs can be found in the following folder:

[Section 3 - Task 6 \(drc_tests\)](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/tree/main/drc_tests)

#### 1. Clone custom inverter standard cell design from github repository

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Clone the repository with custom inverter design
git clone https://github.com/nickson-jose/vsdstdcelldesign

# Change into repository directory
cd vsdstdcelldesign

# Copy magic tech file to the repo directory for easy access
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .

# Check contents whether everything is present
ls

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
```

Screenshot of commands run

![Screenshot from 2024-03-19 00-22-27](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/8f304929-a190-4aa1-9cc4-b8fefa1909e8)

#### 2. Load the custom inverter layout in magic and explore.

Screenshot of custom inverter layout in magic

![Screenshot from 2024-03-19 00-22-44](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/6eae887c-ebc6-4771-8bcc-e4edaf9947d9)

NMOS and PMOS identified

![Screenshot from 2024-03-19 00-28-03](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/eada5c8b-154c-4eea-819c-d49f89495acb)
![Screenshot from 2024-03-19 00-29-14](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/dec7e465-8fd0-45d5-bac6-10de1acb8c76)

Output Y connectivity to PMOS and NMOS drain verified

![Screenshot from 2024-03-19 00-31-17](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/e16fb883-907e-437a-b8d8-1b9d1b2e67a6)

PMOS source connectivity to VDD (here VPWR) verified

![Screenshot from 2024-03-19 00-34-11](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/173efda1-d5d1-49d9-a040-771092b1e55b)

NMOS source connectivity to VSS (here VGND) verified

![Screenshot from 2024-03-19 00-36-09](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/107a8e0b-43de-4b5d-90fe-516ea83673b1)

Deleting necessary layout part to see DRC error

![Screenshot from 2024-03-19 01-10-28](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/861912e4-eef9-4226-b563-db7f49ca6632)

#### 3. Spice extraction of inverter in magic.

Commands for spice extraction of the custom inverter layout to be used in tkcon window of magic

```tcl
# Check current directory
pwd

# Extraction command to extract to .ext format
extract all

# Before converting ext to spice this command enable the parasitic extraction also
ext2spice cthresh 0 rthresh 0

# Converting to ext to spice
ext2spice
```

Screenshot of tkcon window after running above commands

![Screenshot from 2024-03-19 01-24-17](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/831b0be9-3c02-4bbb-800e-6f1c3dc1ba1a)

Screenshot of created spice file

![Screenshot from 2024-03-19 01-27-07](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/2c645f55-c4d5-4007-8b9c-73ba8a8e5bcb)

#### 4. Editing the spice model file for analysis through simulation.

Measuring unit distance in layout grid

![Screenshot from 2024-03-19 01-30-15](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/74335564-b7a6-4b7d-b4b7-bb251c8d790b)

Final edited spice file ready for ngspice simulation

![Screenshot from 2024-03-19 14-50-54](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/b5d20507-b65e-4b54-ba8e-576fb4d09429)
![Screenshot from 2024-03-19 14-51-16](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/9cd17c95-de5b-48da-b8bc-2ee0c30915ef)

#### 5. Post-layout ngspice simulations.

Commands for ngspice simulation

```bash
# Command to directly load spice file for simulation to ngspice
ngspice sky130_inv.spice

# Now that we have entered ngspice with the simulation spice file loaded we just have to load the plot
plot y vs time a
```

Screenshots of ngspice run

![Screenshot from 2024-03-19 14-56-42](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/c023ebb0-756f-4707-ae82-a28746f372da)
![Screenshot from 2024-03-19 14-57-22](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/addefe5e-6a9a-44f2-943a-4a9373ddc56c)

Screenshot of generated plot

![Screenshot from 2024-03-19 14-58-55](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/dd14a5d5-ffd9-4ad8-a871-e32af61362a3)

Rise transition time calculation

```math
Rise\ transition\ time = Time\ taken\ for\ output\ to\ rise\ to\ 80\% - Time\ taken\ for\ output\ to\ rise\ to\ 20\%
```
```math
20\%\ of\ output = 660\ mV
```
```math
80\%\ of\ output = 2.64\ V
```

20% Screenshots

![Screenshot from 2024-03-19 15-15-02](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/261c420f-219f-4c26-ae32-6c0db82a722e)
![Screenshot from 2024-03-19 15-20-04](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/bbb078c4-b3aa-436b-8832-23e5d7777081)

80% Screenshots

![Screenshot from 2024-03-19 15-23-34](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/d10a0ff1-0523-4fe4-96f4-eefc63f647f7)
![Screenshot from 2024-03-19 15-24-13](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/929042ad-2032-49aa-ae07-2a2163b9603e)

```math
Rise\ transition\ time = 2.24638 - 2.18242 = 0.06396\ ns = 63.96\ ps
```

Fall transition time calculation

```math
Fall\ transition\ time = Time\ taken\ for\ output\ to\ fall\ to\ 20\% - Time\ taken\ for\ output\ to\ fall\ to\ 80\%
```
```math
20\%\ of\ output = 660\ mV
```
```math
80\%\ of\ output = 2.64\ V
```

20% Screenshots

![Screenshot from 2024-03-19 15-34-22](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/0180052c-4b8c-4bd8-928c-cd8ab34d5a17)
![Screenshot from 2024-03-19 15-34-34](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/83760cf7-18c9-45d1-8063-04baafe1dd1f)

80% Screenshots

![Screenshot from 2024-03-19 15-36-29](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/7bc0eeee-c7cd-464e-a90b-cac8d4f83144)
![Screenshot from 2024-03-19 15-36-41](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/9a7dc3b0-9936-4704-97cd-1cd03cc6a8cb)

```math
Fall\ transition\ time = 4.0955 - 4.0536 = 0.0419\ ns = 41.9\ ps
```

Rise Cell Delay Calculation

```math
Rise\ Cell\ Delay = Time\ taken\ for\ output\ to\ rise\ to\ 50\% - Time\ taken\ for\ input\ to\ fall\ to\ 50\%
```
```math
50\%\ of\ 3.3\ V = 1.65\ V
```

50% Screenshots

![Screenshot from 2024-03-19 16-02-35](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/e34363cd-a70f-4939-b8e5-efb10620ce93)
![Screenshot from 2024-03-19 16-03-46](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/f7452b60-3612-4bcf-a71d-b8ff021d5297)

```math
Rise\ Cell\ Delay = 2.21144 - 2.15008 = 0.06136\ ns = 61.36\ ps
```

Fall Cell Delay Calculation

```math
Fall\ Cell\ Delay = Time\ taken\ for\ output\ to\ fall\ to\ 50\% - Time\ taken\ for\ input\ to\ rise\ to\ 50\%
```
```math
50\%\ of\ 3.3\ V = 1.65\ V
```

50% Screenshots

![Screenshot from 2024-03-19 16-09-08](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/3d2ff2e5-dab6-497a-b5a4-74959f69c2a2)
![Screenshot from 2024-03-19 16-10-03](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/aa88c26b-0cc4-4cf7-80d7-b2058e8fbc47)

```math
Fall\ Cell\ Delay = 4.07 - 4.05 = 0.02\ ns = 20\ ps
```

#### 6. Find problem in the DRC section of the old magic tech file for the skywater process and fix them.

Link to Sky130 Periphery rules: [https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html](https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html)

Commands to download and view the corrupted skywater process magic tech file and associated files to perform drc corrections

```bash
# Change to home directory
cd

# Command to download the lab files
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz

# Since lab file is compressed command to extract it
tar xfz drc_tests.tgz

# Change directory into the lab folder
cd drc_tests

# List all files and directories present in the current directory
ls -al

# Command to view .magicrc file
gvim .magicrc

# Command to open magic tool in better graphics
magic -d XR &
```

Screenshots of commands run

![Screenshot from 2024-03-21 22-33-57](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/1b4cf68e-fa83-4d44-9b08-ca2b63ceb471)
![Screenshot from 2024-03-21 22-34-09](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/1bc14ddb-feb6-4052-bc12-0f018f09c343)

Screenshot of .magicrc file

![Screenshot from 2024-03-21 22-35-58](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/89b46a0f-63b7-445c-bf2b-e6cda16853c7)

**Incorrectly implemented poly.9 simple rule correction**

Screenshot of poly rules

![Screenshot from 2024-03-21 22-54-49](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/9260cf37-5933-44a1-8362-597183644334)

Incorrectly implemented poly.9 rule no drc violation even though spacing < 0.48u

![Screenshot from 2024-03-21 22-54-19](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/acfbcf69-020e-4b62-96bd-b7630aa74ef0)
![Screenshot from 2024-03-21 23-54-11](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/a05bd29a-b181-4e26-826a-d32f12696b2c)

New commands inserted in sky130A.tech file to update drc

![Screenshot from 2024-03-21 23-58-44](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/dc30df54-3282-42f0-8e7d-fc4d8877ed64)
![Screenshot from 2024-03-21 23-09-50](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/d112ca07-854c-41e7-8822-c269e21defea)

Commands to run in tkcon window

```tcl
# Loading updated tech file
tech load sky130A.tech

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```

Screenshot of magic window with rule implemented

![Screenshot from 2024-03-21 23-13-11](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/b18e8e07-ef0f-40fb-9b6d-8aae878a23c6)
![Screenshot from 2024-03-22 00-00-40](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/d5afe8d8-691b-485d-a89a-8f901e18b56e)

**Incorrectly implemented difftap.2 simple rule correction**

Screenshot of difftap rules

![Screenshot from 2024-03-22 00-14-47](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/086b7a66-b60a-470a-b5c0-a5ac938ebec3)

Incorrectly implemented difftap.2 rule no drc violation even though spacing < 0.42u

![Screenshot from 2024-03-22 00-14-36](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/a2d0d739-2df5-4eb5-ab78-c80d366e24e4)

New commands inserted in sky130A.tech file to update drc

![Screenshot from 2024-03-22 00-26-43](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/b5892f9b-9c5d-4b1b-baa2-6fe45f3965b1)

Commands to run in tkcon window

```tcl
# Loading updated tech file
tech load sky130A.tech

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```

Screenshot of magic window with rule implemented

![Screenshot from 2024-03-22 00-29-22](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/a3f92160-6701-48fb-b6cf-e4c41dc4a531)

**Incorrectly implemented nwell.4 complex rule correction**

Screenshot of nwell rules

![Screenshot from 2024-03-22 00-51-34](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/4ad4901d-0b9a-4339-89e3-7bb3fce2766d)

Incorrectly implemented nwell.4 rule no drc violation even though no tap present in nwell

![Screenshot from 2024-03-22 00-52-51](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/87da8944-0ad8-455d-97ec-3909eac656c3)

New commands inserted in sky130A.tech file to update drc

![Screenshot from 2024-03-22 01-03-42](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/886c6930-6314-4a6f-97d9-6b8423444ac0)
![Screenshot from 2024-03-22 01-04-04](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/d9808e9a-42c2-4421-9b82-2ef65a5a1ad7)

Commands to run in tkcon window

```tcl
# Loading updated tech file
tech load sky130A.tech

# Change drc style to drc full
drc style drc(full)

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```

Screenshot of magic window with rule implemented

![Screenshot from 2024-03-22 01-10-25](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/49b1004d-f860-4ca7-86f4-4d79784a01cf)

## Section 4 - Pre-layout timing analysis and importance of good clock tree (22/03/2024 - 24/03/2024)

### Theory

### Implementation

* Section 4 tasks:-
1. Fix up small DRC errors and verify the design is ready to be inserted into our flow.
2. Save the finalized layout with custom name and open it.
3. Generate lef from the layout.
4. Copy the newly generated lef and associated required lib files to 'picorv32a' design 'src' directory.
5. Edit 'config.tcl' to change lib file and add the new extra lef into the openlane flow.
6. Run openlane flow synthesis with newly inserted custom inverter cell.
7. Remove/reduce the newly introduced violations with the introduction of custom inverter cell by modifying design parameters.
8. Once synthesis has accepted our custom inverter we can now run floorplan and placement and verify the cell is accepted in PnR flow.
9. Do Post-Synthesis timing analysis with OpenSTA tool.
10. Make timing ECO fixes to remove all violations.
11. Replace the old netlist with the new netlist generated after timing ECO fix and implement the floorplan, placement and cts.
12. Post-CTS OpenROAD timing analysis.
13. Explore post-CTS OpenROAD timing analysis by removing 'sky130_fd_sc_hd__clkbuf_1' cell from clock buffer list variable 'CTS_CLK_BUFFER_LIST'.

* Section 4 - Tasks 1 to 4 files, reports and logs can be found in the following folder:

[Section 4 - Tasks 1 to 4 \(vsdstdcelldesign\)](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/tree/main/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign)

* Section 4 - Task 4 files, reports and logs can be found in the following folder:

[Section 4 - Task 4 \(src\)](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/tree/main/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src)

* Section 4 - Task 5 files, reports and logs can be found in the following folder:

[Section 4 - Task 5 \(picorv32a\)](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/tree/main/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a)

* Section 4 - Tasks 6 to 8 & 11 to 13 logs, reports and results can be found in following run folder:

[Section 4 - Tasks 6 to 8 & 11 to 13 Run \(24-03_10-03\)](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/tree/main/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/24-03_10-03)

* Section 4 - Tasks 9 to 11 logs, reports and results can be found in following run folder:

[Section 4 - Tasks 9 to 11 Run \(25-03_18-52\)](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/tree/main/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/25-03_18-52)

#### 1. Fix up small DRC errors and verify the design is ready to be inserted into our flow.

Conditions to be verified before moving forward with custom designed cell layout:
* Condition 1: The input and output ports of the standard cell should lie on the intersection of the vertical and horizontal tracks.
* Condition 2: Width of the standard cell should be odd multiples of the horizontal track pitch.
* Condition 3: Height of the standard cell should be even multiples of the vertical track pitch.

Commands to open the custom inverter layout

```bash
# Change directory to vsdstdcelldesign
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
```

Screenshot of tracks.info of sky130_fd_sc_hd

![Screenshot from 2024-03-24 13-38-09](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/2a35eb22-dd5f-4b67-9712-cbd2a84b526a)

Commands for tkcon window to set grid as tracks of locali layer

```tcl
# Get syntax for grid command
help grid

# Set grid values accordingly
grid 0.46um 0.34um 0.23um 0.17um
```

Screenshot of commands run

![Screenshot from 2024-03-24 13-49-55](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/d0d9c106-4e05-4e73-a7ed-3f718cb69b42)

Condition 1 verified

![Screenshot from 2024-03-24 13-51-55](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/b74b31c8-cdc7-4dcb-9467-5a1787bfa5fe)

Condition 2 verified

```math
Horizontal\ track\ pitch = 0.46\ um
```

![Screenshot from 2024-03-24 13-55-07](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/e045e5b6-3592-4242-995d-de2049438ec5)

```math
Width\ of\ standard\ cell = 1.38\ um = 0.46 * 3
```

Condition 3 verified

```math
Vertical\ track\ pitch = 0.34\ um
```

![Screenshot from 2024-03-24 13-58-32](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/a471b022-91ac-466a-8dd9-72b90f9c16c1)

```math
Height\ of\ standard\ cell = 2.72\ um = 0.34 * 8
```

#### 2. Save the finalized layout with custom name and open it.

Command for tkcon window to save the layout with custom name

```tcl
# Command to save as
save sky130_vsdinv.mag
```

Command to open the newly saved layout

```bash
# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_vsdinv.mag &
```

Screenshot of newly saved layout

![Screenshot from 2024-03-24 14-33-20](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/0beb4300-2ebc-4364-8e3d-37fdb6d52f5b)

#### 3. Generate lef from the layout.

Command for tkcon window to write lef

```tcl
# lef command
lef write
```

Screenshot of command run

![Screenshot from 2024-03-24 14-35-55](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/6928c3dc-e633-414d-9ac1-71349cad4b9b)

Screenshot of newly created lef file

![Screenshot from 2024-03-24 14-37-19](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/15557990-33b4-4402-8c72-39b75da9ed07)

#### 4. Copy the newly generated lef and associated required lib files to 'picorv32a' design 'src' directory.

Commands to copy necessary files to 'picorv32a' design 'src' directory

```bash
# Copy lef file
cp sky130_vsdinv.lef ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# List and check whether it's copied
ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# Copy lib files
cp libs/sky130_fd_sc_hd__* ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# List and check whether it's copied
ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
```

Screenshot of commands run

![Screenshot from 2024-03-24 14-55-23](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/78559cee-ad3f-4301-83ae-df99f8417be3)

#### 5. Edit 'config.tcl' to change lib file and add the new extra lef into the openlane flow.

Commands to be added to config.tcl to include our custom cell in the openlane flow

```tcl
set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
```

Edited config.tcl to include the added lef and change library to ones we added in src directory

![Screenshot from 2024-03-24 15-29-56](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/7b18f216-1160-4a65-91fd-998495ad3175)

#### 6. Run openlane flow synthesis with newly inserted custom inverter cell.

Commands to invoke the OpenLANE flow include new lef and perform synthesis 

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

Screenshots of commands run


![Screenshot from 2024-03-24 15-36-46](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/4170c3c5-1a95-4165-9461-03b298cc20ef)
![Screenshot from 2024-03-24 15-37-32](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/8f52942c-4b28-4abd-b9a0-d48f20a8255f)
![Screenshot from 2024-03-24 15-37-44](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/47849bfd-dc47-4d9c-9077-7fb672df4ead)
![Screenshot from 2024-03-24 15-45-08](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/0bc13ad3-d800-4681-b39d-8b64c9c9104f)

#### 7. Remove/reduce the newly introduced violations with the introduction of custom inverter cell by modifying design parameters.

Noting down current design values generated before modifying parameters to improve timing

![Screenshot from 2024-03-24 16-00-18](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/33fe575a-7459-4c59-8329-f142ba2099e5)
![Screenshot from 2024-03-24 16-13-01](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/13e42f0a-69e7-410d-b901-bc6c4976b7e1)

Commands to view and change parameters to improve timing and run synthesis

```tcl
# Now once again we have to prep design so as to update variables
prep -design picorv32a -tag 24-03_10-03 -overwrite

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to display current value of variable SYNTH_STRATEGY
echo $::env(SYNTH_STRATEGY)

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to display current value of variable SYNTH_BUFFERING to check whether it's enabled
echo $::env(SYNTH_BUFFERING)

# Command to display current value of variable SYNTH_SIZING
echo $::env(SYNTH_SIZING)

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Command to display current value of variable SYNTH_DRIVING_CELL to check whether it's the proper cell or not
echo $::env(SYNTH_DRIVING_CELL)

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

Screenshot of merged.lef in `tmp` directory with our custom inverter as macro

![Screenshot from 2024-03-24 23-46-25](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/55de3fc6-498d-4456-8e79-ae6e175d2ca6)

Screenshots of commands run

![Screenshot from 2024-03-24 17-09-04](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/62209cce-90c2-4c52-a218-25805b57ef3f)
![Screenshot from 2024-03-24 17-09-19](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/2d933bd5-a3b5-4d6d-a22f-5332bc3bf279)
![Screenshot from 2024-03-24 17-10-46](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/a25b66af-9cf9-4ba9-adb6-f38ff85fa7cd)

Comparing to previously noted run values area has increased and worst negative slack has become 0

![Screenshot from 2024-03-24 17-11-08](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/81418082-747e-4702-b5ad-bb3e450eceb3)
![Screenshot from 2024-03-24 17-11-19](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/a1bdb538-527c-4edd-877d-d4263e777321)

#### 8. Once synthesis has accepted our custom inverter we can now run floorplan and placement and verify the cell is accepted in PnR flow.

Now that our custom inverter is properly accepted in synthesis we can now run floorplan using following command

```tcl
# Now we can run floorplan
run_floorplan
```

Screenshots of command run

![Screenshot from 2024-03-24 17-12-09](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/10a18995-0b7c-4f44-8ef4-cca9239652da)
![Screenshot from 2024-03-24 17-37-50](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/72966b69-cea0-4ae7-8dc0-c7130a8c750a)

Since we are facing unexpected un-explainable error while using `run_floorplan` command, we can instead use the following set of commands available based on information from `Desktop/work/tools/openlane_working_dir/openlane/scripts/tcl_commands/floorplan.tcl` and also based on `Floorplan Commands` section in `Desktop/work/tools/openlane_working_dir/openlane/docs/source/OpenLANE_commands.md`

```tcl
# Follwing commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or
```

Screenshots of commands run

![Screenshot from 2024-03-24 23-38-07](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/07534ca7-b0db-4ea2-bbcd-ea7d44241d6c)
![Screenshot from 2024-03-24 23-38-54](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/c9705150-f953-4372-a811-88cae6378d2f)
![Screenshot from 2024-03-24 23-39-56](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/c16bccc6-c650-4a65-b1de-7035609520d7)

Now that floorplan is done we can do placement using following command

```tcl
# Now we are ready to run placement
run_placement
```

Screenshots of command run

![Screenshot from 2024-03-24 23-49-29](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/12788798-aac5-4cfb-9254-69fb3d4e8e70)
![Screenshot from 2024-03-24 23-51-08](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/41eaeae7-d398-417c-b89f-e9014a92a699)

Commands to load placement def in magic in another terminal

```bash
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/24-03_10-03/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

Screenshot of placement def in magic

![Screenshot from 2024-03-25 00-16-54](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/9cb8b463-a0dd-402f-b881-2504686b8d04)

Screenshot of custom inverter inserted in placement def with proper abutment

![Screenshot from 2024-03-25 00-00-10](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/2fb0fc71-4784-4987-a55e-5d71dd35edbf)

Command for tkcon window to view internal layers of cells

```tcl
# Command to view internal connectivity layers
expand
```

Abutment of power pins with other cell from library clearly visible

![Screenshot from 2024-03-25 00-01-46](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/b52d756d-430c-4e43-b514-db0084dc1794)
![Screenshot from 2024-03-25 00-05-35](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/b8342668-c3fe-437e-b701-8c8be3740682)

#### 9. Do Post-Synthesis timing analysis with OpenSTA tool.

Since we are having 0 wns after improved timing run we are going to do timing analysis on initial run of synthesis which has lots of violations and no parameters were added to improve timing

Commands to invoke the OpenLANE flow include new lef and perform synthesis 

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

Commands run final screenshot

![Screenshot from 2024-03-26 05-52-18](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/790d4852-7f1d-47b5-a64f-5735f6064b61)

Newly created `pre_sta.conf` for STA analysis in `openlane` directory

![Screenshot from 2024-03-26 05-53-06](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/0a02d055-f012-44bd-a750-4508bbfc771e)

Newly created `my_base.sdc` for STA analysis in `openlane/designs/picorv32a/src` directory based on the file `openlane/scripts/base.sdc`

![Screenshot from 2024-03-26 05-55-17](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/8c917d87-5507-4079-ba6b-facbf18c238f)
![Screenshot from 2024-03-26 05-55-38](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/e07d05db-2f06-47ab-bff7-2af88c39d001)

Commands to run STA in another terminal

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Command to invoke OpenSTA tool with script
sta pre_sta.conf
```

Screenshots of commands run

![Screenshot from 2024-03-26 06-04-28](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/32def177-5173-4dd7-ba3b-44e37846644c)
![Screenshot from 2024-03-26 06-05-07](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/d8b8c46a-3a8b-458c-8c75-03f6577f5d17)
![Screenshot from 2024-03-26 06-05-53](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/ab396c91-430f-41ca-b9a4-dda72a91c4d5)
![Screenshot from 2024-03-26 06-08-51](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/aa1d50b0-9cb1-45bf-a641-b64842166b48)

Since more fanout is causing more delay we can add parameter to reduce fanout and do synthesis again

Commands to include new lef and perform synthesis 

```tcl
# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a -tag 25-03_18-52 -overwrite

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Command to set new value for SYNTH_MAX_FANOUT
set ::env(SYNTH_MAX_FANOUT) 4

# Command to display current value of variable SYNTH_DRIVING_CELL to check whether it's the proper cell or not
echo $::env(SYNTH_DRIVING_CELL)

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```

Commands run final screenshot

![Screenshot from 2024-03-26 06-20-29](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/c5869cf5-ab95-46f9-9fd1-dc91e92455d8)

Commands to run STA in another terminal

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Command to invoke OpenSTA tool with script
sta pre_sta.conf
```

Screenshots of commands run

![Screenshot from 2024-03-26 06-22-31](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/0f3247fc-aaad-4e98-b23e-bdc9a361d08c)
![Screenshot from 2024-03-26 06-22-41](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/9fcc3975-0e03-4515-aee5-04b7c1c6a315)
![Screenshot from 2024-03-26 06-22-50](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/e19db773-3995-4af4-b0a3-188b02fdf454)
![Screenshot from 2024-03-26 06-23-01](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/c22c85fb-1a94-4639-a731-da4c364d1a78)

#### 10. Make timing ECO fixes to remove all violations.

OR gate of drive strength 2 is driving 4 fanouts

![Screenshot from 2024-03-26 06-55-46](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/73c58332-a212-4a24-b853-e8cae3b385a6)

Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4

```tcl
# Reports all the connections to a net
report_net -connections _11672_

# Checking command syntax
help replace_cell

# Replacing cell
replace_cell _14510_ sky130_fd_sc_hd__or3_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```

Result - slack reduced

![Screenshot from 2024-03-26 07-02-44](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/c50c6023-1836-468d-a8c3-955b02e4224c)
![Screenshot from 2024-03-26 07-04-08](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/6059a511-9977-4d9e-8dde-8939f0e1b8f7)
![Screenshot from 2024-03-26 09-42-15](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/df3dba3c-9445-4d51-9842-bf4410f05cf5)
![Screenshot from 2024-03-26 07-07-50](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/f141d1ff-4e21-4f9a-ab47-19579a686ac4)

OR gate of drive strength 2 is driving 4 fanouts

![Screenshot from 2024-03-26 09-46-23](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/3f66eed9-c43d-483e-ab85-21d70452b81f)

Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4

```tcl
# Reports all the connections to a net
report_net -connections _11675_

# Replacing cell
replace_cell _14514_ sky130_fd_sc_hd__or3_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```

Result - slack reduced

![Screenshot from 2024-03-26 09-49-29](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/d896c1fa-078e-4fe1-8e4d-60fae870d672)
![Screenshot from 2024-03-26 09-50-13](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/a21abd89-b92a-4d28-b065-6b8b523026de)
![Screenshot from 2024-03-26 09-50-33](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/9b961126-3de7-450e-9789-5009ca7f6a16)

OR gate of drive strength 2 driving OA gate has more delay

![Screenshot from 2024-03-26 10-22-10](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/7669eeef-7b93-4cfd-a152-17eec772496a)

Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4

```tcl
# Reports all the connections to a net
report_net -connections _11643_

# Replacing cell
replace_cell _14481_ sky130_fd_sc_hd__or4_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```

Result - slack reduced

![Screenshot from 2024-03-26 10-29-31](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/c63cf6a1-582d-43e5-907f-e292b94cc086)
![Screenshot from 2024-03-26 10-29-55](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/6495c2e8-78c8-41d2-b783-628f36b982c5)

OR gate of drive strength 2 driving OA gate has more delay

![Screenshot from 2024-03-26 10-32-27](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/4185dc98-a065-49d3-a92f-fb4b02f23ee5)

Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4

```tcl
# Reports all the connections to a net
report_net -connections _11668_

# Replacing cell
replace_cell _14506_ sky130_fd_sc_hd__or4_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```

Result - slack reduced

![Screenshot from 2024-03-26 10-36-59](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/be71b3a4-efe1-4239-be5a-28ccebd2b7f4)
![Screenshot from 2024-03-26 10-37-14](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/2ae759b3-f9a7-478f-a34a-3f8706347eca)

Commands to verify instance `_14506_`  is replaced with `sky130_fd_sc_hd__or4_4`

```tcl
# Generating custom timing report
report_checks -from _29043_ -to _30440_ -through _14506_
```

Screenshot of replaced instance

![Screenshot from 2024-03-26 10-43-04](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/970b3cb7-fe10-4b5e-99c9-85059714f8f2)

*We started ECO fixes at wns -23.9000 and now we stand at wns -22.6173 we reduced around 1.2827 ns of violation*

#### 11. Replace the old netlist with the new netlist generated after timing ECO fix and implement the floorplan, placement and cts.

Now to insert this updated netlist to PnR flow and we can use `write_verilog` and overwrite the synthesis netlist but before that we are going to make a copy of the old old netlist

Commands to make copy of netlist

```bash
# Change from home directory to synthesis results directory
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/25-03_18-52/results/synthesis/

# List contents of the directory
ls

# Copy and rename the netlist
cp picorv32a.synthesis.v picorv32a.synthesis_old.v

# List contents of the directory
ls
```

Screenshot of commands run

![Screenshot from 2024-03-26 10-54-15](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/90c90853-0664-44d7-8ff2-573621935870)

Commands to write verilog

```tcl
# Check syntax
help write_verilog

# Overwriting current synthesis netlist
write_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/25-03_18-52/results/synthesis/picorv32a.synthesis.v

# Exit from OpenSTA since timing analysis is done
exit
```

Screenshot of commands run

![Screenshot from 2024-03-26 11-02-19](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/c6c8ce7f-5219-41bc-a5a8-47b0e1c62c7c)

Verified that the netlist is overwritten by checking that instance `_14506_`  is replaced with `sky130_fd_sc_hd__or4_4`

![Screenshot from 2024-03-26 11-01-25](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/1ddb66da-64cb-426d-8d78-30370c63dacb)

Since we confirmed that netlist is replaced and will be loaded in PnR but since we want to follow up on the earlier 0 violation design we are continuing with the clean design to further stages

Commands load the design and run necessary stages

```tcl
# Now once again we have to prep design so as to update variables
prep -design picorv32a -tag 24-03_10-03 -overwrite

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Follwing commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or

# Now we are ready to run placement
run_placement

# Incase getting error
unset ::env(LIB_CTS)

# With placement done we are now ready to run CTS
run_cts
```

Screenshots of commands run

![Screenshot from 2024-03-26 11-33-14](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/a0f223bc-3cbb-4bb1-8c7e-d800f1a4efd7)
![Screenshot from 2024-03-26 11-33-22](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/9bd610a8-fd5e-452b-94b2-5a5fc76db5e9)
![Screenshot from 2024-03-26 11-35-40](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/283d9b5e-fc48-45a3-8548-c68fc8966f46)
![Screenshot from 2024-03-26 11-36-16](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/af866818-af6f-4bc4-96c0-cf3c99839003)
![Screenshot from 2024-03-26 11-37-36](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/55aa5130-8a9f-48d6-abc8-7aace68b58c8)
![Screenshot from 2024-03-26 11-38-26](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/755fc79a-2a77-402b-aa11-33799e31ee09)
![Screenshot from 2024-03-26 11-39-53](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/45fdc85e-cc32-4b08-954e-bd78dcec0891)
![Screenshot from 2024-03-26 12-00-48](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/5deb6b89-c81e-4b4d-a225-badc1f7c7299)

#### 12. Post-CTS OpenROAD timing analysis.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD

```tcl
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/24-03_10-03/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Check syntax of 'report_checks' command
help report_checks

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow
exit
```

Screenshots of commands run and timing report generated

![Screenshot from 2024-03-26 12-55-00](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/ee26dfa7-715a-4df7-97e7-4c6e54d16522)
![Screenshot from 2024-03-26 12-57-40](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/e04a4dfd-000c-406b-bdaa-f5314c4eedef)
![Screenshot from 2024-03-26 12-58-12](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/ac27d567-1b72-444d-a638-9be2db677ae2)
![Screenshot from 2024-03-26 13-09-57](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/e63cac70-072d-4453-992e-076b94f8f1a2)

#### 13. Explore post-CTS OpenROAD timing analysis by removing 'sky130_fd_sc_hd__clkbuf_1' cell from clock buffer list variable 'CTS_CLK_BUFFER_LIST'.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis after changing `CTS_CLK_BUFFER_LIST`

```tcl
# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Removing 'sky130_fd_sc_hd__clkbuf_1' from the list
set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Checking current value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Setting def as placement def
set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/placement/picorv32a.placement.def

# Run CTS again
run_cts

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/24-03_10-03/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts1.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/24-03_10-03/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Report hold skew
report_clock_skew -hold

# Report setup skew
report_clock_skew -setup

# Exit to OpenLANE flow
exit

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Inserting 'sky130_fd_sc_hd__clkbuf_1' to first index of list
set ::env(CTS_CLK_BUFFER_LIST) [linsert $::env(CTS_CLK_BUFFER_LIST) 0 sky130_fd_sc_hd__clkbuf_1]

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)
```

Screenshots of commands run and timing report generated

![Screenshot from 2024-03-26 13-42-03](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/2191f13b-ff76-4281-9d3c-52cbf12f9142)
![Screenshot from 2024-03-26 13-45-28](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/3dcd6efe-d31a-4f72-aede-be1cdd7b078f)
![Screenshot from 2024-03-26 13-48-01](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/2cb2c6fd-1e8c-4921-8553-7dcfb51258e5)
![Screenshot from 2024-03-26 13-48-13](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/70353613-86f2-432c-a1d8-821083e8c209)
![Screenshot from 2024-03-26 13-50-12](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/bf6c116b-e31c-4dce-b04f-a75430b1d03b)
![Screenshot from 2024-03-26 13-53-30](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/a26e9d23-d448-4512-8676-8a2b3fb22572)

## Section 5 - Final steps for RTL2GDS using tritonRoute and openSTA (25/03/2024 - 26/03/2024)

### Theory

### Implementation

* Section 5 tasks:-
1. Perform generation of Power Distribution Network (PDN) and explore the PDN layout.
2. Perfrom detailed routing using TritonRoute.
3. Post-Route parasitic extraction using SPEF extractor.
4. Post-Route OpenSTA timing analysis with the extracted parasitics of the route.

* All section 5 logs, reports and results can be found in following run folder:

[Section 5 Run - 26-03_08-45](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/tree/main/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/26-03_08-45)

#### 1. Perform generation of Power Distribution Network (PDN) and explore the PDN layout.

Commands to perform all necessary stages up until now

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Following commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or

# Now we are ready to run placement
run_placement

# Incase getting error
unset ::env(LIB_CTS)

# With placement done we are now ready to run CTS
run_cts

# Now that CTS is done we can do power distribution network
gen_pdn 
```

Screenshots of power distribution network run

![Screenshot from 2024-03-26 14-22-34](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/dd916806-6688-4c96-b1af-156b2d4acfe6)
![Screenshot from 2024-03-26 14-22-46](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/1f6ade75-93c2-4b76-bc46-77d1d532a84c)

Commands to load PDN def in magic in another terminal

```bash
# Change directory to path containing generated PDN def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/26-03_08-45/tmp/floorplan/

# Command to load the PDN def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &
```

Screenshots of PDN def

![Screenshot from 2024-03-26 14-30-52](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/b13997fd-296c-4213-b4f9-8f66a7375e47)
![Screenshot from 2024-03-26 14-32-24](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/79b5f158-acf4-4065-a0ec-61007ab465d0)
![Screenshot from 2024-03-26 14-34-03](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/bee921ce-03d5-49fb-a9fc-bcc6e3402f8c)

#### 2. Perfrom detailed routing using TritonRoute and explore the routed layout.

Command to perform routing

```tcl
# Check value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Check value of 'ROUTING_STRATEGY'
echo $::env(ROUTING_STRATEGY)

# Command for detailed route using TritonRoute
run_routing
```

Screenshots of routing run

![Screenshot from 2024-03-26 14-48-29](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/f166be26-f49a-4001-abee-ce395857990f)
![Screenshot from 2024-03-26 15-38-39](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/c0c8f372-0293-4fdd-a0a3-691f164e7bed)
![Screenshot from 2024-03-26 15-29-38](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/70a99289-06ea-4eb8-b3b0-4147395c6f9c)

Commands to load routed def in magic in another terminal

```bash
# Change directory to path containing routed def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/26-03_08-45/results/routing/

# Command to load the routed def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &
```

Screenshots of routed def

![Screenshot from 2024-03-26 15-33-12](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/6eade230-eb96-4d7b-b7a9-7a7db9c2c8b7)
![Screenshot from 2024-03-26 15-30-36](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/b1900c55-7470-41b2-8b4f-3af871494d99)
![Screenshot from 2024-03-26 15-31-29](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/5faaca5b-fb6e-4abd-946d-6531b35489b8)
![Screenshot from 2024-03-26 15-32-20](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/594ef79b-4755-4934-a087-33fb24996526)

Screenshot of fast route guide present in `openlane/designs/picorv32a/runs/26-03_08-45/tmp/routing` directory

![Screenshot from 2024-03-26 15-41-18](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/1dc38a57-03c9-45c3-acdb-063731a86433)

#### 3. Post-Route parasitic extraction using SPEF extractor.

Commands for SPEF extraction using external tool

```bash
# Change directory
cd Desktop/work/tools/SPEF_EXTRACTOR

# Command extract spef
python3 main.py /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/26-03_08-45/tmp/merged.lef /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/26-03_08-45/results/routing/picorv32a.def
```

#### 4. Post-Route OpenSTA timing analysis with the extracted parasitics of the route.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD

```tcl
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/26-03_08-45/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/26-03_08-45/results/routing/picorv32a.def

# Creating an OpenROAD database to work with
write_db pico_route.db

# Loading the created database in OpenROAD
read_db pico_route.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/26-03_08-45/results/synthesis/picorv32a.synthesis_preroute.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Read SPEF
read_spef /openLANE_flow/designs/picorv32a/runs/26-03_08-45/results/routing/picorv32a.spef

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow
exit
```

Screenshots of commands run and timing report generated

![Screenshot from 2024-03-26 23-16-16](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/72ef3e8d-7ca2-4b60-89ea-de053f9c2902)
![Screenshot from 2024-03-26 23-17-09](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/5cac9ce5-420a-4eaa-b5f4-09286701e550)
![Screenshot from 2024-03-26 23-17-32](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/7d809f14-66b6-4dd6-8161-2ad8371cfaf9)
![Screenshot from 2024-03-26 23-17-56](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/64ccb1d8-74aa-42b0-88d4-a0f9588d2ca2)

# Certificate

![Digital-VLSI-SoC-Design-and-Planning-Certificate](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/6f292f50-f95b-471c-8dac-bf0ea7345c9b)

# Acknowledgements

* [Kunal Ghosh](https://github.com/kunalg123), Co-founder, VSD Corp. Pvt. Ltd.
* [Nickson P Jose](https://github.com/nickson-jose), Physical Design Engineer, Intel Corporation.
* [R. Timothy Edwards](https://github.com/RTimothyEdwards), Senior Vice President of Analog and Design, efabless Corporation.


</details>










































































  



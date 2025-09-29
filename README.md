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
🔹 Added a 10-bit DAC to enable digital-to-analog conversion.
🔹 Enabled interfacing with external analog systems (e.g., 📺 televisions, 📱 mobile phones) for audio/video outputs.
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
</details>

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



























  



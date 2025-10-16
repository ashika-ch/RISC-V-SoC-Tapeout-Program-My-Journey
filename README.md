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

2️⃣ ID–VGS Characteristics (Transfer Characteristics) 🔄

Purpose:
To study how the drain current (ID) changes with the gate-to-source voltage (VGS) while keeping VDS constant.

Why it’s done:

Helps determine threshold voltage (VTH) accurately — the point where the transistor just starts to conduct 🚦.

Shows the transconductance (gm), which measures how effectively the gate voltage controls the drain current.

Important for understanding switching speed and gain in CMOS logic circuits.

In short: ID–VGS gives insight into the input control behavior of the MOSFET — how “strongly” a transistor turns ON or OFF ⚙️.

3️⃣ Voltage Transfer Characteristics (VTC) of CMOS Inverter 🔁

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

4️⃣ Static and Dynamic Power Dissipation 🔥

Purpose:
To measure how much power is consumed by CMOS circuits in both steady (static) and switching (dynamic) states.

Why it’s done:

Static power = leakage current when the circuit is idle 💤

Dynamic power = charging/discharging of load capacitances during switching ⚙️

Helps optimize circuits for low-power design, crucial in portable electronics 📱 and IoT devices 🌐.

In short: This experiment shows why CMOS is energy-efficient — consuming almost no power when idle and scaling well with voltage/frequency ⚡.

5️⃣ Delay and Switching Speed ⏱️

Purpose:
To analyze how fast a CMOS inverter or logic gate responds when input changes.

Why it’s done:

Measures propagation delay (tp) and rise/fall times (tr, tf).

Helps evaluate circuit speed and timing performance ⏩.

Crucial for designing high-frequency and high-speed digital systems.

In short: Delay analysis reveals how quickly CMOS logic transitions between logic ‘0’ and ‘1’ — key for modern GHz processors ⚙️💨.

<img width="1219" height="794" alt="Screenshot from 2025-10-13 19-29-50" src="https://github.com/user-attachments/assets/926a124f-c70a-483c-96a4-8e918a44de92" />


<summary> SPICE Netlists / Code </summary>

🧾 What is a SPICE Netlist?

A SPICE (Simulation Program with Integrated Circuit Emphasis) netlist is a text-based description 📝 of an electronic circuit.
It contains:

The circuit elements (transistors, resistors, capacitors, etc.)

Their connections (nodes) 🔌

The models for devices (like NMOS, PMOS)

The simulation commands to analyze the circuit 📊

SPICE simulations help us study how CMOS circuits behave before actually fabricating them 🧪💡.

🔋 1️⃣ ID–VDS Characteristics (Output Characteristics)

This experiment studies how drain current (ID) varies with drain–source voltage (VDS) for different gate voltages (VGS).

🧠 Purpose:

To identify the transistor’s operating regions: cutoff, linear, and saturation 🚦.

🧩 SPICE Netlist Example:
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


📈 This simulation plots ID vs VDS for several VGS values — showing how current increases and saturates as VDS increases.

⚡ 2️⃣ ID–VGS Characteristics (Transfer Characteristics)
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

🔁 3️⃣ CMOS Inverter – Voltage Transfer Characteristics (VTC)
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


📈 The output will be a sharp transition — showing logic inversion and identifying key points like VM, noise margins, and VOH/VOL 🧭.

⏱️ 4️⃣ Transient Analysis (Switching Behavior)
🧠 Purpose:

To study how fast a CMOS inverter switches when the input changes over time — i.e., propagation delay, rise/fall time ⏩.

🧩 SPICE Netlist Example:
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

🌡️ 5️⃣ Parameter / Process Variation Simulation
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

<summary>Plots & Figures</summary>

🔋 1️⃣ ID vs VDS Characteristics (Output Characteristics)

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

🔄 2️⃣ ID vs VGS Characteristics (Transfer Characteristics)

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

🔁 3️⃣ CMOS Inverter – Voltage Transfer Characteristics (VTC)

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

⏱️ 4️⃣ Transient Waveforms (Dynamic Switching Behavior)

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









<details>










































































  



# RISC-V-SoC-Tapeout-Program-My-Journey
👩‍💻 Participant: Ashika C H 
<br>
📍 Program: RISC-V Reference SoC Tapeout Program (VSD) 
<br>
🏭 Impact: Part of India’s largest collaborative open-source tapeout with 3500+ participants .
<br>
This repository tracks my week-by-week progress in the SoC Tapeout Program, covering everything from RTL design to GDSII.

<details>
	<summary>Day 0 - Tools Installation </summary>

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

4photos 





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

photo 

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

image

Hierarchy is preserved. sub_module1 and sub_module2 are instantiated separately in the synthesized Verilog netlist. Rather than seeing AND or OR gate, we see sub_modules when we run the command 'show' as shown in the screenshot.

image

If we look into the sub_module2 in synthesized netlist 'multiple_modules_hier.v', we see that rather than OR gate, the inputs a & b, pass through the inverter and then NAND gate. It is because in CMOS, stacking PMOS, which happens in 'OR' gate is bad as PMOS has lower mobility and always have to be wider to get some meaningful output. The next step is to check .lib file for the answer.

### Flat Synthesis
The design can be flattened by using the command `flatten`.

Screenshot shows the command, synthesized netlist and the logical diagram.

image

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

image

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








  



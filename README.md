# RISC-V-SoC-Tapeout-Program-My-Journey
👩‍💻 Participant: Ashika C H 
📍 Program: RISC-V Reference SoC Tapeout Program (VSD) 
🏭 Impact: Part of India’s largest collaborative open-source tapeout with 3500+ participants  .
This repository tracks my week-by-week progress in the SoC Tapeout Program, covering everything from RTL design to GDSII.


# Day 0 - Tools Installation
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

<img width="777" height="496" alt="Image" src="https://github.com/user-attachments/assets/07c16b28-8c3c-42fd-8915-6742bbfdb04c">

## Iverilog
```
$ sudo apt-get install iverilog
```

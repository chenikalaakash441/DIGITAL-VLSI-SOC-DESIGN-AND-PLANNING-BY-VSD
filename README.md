# DAY 1 -Inception of open-source EDA,OpenLANE and Sky130 PDK
## How to talk to computers
### Introduction to QFN-48 package,chip,pads,core,die and IPs
A package contains chip, that chip is done using silicon wafer with selecting some area as a die, in that die core is selected with some area and around core pads are placed such as IO pads.Core contains macros and foundry ips and standard cells.
### Introduction to RISC-v
risc-v architecture that is implemented (picorv32 cpu core) in verilog and then layout is generated using qflow
### From software applications to hardware
flow is as follows
Application software or Apps => system software => hardware
in system software the flow is as follows
* RISC-v instruction set architecture (RTL snippet of hardware which understands instructions like x6,x10
*  RTL and synthesis of risc-v based cpu core -picorv32 (gives synthesized netlist of RTL)
*  physical design implementation of picorv32
## soc design and openlane
### simplified RTL to GDS flow
the flow is as follows
synthesis (RTL as input) => Floorplan + powerplan => placement => CTS (PDK as another input) => route => signoff => GDS2
### Design preparation step
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/5a9a6b60504bc9ad71acc1990eda7cac72da1162/Screenshot%202025-02-17%20162734.jpg)
### running synthesis
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/3460f6621b9f17eee70312bb10b818361eec904e/syncomplete.jpg)
### steps to characterize synthesis results
flop ratio:dxtp_4 =1634/17323=0.094

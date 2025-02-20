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
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/04097f3495ab985864c79377393bcd0fb7ef1a61/dffcount.jpg)
after running the command run_synthesis it gives synthesized netlist
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/4e11b4aa02b0265c2612ca6bebfc1d3302c1a6f4/synthesizednetlist.jpg)

# DAY 2- Good floorplan vs bad floorplan and introduction to library cells
## Chip floorplan considerations
* utilization factor=(Area occupied by netlist/Total area of the core)
* Aspect ratio=height/width

## concept of preplaced cells
To design a preplaced cells the netlist of some logic is created as a blocks with extended io pins and they treated as black boxesand these black boxes are called as ip's or modules which includes memory,clock-gating cell,comparator etc.

## Decoupling capacitors
* These are used to get the vdd to the macro without any voltage drop in the wire of power supply vdd by placing the capacitor parallel to the macro and power supply.
* to get rid of undefined region of noise margin
* The preplaced cells are surrounded with decoupling capacitors

## Powerplanning
It is done to get rid of Ground Bounce and voltage droop

## Pin placement
The input pins are placed at left side and the output pins are placed at right side with some random equidistant space.

## steps to run floorplan using openlane
after running synthesis run_floorplan command is used to run the floorplan.(init_floorplan => place_io => tap_decap_or)
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/d6817ba4eea505ac3c80c3c47aa34bb32ef1c897/floorplancomplete.jpg)
after completion of floorplan the floorplan def file is created.
![image alt](

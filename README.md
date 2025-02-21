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
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/73f53bca6d5a665357aea0a07714ecd40191c069/def.jpg)
the def file is shown below
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/9a0ec658f2269c34bb202319b8639ec9ee2100e1/deffile.jpg)

## Review floorplan layout in Magic
The floorplan layout is generated using magic layout tool by reading tech file and lef file and def file.
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/04cef00dd79616802df96be75d4187a8012c37a7/magiclayout.jpg)

## Netlist binding and initial place design
the netlist is binded with physical cells and the logic cells are placed in the core area

## Optimize placement using estimated wire-length and capacitance
* The placement is optimized by estimating wire-length and capacitance
* Repeaters (data buffers) are inserted in the placement stage to avoid loss of signal i.e., to maintain signal integrity between logic cells  and logic cells and between inputs and logic cells and between logic cells and output pins.

## Need for libraries and characterization
The technology libraries which designed based on process,voltage and temperature in three formats such as minimum and maximum and typical conditions of above three parameters which gives the information about physical cells.
Library characterization and modelling: The logic cells are designed with different sizes and different functionality and different Vt.

## Congestion aware placement using Replace
placement is done using command run_placement and then it is viewed using magic layout.
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/70678743239e16d4bb31558d892b62211ffa0e09/placementmagic.jpg)
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/c4ed1528909f657ee2b1bb91987b63fa9435c498/placementofcells.jpg)

## cell design and characterization flows
### inputs for cell design flow
It contains PDK's: 
* DRC & LVS rules- which are tech file with lambda based design rules
* spice models - designed with spice model parameters for example pmos and nmos transistors spice model netlist is designed by using these spice model parameters.
* library and user defined specs - which contains info of cell height,supply voltage,metal layers,pin locations,drawn gate length.

### Design steps
* circuit design - the required ratio of pmos vs nmos transistor size can be derived such that Vm is set.
* Layout design - Art of layout such as Euler's path and stick diagram
* characterization (timing,power,noise)

### outputs
* circuit description language
* GDS2,LEF,extracted spice netlist(.cir)
* Timing,noise,power,.libs,function

### characterization flow
1) To reading the model files
2) To read the extracted spice netlist
3) To define or recognize the behavior of buffer
4) To read in the sub ckt of the inverter
5) Attach the power sources
6) To apply the stimulus
7) To give neccessary output load capacitance
8) To give neccessary simulation command (.trans,.dc)
Feeding all these steps into GUNA softwareit generates timing,noise,power,.libs function models 

## General timing characterization parameters
### Timing threshold defnitions
* slew_low_rise_thr , slew_low_fall_thr = 20%
* slew_high_rise_thr , slew_high_fall_thr = 80%
* in_rise_thr , in_fall_thr , out_rise_thr , out_fall_thr = 50%

### Propagation delay
propagation delay: time(out_*_thr) - time(in_*_thr)
time(in_rise_thr) = 50%
time(out_fall_thr) = 50%

### Transition time
transition time: Time(slew_high_rise_thr)-time(slew_low_rise_thr)
input slew and output slew are calculated using these timing threshold definitions.

# Day 3 - Design library cell using Magic layout and ngspice characterization
## spice deck creation for cmos inverter
VTC-spice simulation
* component cnnectivity
* component values
* identify nodes
* name nodes

## spice simulation lab for cmos inverter
the cmos inverter is simulated after doing the above steps. Simulation is done using .trans or .dc sweep for VTC

## switching threshold Vm
the switching threshold Vm is defined by the transistor sizes of pmos and nmos where length is fixed and widths are varied. It means the width of pmos transistor is some multiple of width of nmos transistor(Wp=2.5Wn).The Vm is selected in such a way that the VTC curve should be smooth it describes the delay of the inverter.

## Steps to gitclone vsdstdcelldesign
The vsdstdcelldesigned is gitcloned by using github link of vsdstdcelldesign
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/e672a9e730851c38aa0a02763d0a6dfc350593c7/stdinverterlayout.jpg)

## Steps to create std cell layout and extract spice netlist
The spice deck of inverter is extracted from vsdstdcelldesign of sky130_inv.mag file by using command of ext2spice
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/505e2184dfbbd5cd4a832384e01cb4fcfea38954/extractingparasitics.jpg)
The generated spice deck is shown below
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/7ea1d65bd2e538a457bc33b8b198bfa62f5a75dd/spicedeck.jpg)

## Steps to create final spice deck using sky130 tech
the updated spice deck for simulation is shown below
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/55db0fc63a92c9a2b9c4732671015e1f7aa4c8da/editedspicedeck.jpg)

## steps to characterize inverter using sky130 model files
The characterization of inverter is done by doing trans simulation 
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/5b71792f9917db7334aeabfcb1e9bc77355aafae/invsimulation.jpg)
proopagation delay of inverter is obtained by using magic layout tool with definition of propagation delay is 0.058ns=58ps.
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/204ded034b58ac110f91619f26eef8747dc949c5/propagationdelay.jpg)

# Day 4 - Prelayout timing analysis and importance of good clock tree
## Steps to convert grid info to track info
the layout of inverter is modified with the grids by using grid command with metal informations of x and y coordinates with some spacing after generating of grids check that the contacts are placed at the interconnect of vertical and horizontal grid lines.
the cell dimensions are placed with in these grids i.e., odd multliple of height and widths of metals dimensions.
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/33c0bb9b8f4c77b26344c2ddcd0dec0fffc64b78/withgrid.jpg)
The track info is shown below
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/5492598ad9fde5e2b3fcd8e124a51b776b71f3ae/traksinfo.jpg)

## steps to convert magic layout to std cell lef
the lef file of std cell of inverter is obtained by doing write lef in tickon window
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/aa6928137baf39eae2185c5ad1c852da0b256ea5/vsdinvleffile.jpg)

## steps to configure synthesis settings to fix slack and include vsdinv
* after synthesis of picorv32a it is violated with slack but the floorplan and placement is done.
* now the synthesis is done again and trying to fix the slack with the setting MAX_CAP_FANOUT to 4 and slack is not met and the lef file of sky130_vsdinv.lef is inserted into picorv32a design and acts as lef input.

## re-synthesis and re-floorplan and re-placement with vsdinv
re-Synthesis
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/c8229110c59aa40d9241e441304e4bb2a1bb37e0/synthesisofinvlefsuccess.jpg)

re-floorplan
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/1bf390cd90a362bb42553bf43ff1cdea904817ed/refloorplandone.jpg)

re-placement - the placement def file is shown below which includes sky130_vsdinv cell.
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/60f715e13fd106a549c4330e176b7cdb71b53429/invplacementdeffile.jpg)


## Steps to configure openSTA for post-synth timing analysis
* the pre_sta_config.tcl is created giving synthesized netlist as input
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/c5f13c9b90ef1266ed3b72e3c03c33bd4c3706b1/stafile.jpg)

* the slack is checked it is not met
* after analysing net connections and the cells, the cells with more delay are replaced with large size cells such that slack is reduced and it is obtained as -21.85 from -26.64
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/2313dadd8edc65f75a17f6aa7b7175d0ba3fd9e5/slack21afternewsynth.jpg)
* this synthesized netlist is updated and overwrited in the synthesis results of design.

## re-floorplan and re-placement and cts done
* after doing re-floorplan and re-placement with the updated synthesized netlist the cts is done using command run_cts
* the slack did not met it shows -7.63 

## Analyze timing with real clocks using openSTA and with right timing libraries and to observe impact of bigger cts buffers on setup and hold timings
* After cts the openroad is open for STA
* In that after reading lef,def,tech libraries,db,verilog,sdc and linking design with picorv32a and then setting propagated clock and doing report checks of timing.
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/1f496876d608aa73ab5713a32c39bcbc589af762/openroadflow.jpg)

* for min and max libraries the slack is 4.28
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/c3610e91822a2ae0780f5a3098cdeb6a9d9e27b7/slackofmimmaxlib.jpg)

* for typical library the slack is -5.22
* after removing sky130_fd_sc_hd_clkbuffer1 for typical library the slack is -5.15
* after removing sky130_fd_sc_hd_clkbuffer1 for min max libraries the slack is 3.6395 for setup and for hold is 1.9582
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/119dd6a3a5538a2f34233b5601f7aaf555217c55/slackofminmaxwithoutbuff1.jpg)

# Day 5 - Final steps for RTL2GDS using tritonRoute and openSTA
## steps to build power distribution network
The power distribution network for the design is implemented using the command gen_pdn but before this make sure that the current def file should be in cts stage.
![image alt](https://github.com/chenikalaakash441/DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING-BY-VSD/blob/42bc9f7125b8c041de513506ebd50e2b02393a13/pdn_gen.jpg)

The power distribution network layout is shown below
![image alt](

## Detailed routing using Triton-route
The current def file should be in pdn stage and set the ROUTING_STRATEGY should be 0 and then run the command run_routing.
![image alt](

The routing layout is shown below
![image alt](

## spef extraction
The spef file is extracted using following command shown in below
![image alt](

the spef file is shown below
![image alt](




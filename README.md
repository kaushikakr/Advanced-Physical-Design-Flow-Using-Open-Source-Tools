### Sky130 Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK
#### Familiarity to open-lane framework


*1.3. SKY130_D1_SK3 - Get familiar to open-source EDA tools*
* L1. OpenLANE directory structure
Desktop/work/tools/open_lane_working_dir/

 ![Test Image 1.3.1](IMAGES/image1.3.1.png)

inside pdk folder, 
skywater-pdk has timing libraries, LEF file, TECH lib, cell LEF
open_pdk - Silicon foundry file are made for commercial EDA tools, Open pdk helps to make the pdk compatible with open EDA tools. They are a set of scripts and files to make it compatible, like magic, NETgen, etc tools.

 ![Test Image 1.3.1.2](IMAGES/image1.3.1.2.png)

sky130A - made compatibe for open source env. sky130A is open source variants. 

sky130A has libs.ref(timing, LEF, tech LEF- specific to the process) and libs.tech(specific to tool)

![Image 1.3.1.3](IMAGES/1.3.1.3_libs_ref.png)
![Image 1.3.1.4](IMAGES/1.3.1.4_libs_tech.png)

we work on sky130_fd_sc_hd(processname_foundryname_stdcell_variantofPDK).

libs.ref/sky130_fd_sc_hd has techlef- layer info; mag file, lef, lib [image1.3.1.5]- timing files of all process corners. just LEF is cell LEF, tech LEF(tlef) is technology LEF.

![Image 1.3.1.5](IMAGES/1.3.1.5.png)

Inside openLANEworkdir, where we are working on. Invoking openLANE tool from this dir. Desktop/work/tools/open_lane_working_dir/openlane

* L2. Design prep

./flow.tcl -interactive(for step by step flow run)

./flow.tcl -design <design_name> for running the complete flow

To import all the packages required to run the flow: 
%package require openlane 0.9

Inside openlane/designs - there are many designs built into openlane. 
We are doing for picorv32a: src, sky130a_sky130_fd_sc_hd_config.tcl

src file - verilog file of rtl netlist & sdc info.
config.tcl[image1.3.2.1] - bypassed any configurations that are set in default. 

Whwn we run our custom design, sky130a_sky130_fd_sc_hd_config.tcl won't be there. 
But we need to create config.tcl file.

order of precedence(priority order), default value set by open lane < config.tcl < sky130a_sky130_fd_sc_hd_config.tcl(optional file)



* L3. Run Synthesis

%prep -design <design_name>

here, %prep -design picorv32a

![Image 1.3.3.1](IMAGES/1.3.3.1.png)

The new **runs** directory gets created in *openlane/designs/picrorv32a*.

Inside runs/timestamp/, tmp file has 

In the merged.lef file, we have tlef info(layer, wire, vias), cell level lef info(macro).
merged lef has both the cell lef as well as tech lef.

Inside runs/timestamp, we have a config.tcl. In the ENV: It has pdk, lef info, merged.lef, tracks info, tlef info, synthesis takes it's library info from pdks/sky130A/libs.ref/sky130A_fd_sc_hd/lib/file.lib, MIN and MAX libraries.

In open lane, changes can be made to the config file on the fly. 

Eg: Core utilisation change can be done.

%run_synthesis

will run the yosys synthesis as well as the abc.

![Test Image 1.3.3.2](IMAGES/1.3.3.2.png)

So now the STA, synthesis and abc run are done.

* L4. Open Lane git results

*https://github.com/efabless/openlane.git*

* L5. Characterising Synthesis results

We can find the report of chip area, cell count, etc.

In the runs, we see the updated files in synthesis folder as we are done with the synthesis process. This will have the synthesized netlist /\*.v with all the mappings done by abc.

To check the design stats, it's in the *reports/synthesis/yosys_2.stat.rpt* 

To check pre-layout timing report, it's in the *reports/synthesis/opensta_main.timing.rpt* 



![Test Image 1](IMAGES/openLANEflow.png)

### Sky130 Day 2 - Good floorplan vs bad floorplan and introduction to library cells
*2.1. SKY130_D2_SK1 - Chip Floor planning considerations*

* L6. Steps to run floorplan using openlane

Setting the die area, core area, aspect ratio, utilization ratio, I/O cells, power distribution network.

There are a lot of switches to adjust the flow direction. 

Switches:- 
In openlane/configurations,
when we open readme.md, 
image 2.1.6.1-synthesis switch
image 2.1.6.2-floorplan switch
image 2.1.6.3
image 2.1.6.4

These switches need to be set for floorplan and placement, can be found inside openlane/configurations as tcl scripts.

image 2.1.6.5

From the above figure, which is a floorplan.tcl file, 
for example: FP IO mode can set pin positioning equi-distant for 1 and random for 0.

These above config file which are the default settings have lowest priority.

If we open the config.tcl from the designs/picorv32a, 
we have the design file reference, clock period, clock net and port.
The core utilization is set at 65%, the I/O vertical metal as layer 4 and horizontal one as layer 3.

In the default floorplan.tcl file, the core utilization is 50%, VMETAL is 2, HMETAL is 3, whereas in config.tcl it is 65%.

So, running the floorplan now.
%run_floorplan

To check the results, go to current runs folder, then to logs/floorplan and open ioPlacer.log to check the metal layers used.
image2.1.6.5-floorplan.tcl
image2.1.6.6-config.tcl


* L7. Review floorplan files and steps to view floorplan 

And to check the core utilization, go to config.tcl used in th current run.
image2.1.7.1 - metal layer verification
image2.1.7.2 - core utilization check
image2.1.7.3 - sky130 config highest priority file


In the current run, results/floorplan, open the def(design exchange format) file
[image2.1.7.4] we can see the die area co-ordinates, cell orientation

* L8. Review floorplan layout in Magic

To check the layout after floorplan, open magic using the command *magic -T openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130.tech lef read tmp/merged.lef def read results/floorplan/\*floorplan.def &*


We can find the IO pads placed on the metal layers set in the config files. We also see the tap cells, which are meant to avoid the latch up conditions which occur in CMOS devices which are diagonally equi-distance which is configured. Floorplan doesn't consider standard cells but the layout has standard cells present but not placed orderly. 
[image 2.1.8.1]

The power ground network is also created during floorplan.

*2.2. SKY130_D2_SK2 - Library Binding and Placement*

* L5. Congestion aware placement using RePlAce
After floorplan, we are doing a placement which is congestion aware. Timing will be taken care in the later stages.

There are 2 placements, one is global placement which is a coarse placement with no legalisations happening. Legalisation is placement of cells in the std cell rows and no overlaps among cells.


%run_placement

First global placement is done. Reduction of half parameter wire length is the main focus. Iterations happen till the overflow converges.

[image 2.2.5.1]

Here is where the std cell position gets fixed. def file is created in the current run placement folder.

Then open magic with this latest def file.

[image 2.2.5.2]

Floorplan ensures that there is decaps at the std cell boundaries, Tap cells and IO cells correctly placed. Placement ensures that std cells are perfectly placed in the std cell rows.


### Sky130 Day 3 - Design library cell using Magic Layout and ngspice characterization
*3.1. SKY130_D3_SK1 - Labs for CMOS inverter ngspice simulations*

* L0. IO placer revision
* L5. Lab steps to git clone vsdstdcelldesign

*3.2. SKY130_D3_SK2 - Inception of Layout – CMOS fabrication process*
* L8. Lab intro to Sky130 basic layers layout & LEF using inverter
* L9. Lab steps to create std layout & extract spice netlist

*3.3. SKY130_D3_SK3 - Sky130 Tech File Labs*
* L1. To create final SPICE deck using Sky130 tech
* L2. To characterize inverter using sky130 model files
* L3. To introduce Magic tool options and DRC rules
* L4. To introduce Sky130 pdk’s and steps to download labs
* L5. To introduce Magic & steps to load Sky130 tech-rules
* L6. To fix poly.9 error in Sky130 tech-file
* L7. To implement poly resistor spacing to diff and tap
* L8. To describe DRC error as geometrical construct


### Sky130 Day 4 - Pre-layout timing analysis and importance of good clock tree
*4.1. SKY130_D4_SK1 - Timing modelling using delay tables*
* L1. To convert grid info to track info
* L2. To convert magic layout to std cell LEF
* L3. To introduce timing libs & steps to include new cell in synthesis
* L7. To configure synthesis settings to fix slack and include vsdinv

*4.2. SKY130_D4_SK2 - Timing analysis with ideal clocks using openSTA*
* L3. To configure OpenSTA for post-synth timing analysis
* L4. To optimize synthesis to reduce setup violations
* L5. To do basic timing ECO

*4.3. SKY130_D4_SK3 - Clock tree synthesis TritonCTS and signal integrity*
* L3. To run CTS using TritonCTS
* L4. To verify CTS runs

*4.4. SKY130_D4_SK4 - Timing analysis with real clocks using openSTA*
* L3. To analyze timing with real clocks using OpenSTA
* L4. To execute OpenSTA with right timing libraries and CTS assignment
* L5. To observe impact of bigger CTS buffers on setup and hold timing


### Sky130 Day 5 - Final steps for RTL2GDS using tritonRoute and openSTA
*5.2. SKY130_D5_SK2 - Power Distribution Network and routing*
* L1. To build power distribution network
* L2. Steps from power straps to std cel power
* L3. Basics of global and detail routing and configure TritonRoute

*5.3. SKY130_D5_SK3 - TritonRoute Features*
* L4. Final files list post-route

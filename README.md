### Sky130 Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK
#### Familiarity to open-lane framework

*1. SKY130_D1_SK3 - Get familiar to open-source EDA tools*

*1.3. SKY130_D1_SK3 - Get familiar to open-source EDA tools*
* L2. Design prep
* L3. Run Synthesis
* L4. Open Lane git results
* L5. Characterising Synthesis results

### Sky130 Day 2 - Good floorplan vs bad floorplan and introduction to library cells
*2.1. SKY130_D2_SK1 - Chip Floor planning considerations*

* L6. Steps to run floorplan using openlane
* L7. Review floorplan files and steps to view floorplan
* L8. Review floorplan layout in Magic

*2.2. SKY130_D2_SK2 - Library Binding and Placement*

* L5. Congestion aware placement using RePlAce


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

<p align = "center">
<h1 align="center"> Physical Design Workshop using OpenLANE </h1>
</p>
<p align = "center"> 
            <h3 align="center">Complete RTL to GDSII flow using OpenLANE: Open source automated physical design solution.</h3>
</p>
  
<br>
<br>
  
## Day-1: Inception | Theory

### Parts of a fabricated chip

- **Die:** Die refers to a small piece of silicon that contains a single instance of a microchip or microprocessor.
- **Package:** Package is the covering of a chip which protects it from physical damage and corrosion. It also allows mounting of electrical contacts connecting the chip to the Printed Circuit Board (PCB).
- **Core:** Core is the functional part of the chip implemented by user to perform central operations of the chip.
- **Pads:** Pads are specialized cells used for connecting I/O pins or power/ground pins of the chip to the internal circuitry.
- **IP:** IP stands for 'Intellectual Property'. It consists of the peripherals inside the chip that are not designed by user but sourced directly from other vendors.

### Instruction Set Architechture (ISA) | RISC-V ISA

Instruction Set Architecture (ISA) is the language in which user communicates with the microprocessor. It consists of all the 'instructions' a user can ask the processor to perform. These instructions are described using English-like commands. However, they are implemented in hardware using binary language.

Reduced Instruction Set Computer (RISC) uses a reduced ISA i.e., it efficiently implements only the instructions that are frequently used in programs, while the less common operations are implemented as subroutines.
**Eg of RISC-V instruction:** ADD rd, rs1, rs2 : Adds the contents of registers rs1 and rs2, and stores the result in register rd

### How computers work?

Programs written in High Level Languages are converted into Machine Language using softwares called **compilers**.
If the program carries out some specific application while interacting with user, it is called **application software**. Such application software requests the **operating system**, the guard and controller of computer hardware, permission to interact with peripheral devices and access to memory. Once the application software is loaded into memory, in binary language, it can perform the required task.

### Phyical Design Workflow | RTL2GDSII implmentation
---

Physical design in VLSI (Very Large Scale Integration) refers to the process of transforming a logical circuit representation into a physical layout that can be fabricated on a chip.

![](/Images/Day-1/Theory/PD_flow.png)

ASIC physical design consists of following parts:

1. **Synthesis:** Synthesis is the process of implementing the functionality of a RTL using logic gates. It takes in RTL and outputs Gate netlist.

2. **Floorplanning:** Floorplanning involves determining the size and shape of the chip and dividing it into sections. We place macros in appropriate locations and also determine locations where standard cells will be placed in future.
3. **Placement:** Placement involves determining the location of each standard cell on the chip. Placement algorithm tries to minimize total wire length of the chip.
4. **Clock tree synthesis:** This step involves the synthesis of the clock distribution network in the chip. Goal is to have the clock arrive at all sequential clock pins at the same time.
5. **PDN generation:** PDN stands for Power Distribution Network. It refers to the network of power and ground wires that deliver power to the various components of the circuit.
6. **Routing:** Routing involves determining the paths that connect the components using wires. Routing algorithm tries to achieve routing with minimum wire length and minimum wire bends.

### Introduction to OpenLANE 
---

OpenLANE is an open-source automated RTL-to-GDSII (register transfer level to graphic design system II) flow tool for digital ASIC (application-specific integrated circuit) design. It is built on a collection of open-source tools, such as Yosys for synthesis, Magic for physical layout, and OpenROAD for optimization, and is designed to provide a complete, end-to-end solution for ASIC design.

<img src="/Images/Day-1/Theory/openLANE_flow.png"/>

## Day-1: Inception | Labs 
  
 ### Invoking openLANE
 
 Use the following code to invoke openLANE in interactive:
 
 ```
 docker
 ./flow.tcl -interactive
 package require openlane 0.9
 prep -design <design_name> -tag <name_of_run_dir>
 ```
 This will invoke openLANE as shown below:  
 
 <p align="center"> <img src="/Images/Day-1/Labs/Invoking_openLANE.png"/></p>
 
 ### Runnning synthesis
 
 Synthesis can be run using following atomic command:
  ```
  run_synthesis
  ```
  run_synthesis generates the synthesized netlist in <run_tag>/results/synthesis directory as shown below:
  <p align="center"> <img src="/Images/Day-1/Labs/synthesis.png"/></p>
  
  ### Analyzing synthesis results
  
  We can examine the cell stats of synthesized run from reports present in <run_tag>/reports/synthesis/ directory.
  Here, we have calculated flop ratio for the given design:
  
  <p align="center"> <img src="/Images/Day-1/Labs/flop_ratio.png"/></p>
  
 ## Day-2: Floorplanning and Placement | Theory
 
 ### What is floorplanning?
 **Floorplanning** is the process of determining die area of the chip, placing macros and allocating areas for standard cell placement.  
 
 A **die** can be divided into central **core** region for logic cells placement and a peripheral region. The core region is demarcated into different sections during floorplanning. Macros are placed in some sections while others are left for standard cell placement.  
 
 **Target of floorplanning:** Lay down macros in suitable positions on the core based on timing and power considerations. For e.g., a macro frequently receivng data from input pins will be placed close to them as this will reduce delay and wire cap. All this needs to be done while leaving out sufficiently large contiguous blocks for standrad cell placement.
 
 #### Important terminology related to floorplanning
 
 **Utilisation factor:** Utilisation factor is defined as the ratio of area of netlist to the area of core of the die. Usually, it's value is kept close to 50%. This leaves substantial space in the core for CTS and ECO changes.  
 
 **Aspect ratio:** Aspect ratio is the ratio of height of the core to the width of the core. It determines the shape of a chip. Aspect ratio of 1 implies a square-shaped chip.
 
 **Pre-placed cells:** Pre-placed cells (PPCs) are cells that are placed in the core before the automated placement and routing process. These cells usually consists of **foundry IPs** and their location is fixed during floorplanning. It is not altered by placement tool.
 
 **Decoupling capacitors:** Decoupling capacitors help reduce noise and improve power supply performance. They work by temporarily storing electrical charge and then releasing it back into the circuit when needed. Preplaced cells are surrounded by decoupling capacitors during floorplanning.
 
 ### What is powerplanning?
 **Power planning** is done to ensure that each block of the chip has access to a stable and reliable power supply. Power and ground pin locations are determined.
 
 #### Importnant terminology related to powerplanning
 
 **Voltage drop:** Voltage drop is an incident wherein power supplied at an instance's power pin can fall below supply voltage because to intervening RC circuit. If this drop is large, it can cross **noise margins** of the standard cell and impact correct funtioning of the chip.
 
 **Ground bounce:** Ground bound is an incident wherein voltage at the ground pin of an instance can jump above 0V. Ground bounces can cause similar throw a cell off its noise margins and impact correct functioning of the chip.
 
 Good powerplanning reduced the chances of voltage drop and ground bounce.
 
 ### What is placement?
 **Placement** is the process of assigning fixed locations to standard cells on the chip.  
 
 #### Factors determing good placement
 
 The main goal of placement is to keep connected cells close to each other and also close to connected I/O ports.  
 This helps reduce wire cap during routing, which makes the circuit faster as well as reduces its power consumption. Shorter connections also help preserve signal integrity better.
 
 **Abuttment** of connected cells is done to ensure good placement.
 **Repeater buffers** are inserted to preserve signal integrity where connection lengths are long.
 
## Day-2 : Floorplanning and Placement | Labs

### Floorplanning

Atomic command used to invoke floorplanning in openLANE:

```
run_floorplan
```

After floorplan run is complete, a floorplan DEF wil be generated in <run_tag>/results/floorplan directory, as shown below:
 <p align="center"> <img src="/Images/Day-2/Labs/fp_def.png"/></p>
 
 This DEF can be opened in Magic using following command:
 ```
 magic -T <path_to_tech_file> lef read <path_to_merged_lef> def read <path_to_floorplan_def>
 ```
 Standard cells are not placed inside floorplan DEF but standard cell rows have been defined. The final DEF look like this in magic:
 <p align="center"> <img src="/Images/Day-2/Labs/fp_def_magic.png"/></p>
 
 ### Placement
 
 Atomic command to invoke placement in openLANE:
 
 ```
 run_placement
 ```
 
 A placement DEF is created in <run_tag>/results/placement directory. Viewed in magic, it looks something like this:
 <p align="center"> <img src="/Images/Day-2/Labs/placement_def_magic.png"/></p>

 You can see that all standard cells have been placed at fixed locations inside this DEF.
  
## Day-3 : Inverter characterization , CMOS fabrication process and DRC/LVS | Theory
  
  ### Part -1 | Inverter characterization
  
  #### Slew and slew characteization
  
  **Rise slew:** Time taken by a signal to transition from LOW state to HIGH state is called rise slew.
  **Fall slew:** Time taken by a signal to transition from HIGH state to LOW state is called fall slew.
  
  **Calculation of slew**: We need two points on a rising/falling signal waveform to determine its slew. Slew can be calculated by taking the time difference between these two points. These points are defined as follows:
  
  - **slew_low_rise_threshold:** Bottom point (lying close to 0V) sampled for slew calculation on a rising waveform. Usually set to **20% of VDD**.
  - **slew_high_rise_threshold:** Top point (lying close to VDD) sampled for slew calculation on a rising waveform. Usually set to **80% of VDD**.
  - **slew_low_fall_threshold:** Bottom point (lying close to 0V) sampled for slew calculation on a falling waveform. Usually set to **20% of VDD**.
  - **slew_high_fall_threshold:** Top point (lying close to VDD) sampled for slew calculation on a falling waveform. Usually set to **80% of VDD**.

Given these definitions, rise slew of a signal is:

```math
rise\_slew = time_{slew\_high\_rise\_threshold} - time_{slew\_high\_rise\_threshold}
```
and fall slew is:
```math
fall\_slew = time_{slew\_low\_fall\_threshold} - time_{slew\_high\_fall\_threshold}
```
  #### Delay and delay characteization
  
  **Propagation delay:** Propagation delay of a cell is the time difference between output pin transition and input pin transition.
  
  To calculate propagation delay, we need to define following terms:
  
  - **in_rise_threshold:** Input rising waveform is sampled at this point for delay calculation. Usually set to **50% of VDD**.
  - **in_fall_threshold:** Input falling waveform is sampled at this point for delay calculation. Usually set to **50% of VDD**.
  - **out_rise_threshold:** Output rising waveform is sampled at this point for delay calculation. Usually set to **50% of VDD**.
  - **out_fall_threshold:** Output falling waveform is sampled at this point for delay calculation. Usually set to **50% of VDD**.

Given this, propagation delay of a cell can be calulated using following formula:
```math
time_{pd} = time_{out\_*\_threshold} - time_{in\_*\_threshold}
```

**Note:**  
- Rise delay of a cell implies delay when output is rising and fall delay implies delay when output is falling
- If delay thresholds are not properly chosen, one can see negative delay values. Care should be taken in determining thresholds.

#### Switching margin:
   
 Switching margin (`Vm`) is the output voltage of a circuit when it is equal to the input voltage.
 
 ### Part-2 | 16 mask CMOS farication process:
 
<br>
<p align="center">Final view of a CMOS fabricated using 16-mask process</p>
<p align="center"><img src="/Images/Day-3/Theory/cmos_fabricated_final.png"/></p>
 
CMOS fabrication is a highly detailed process involving several process and steps. These are described in brief below:
 
#### Steps:

1. Substrate preparation
2. Creation of active region for transistors:
   - Layer of SiO2 and Si3N4 is created over the wafer
   - Using **photolithography** and **etching**, Si3N4 is etched away from boundary of nMOS and pMOS regions.
   - SiO2 layer at boudaries is thickened using **LOCOS (LOCal Oxidation of Silicon)**
   - Remaining Si3N4 is removed using hot Phosphoric acid
3. N-well and P-well formation
   - Using **photolithography** and **ion-implantaion**, impurities are added to selective sites of the substrate in order to create n-well and p-well
   - Chip is heated at hgh temperatures for well diffusion to occur. This deepens the well.
4. Formation of Gate:
   - To ensure correct doping concenterations, diffused n-well/p-well is again doped using ion implantation. Correct doping concenteration is necessary to obtain correct threshold voltage
   - Layer of polysilicon is deposited over wafer. 
   - Using photolithography, poly is etched away from entire surface except for the sites of Gate formation.
5. Lightly Doped Drain formation:
   - Doped concenteration should gradually change between diffusion and Source/Drain region in order to prevent **hot electron effect** and **short channel effect**. Doping concenteration should be p+, p-, n for a pMOS and n+, n-, p for a nMOS.
   - Ion implantation is done to create lightly doped Source/Drain region (p- and n- doping for pMOS and nMOS respectively)
   - **Plasma anisotropic etching** is used to create side wall spacers of Si3N4
6. Source and drain formation:
   - Thin layer of SiO2 (called screen oxide) is added to prevent channeling during through lightly doped regions during ion implantation.
   - Ion implnatation is done to make heavily doped source and drain regions
   - **Annealing** is done is a high temperature furnace to push doping atoms deeper. 
7. Local Interconnect (locali) formation:
   - Screen oxide is removed by etching with HF.
   - Titanium is **sputtered** over the wafer, which is heated to 650-700 celsius in N2 ambient
   - TiSi2 is formed where Ti in in touch with Silicon. Layer of TiN2 is formed above it.
   - Using photolithography and **RCA based selective etching**, excess TiN2 is removed till only TiN2 local interconnects are left.
8. Higher level metal formation:
   - Non-planar surface is not suitable for metal deposition.
   - Wafer is covered with **phosphosilicate glass** and polished using **Chemical Mechanical Polishing (CMP)**.
   - Photolithography is done to remove phosphosilicate glass from places where holes need to be drilled for contact formation.
   - Tungsten is deposited over entire wafer and CMP is done till only Tungsten filled holes remain.
   - Folllowing that Al is deposited over entire surface and photolithography is done to remove extra Al.
   - Multiple layers of SiO2, TiN2, Tungsten and ALuminium are deposited and etched in a similar fashion to create higher metal layers.
   - Finally, chip is coated with Si3N4 for protection and holes are drilled to make final contacts of CMOS.

 
### Part-3 | CMOS inverter layout and DRC 

**Design Rules** are constraints specified in PDK tech file. Sticking to these constraints is mandatory to ensure correct fabrication.

**Why such constraints?**
CMOS fabrication is heavily dependent on UV photolithography. The wavelength of UV light used imposes a physical contraint on the extent of size and spacing between different parts of the layout. Photolithography cannot be done on an object whose size is smaller than tha wavelength of UV light used.

**Design Rule Check (DRC):** DRC is the process of testing final layout of the chip for any design rule violations. If found, these need to be corrected before tapeout.

## Day-3 | Labs:

### Part-1 | CMOS inverter SPICE extraction and characterization

<p align="center">sky130_inv Layout (as viewed in Magic)</p>
<p align="center"><img src="/Images/Day-3/Labs/cmos_inv.png"/></p>

- Use following commands in magic to extract SPICE netlist from CMOS inverter layout (.mag file):
```
extract all
ext2spice cthresh 0 rthresh 0
ext2spice
```
Extracted netlist looks like this:
<p align="center"><img src="/Images/Day-3/Labs/spice_extraction.png"/></p>
<br>

- Modify the SPICE netlist as shown in figure below, and run SPICE simulation using command:
```
ngspice <path_to_spice_file>
```
<p align="center"><img src="/Images/Day-3/Labs/spice_run.png"/></p>
<br>

- Use the following commands in ngspice to view simulation results:
```
setplot tran1
display
plot y vs time a vs time
```
Simulation output looks as follows:
<p align="center"><img src="/Images/Day-3/Labs/sim_output.png"/></p>

### Part-2 | DRC checks using Magic

- In Magic, regions exhibitng DRC violations are shown as covered in white dots.
- Selecting an area on the magic layout followed by querying 'drc why' on the console shows the reason behind all violations in the given region. An example in 'met3.mag' is shown below:

<p align="center"><img src="/Images/Day-3/Labs/drc_met3.png"/></p>

- One can use 'cif see' to see implanted layers on the GUI. Below diagram shows one example of this. Further, I have queried spacing between via and metal boundary by creating a box and querying its dimensions. This distance is larger than the design rule given in sky130A.tech.

<p align="center"><img src="/Images/Day-3/Labs/cif_see.png"/></p>

- **Fixing violations:** Given tech file is missing a design rule which governs spacing between polyres and poly layers. I added this design rule to the tech file and re-checked for DRC violations. Following violation was now seen in the console which was earlier absent:

<p align="center"><img src="/Images/Day-3/Labs/drc_poly_fix.png"/></p>
<br>

## Day-4: STA and CTS | Theory

### Part -1 | STA fundamentals:
 
 ### Introduction
 
All real cells have finite **propagation delays**. This can be found from the dotLIB file of a cell, which contains propagation delay value for several combinations of **input_slew** and **output_load**.
 
**Static Timing Analysis(STA)** is a method used in digital circuit design to ensure that the circuit meets its timing requirements. Timing requirements are critical in digital circuit design because if the signals do not arrive at the right time, errors can occur, and the circuit may not function as intended.

Timing requirements can mainly be required in two categories:
- **Setup requirement:** Setup requirement refers to a timing requirement that ensures that the input signal to a flip-flop is stable for a specified amount of time before the clock edge that triggers that flip-flop.
- **Hold requirement:** Hold requirement refers to a timing requirement that ensures that the output signal to a flip-flop remains stable for a specified amount of time after the clock edge that triggers that flip-flop.

#### Setup analysis:

- Consider a launch flop sending data through a combintaional circuit to the capture flop.
- In a highly simplified scenario with ideal clocks and ideal flops, it should be easy to see that delay of the combinational circuit must be less than the time period of the clock in order to meet setup requirement. Otherwise, capture flop will sample data before correct signal has reached the input pin. Therefore,

```math
\theta < T
```
<p align="center">where, T = Time period of the clock</p>
<p align="center">       $\theta$ = total delay in combinational circuit</p>

- If we relax the flops to be non-ideal, they will have non-zero setup time. Then output signal from launch flop must reach at least setup time before the second clock tick. Therefore,

```math
\theta < T - S
```
<p align="center">       S = Setup time of capture flop</p>

- Clock period can have some uncertainity. Accounting for that,

```math
\theta < T - S - SU
```

<p align="center">       SU = Setup uncertainity of clock</p>

- If clock network is non-ideal, clock can reach the launch and capture flops at different times. Hence, **final setup requirement** will become:

```math
(\theta + \delta_{launch}) < (T + \delta_{capture}) - S - SU
```
<p align="center">where, T = Time period of the clock</p>
<p align="center">       $\theta$ = total delay in combinational circuit</p>
<p align="center">       S = Setup time of capture flop</p>
<p align="center">       SU = Uncertainity of clock time period for setup analysis</p>
<p align="center">       $\delta_{launch}$ = Delay from clock port to clock pin of launch flop</p>
<p align="center">       $\delta_{capture}$ = Delay from clock port to clock pin of capture flop</p>

- **Slack**, defined as difference between data required time and data arrival time at capture flop must always be positive.

#### Hold analysis

- Consider a launch flop sending data through a combintaional circuit to the capture flop.
- In a simplified scenario with ideal clocks, the delay of combinational circuit must be greater than hold time for capure flop. If that is not the case, data will reach capture flop before its hold time is met, and metastability will occur.

```math
\theta > H
```
<p align="center">where, H = Hold time of capture flop</p>
<p align="center">       $\theta$ = total delay in combinational circuit</p>

- Introducing non-ideal clock network and a small amout of hold uncertainity, the **final hold requirement** will become:

```math
(\theta + \delta_{launch}) < (T + \delta_{capture}) + HU
```
<p align="center">where, H = Hold time of capture flop</p>
<p align="center">       $\theta$ = total delay in combinational circuit</p>
<p align="center">       HU = Hold uncertainity</p>
<p align="center">       $\delta_{launch}$ = Delay from clock port to clock pin of launch flop</p>
<p align="center">       $\delta_{capture}$ = Delay from clock port to clock pin of capture flop</p>

- **Slack**, defined as difference between data arrival time and data required time at capture flop must always be positive.

### Part -2 | Clock Tree Synthesis (CTS):

#### Introduction

**Clock Tree Synthesis (CTS)** is the process of designing clock distribution network from the clock port to sequential pins. The clock distribution network contains mainly of clock buffers and clock gates.

CTS becomes mandatory because a single clock port cannot drive all sequential pins inside a design. Further CTS should achieve following goals:
1. Clock should reach all sequential pins at the same time i.e., there should be **minimal skew**.
2. Variation in clock period, called **jitter**, should be minimal

These requirements are better met if clock tree has following **properties**:
1. Each layer should have same buffer cells
2. Each cell should drive the same load

#### CTS algorithm | H-tree algorithm

In order to minimize skew, clock nets are not laid in an unorderly fashion. Rather, it is ensured that the parent branch of the clock net intersects the child branch at its midpoint. 
When this is ensured for all branches:
1. Each sequential pin is equidistant from the clock port and all sequential pins see the same clock delay.
2. Resultant routing structure looks like the letter "H", hence giving the algorithm its name

**Repeaters** are added to long branches to ensure signal integrity. It is attempted that equal number of repeaters lie between the clock port and each sequential clock pin. Clock repeaters have the special proprty that their rise and fall times are equal.

#### Crosstalk and clock net shielding

**Crosstalk** is the phenomena wherein state changes on an aggressor net can induce glitches or delays in a nearby victim net. This happens because capacitances of nearby nets are coupled with each other.

**Clock Tree Shielding:** 
Clock nets are extremely sensitive to glitches. A single glitch on clock net can corrupt functioning of the circuit.
In order to avoid glitches on clock nets due to crosstalk, they are surrounded by power or ground nets which do not toggle. This process of protecting clock tree from crosstalk is called **Clock net shielding**.

## Day-4 | Labs

### Part-1 | Converting layout to LEF

- DRC checks on inverter. Grid height, width and I/O pin location should satisfy certain criterion as shown below:

<p align="center"><img src="/Images/Day-4/Labs/inv_drc.png"/></p>

- Port creation on layout:

<p align="center"><img src="/Images/Day-4/Labs/port_creation.png"/></p>

- Write lef:

<p align="center"><img src="/Images/Day-4/Labs/write_lef.png"/></p>

### Part-2 | Run synthesis and verify WNS and TNS using openSTA

- Correct setup. CLOCK_PERIOD variable had different values in different files as shown below. Made them all equal to 12.00 ns so that errors/inconsistencies are avoided later.

<p align="center"><img src="/Images/Day-4/Labs/clk_correction.png"/></p>

- Run synthesis and verify our custom cell 'sky130_vsdinv' has been used in the netlist.

<p align="center"><img src="/Images/Day-4/Labs/syn_new.png"/></p>

### Part-3 | Reduce slack by changing run parameters

- Change ::env(SYNTH_STRATEGY) and ::env(SYNTH_SIZING) and observed impact on slack. For me, TNS reduced from -3242.97 ps to -407.96ps; WNS reduced from -26.67ps to -5.59ps

<p align="center"><img src="/Images/Day-4/Labs/reduce_slack1.png"/></p>

- Do STA analysis using OpenSTA. Verify that WNS(Worst Negative Slack) and TNS (Total Negative Slack) match those reported after 'run_synthesis'.

<p align="center"><img src="/Images/Day-4/Labs/open_sta1.png"/></p>

- Change ::env(SYNTH_MAX_FANOUT) and observe reduction in slack:

<p align="center"><img src="/Images/Day-4/Labs/reduce_slack2.png"/></p>

### Part-4 | Reduce slack using ECO

- Run openSTA. Replace cells showing large delay with their upsized verisons. Observe reduction in slack:

<p align="center"><img src="/Images/Day-4/Labs/reduce_slack3_eco.png"/></p>

- Reduce slack further by repeating ECO.

<p align="center"><img src="/Images/Day-4/Labs/reduce_slack4_eco.png"/></p>

### Part-5 | Check your custom cell in DEF

- Run floorplan and placement. Check if your custom cell is present in DEF:

<p align="center"><img src="/Images/Day-4/Labs/def_w_sky130_inv.png"/></p>

### Part-6 | Run CTS

- Run CTS. Use atomic command given below:
```
run_cts
```
<p align="center"><img src="/Images/Day-4/Labs/cts_run.png"/></p>

- Perform STA analysis on typical corner using openROAD. Use cts.netlist.

<p align="center"><img src="/Images/Day-4/Labs/cts_nl_sta.png"/></p>
<br>

## Day-5: Routing | Theory


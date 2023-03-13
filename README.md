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
 
 ### Important terminology related to floorplanning
 
 **Utilisation factor:** 
 Utilisation factor is defined as the ratio of area of netlist to the area of core of the die.
 It determines the total core area based on the area occupied by netlist and is an important factor for generating a good floorplan. Usually, it's value is kept close to 50%. This leaves substantial space in the core for CTS and ECO changes.  
 
 **Aspect ratio:**
 It is the ratio of height of the core to the width of the core.
 Aspect ratio determines the shape of a chip. Aspect ratio of 1 implies a square-shaped chip.
 
 **Pre-placed cells:**
 Pre-placed cells (PPCs) are cells that are placed in the core before the automated placement and routing process. 
 These cells usually consists of **foundry IPs** and their location is fixed during floorplanning. It is not altered by placement tool.
 
 **Decoupling capacitors:**
 Decoupling capacitors help reduce noise and improve power supply performance. They work by temporarily storing electrical charge and then releasing it back into the circuit when needed.
 Preplaced cells are surrounded by decoupling capacitors during floorplanning.
 
 ### What is powerplanning?
 **Power planning** is done to ensure that each block of the chip has access to a stable and reliable power supply. Power and ground pin locations are determined.
 
 ### Importnant terinology related to powerplanning
 
 **Voltage drop:**
 Voltage drop is an incident wherein power supplied at an instance's power pin can fall below supply voltage because to intervening RC circuit. 
 If this drop is large, it can cross **noise margins** of the standard cell and impact correct funtioning of the chip.
 
 **Ground bounce:**
 Ground bound is an incident wherein voltage at the ground pin of an instance can jump above 0V. 
 Ground bounces can cause similar throw a cell off its noise margins and impact correct functioning of the chip.
 
 Good powerplanning reduced the chances of voltage drop and ground bounce.
 
 ### What is placement?
 **Placement** is the process of assigning fixed locations to standard cells on the chip.  
 
 ### Factors determing good placement
 
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
time_{pd} = time_{out\_\*\_threshold} - time_{in\_\*\_threshold}
```

**Note:**  
- Rise delay of a cell implies delay when output is rising and fall delay implies delay when output is falling
- If delay thresholds are not properly chosen, one can see negative delay values. Care should be taken in determining thresholds.

   #### Switching margin:
   
   Switching margin (`V_m`) is the output voltage of a circuit when it is equal to the input voltage.



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
 <br>
 This will invoke openLANE as shown below:  
 
 <p align="center"> <img src="/Images/Day-1/Labs/Invoking_openLANE.png"/></p>
 
 ### Runnning synthesis
 
 Synthesis can be run using following atomic command:
  ```
  run_synthesis
  ```
  <br>
  run_synthesis generates teh netlist is <run_tag>/results/synthesis directory as shown below:
  <p align="center"> <img src="/Images/Day-1/Labs/synthesis.png"/></p>
  <br>
  
  ### Analyzing synthesis results
  
  We can examine the cell stats of synthesized run from reports present in <run_tag>/reports/synthesis/ directory.
  Here, we have calculated flop ratio for the given design:
  
  <p align="center"> <img src="/Images/Day-1/Labs/synthesis.png"/></p>
  
 
 
 









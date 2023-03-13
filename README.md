# Physical Design Workshop using OpenLANE
Complete RTL to GDSII flow using OpenLANE: A repository automating physical design flow using open source EDA tools for e.g., Yosys, Magic, OpenROAD

## Day-1: Inception

### Day-1 | Theory

#### Parts of a chip

- **Die:** The part of a chip that contains its circuitry implemented on Silicon.
- **Package:** Covering of the chip which protects it from physical damage and corrosion. It also allows mounting of electrical contacts connecting the chip to the Printed Circuit Board (PCB).
- **Core:** Functional part of the chip implemented by user to perform core operations of the chip.
- **IP:** IP stands for 'Intellectual Property'. It consists of the peripherals inside the chip that are not designed by user but sourced directly from other vendors.

#### Instruction Set Architechture (ISA) | RISC-V ISA

Instruction Set Architecture (ISA) is the language in which user communicates with the microprocessor. It consists of all the 'instructions' a user can ask the processor to perform. These instructions are described using English-like commands. However, they are implemented in hardware using binary language.

Reduced Instruction Set Computer (RISC) uses a reduced ISA i.e., it efficiently implements only the instructions that are frequently used in programs, while the less common operations are implemented as subroutines.
**Eg of RISC-V instruction:** ADD rd, rs1, rs2 : Adds the contents of registers rs1 and rs2, and stores the result in register rd

#### How computers work?

Programs written in High Level Languages are converted into Machine Language using softwares called **compilers**.
If the program carries out some specific application while interacting with user, it is called **application software**. Such application software requests the **operating system**, the guard and controller of computer hardware, permission to interact with peripheral devices and access to memory. Once the application software is loaded into memory, in binary language, it can perform the required task.

<p align="center">
<img src="https://www.uow.edu.au/assets/contributed/deputy-vice-chancellor---academic/learning-co-op/technology-and-software/uow230703.png" />
</p>

#### Phyical Design Workflow | RTL2GDSII implmentation

Physical design in VLSI (Very Large Scale Integration) refers to the process of transforming a logical circuit representation into a physical layout that can be fabricated on a chip.

It consists of following parts:

1. **Synthesis:** Synthesis is the process of implementing the functionality of a RTL using logic gates. It takes in RTL and outputs Gate netlist.
2. **Floorplanning:** Floorplanning involves determining the size and shape of the chip and dividing it into sections. We place macros in appropriate locations and also determine locations where standard cells will be placed in future.
3. **Placement:** Placement involves determining the location of each standard cell on the chip. Placement algorithm tries to minimize total wire length of the chip.
4. **Clock tree synthesis:** This step involves the synthesis of the clock distribution network in the chip. Goal is to have the clock arrive at all sequential clock pins at the same time.
5. **PDN generation:** PDN stands for Power Distribution Network. It refers to the network of power and ground wires that deliver power to the various components of the circuit.
6. **Routing:** Routing involves determining the paths that connect the components using wires. Routing algorithm tries to achieve routing with minimum wire length and minimum wire bends.

#### Introduction to OpenLANE 

OpenLANE is an open-source automated RTL-to-GDSII (register transfer level to graphic design system II) flow tool for digital ASIC (application-specific integrated circuit) design. It is built on a collection of open-source tools, such as Yosys for synthesis, Magic for physical layout, and OpenROAD for optimization, and is designed to provide a complete, end-to-end solution for ASIC design.

<p align="center">
![](/Images/Day-1/Theory/openLANE_flow.png)
</p>








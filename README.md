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

![](https://www.uow.edu.au/assets/contributed/deputy-vice-chancellor---academic/learning-co-op/technology-and-software/uow230703.png) 


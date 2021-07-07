# Understanding the Morello Memory Map.

 [Go back to Morello Getting Started Guide.](./../../../../morello-getting-started.md)

Documentation outlining Morello's memory maps can be found here: [Arm Morello System Development Platform Preliminary Technical Reference Manual.](./https://developer.arm.com/documentation/102278/latest)

Whilst this provides a general overview, it does not give clear details of the location of some specific devices and their memory mapped registers.

The purpose of this guide is to fill in missing detailed information and is a work-in-progress document that will be added to as necessary for the purposes of this project.


## Memory Types
There are two different types of memory defined: **normal memory** and **device memory**. Normal memory is assigned to areas such as program memory, for example where DRAM is located in the memory map. Device memory is assigned to areas where peripheral devices and their registers are located such as the Interrupt controller and its memory mapped registers. It is important to understand the difference when setting up the MMU translation tables.

## DRAM Program Memory
Bare metal programs are loaded into DRAM0 memory by default, starting at the base address of 0x80000000. The image entry point, where the actual program starts, is usually offset to 0x80011000. This can be seen by observing the **ELF** file (filename.axf). Additionally, the memory management unit (MMU) is initialised at EL3 for DRAM0 by default. The DRAM0 memory range is as follows:

0x80000000 - 0xFFFFFFFF - 2GBytes

It is possible to change the base address at which the program is loaded into memory through a linker command line option. 

```
--image-base=0x80000000
```
In Development Studio select the project you wish to change and right-click. Select **Properties -> C/C++ Build -> Settings -> LLVM C Linker 11.0.0 -> Miscellaneous** and then add the command line to the **-Xlinker** options. After the project is re-built the ELF file should now show the new image entry point address (with the offset).

## General Interupt Controller (GIC-600) Memory Mapped Registers
To use the Interrupt controller there are both system registers and memory mapped registers that need to be accessed. To use the memory mapped registers the Interrupt Controller System Register Enable register for each exception level needs to be configured:

ICC_SRE_EL3, ICC_SRE_EL2, ICC_SRE_EL1,

The memory mapped registers are configured as shown in the diagram. There are two regions: the first is located at the GICD base address and uses 64kB of memory space, the second is located at the RD base address and uses 128kB split into two regions of 64kB of contiguous memory space. The Morello Platform uses the GIC-600 version where extra power management of the redistributor is required.

![GIC Morello Memory Map](./GICMemMap.gif)

The technical manual can be located here:

https://developer.arm.com/documentation/100336/0106/introduction/about-the-gic-600

**GICD Base Address** - this is the base adress for the **Distributor** registers on the Morello Platform and are identified with the prefix GICD.
Table 4.2 (section 4.2) in the manual specifies the address offset of each register from the base address.

**RD Base Address** - this is the base address of the **Redistributor** registers on the Morello Platform, and registers in both memory regions are identified with the prefix GICR. The first memory region (GICR) registers are specified in table 4.21 (section 4.4) of the manual. The second contiguous memory block (SGI) has a fixed offset from RD of 0x10000 and can be labelled as the SGI base. These registers are specified in Table 4.29 (section 4.5) of the manual.

## Access to the PL011 UART
The Morello Platform has access to the standard ARMv8-A PL011 UART. Although not specified in the Morello Documentation, this can be accessed through the base address at 0x1C090000. This is located at the top of the **Expansion AXI** region (0x08000000 - 0x1FFFFFFF). The Technical Reference Manual for the uart can be found here: https://developer.arm.com/documentation/ddi0183/latest/

## More....
More Content to be added.
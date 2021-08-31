# Compiling for Morello using Development Studio

 [Go back to Morello Getting Started Guide.](./../../../../morello-getting-started.md)


## Overview

This document lists the settings needed when compiling for *Morello*, *Morello-purecap*, or *armv8.2*


## Change the compile toolchain settings

In the Project Explorer view, select a **project** and **right click**, then select **Properties**. Select **C/C++ Build -> Settings** tab and set the LLVM toolchain as necessary below.


## Configure the toolchain for normal Morello (without capabilities)

* LLVM C Compiler options:
    * In Target, enter `aarch64-none-elf`
    * In Architecture, `morello`

* LLVM Assembler options:
    * In Target, enter `aarch64-none-elf`

* LLVM C Linker options:
    * In Target, enter `aarch64-none-elf`
    * In Architecture, enter `morello`

Click **Apply and Close**. If you are prompted to rebuild the index, click **Yes**.

## Configure the toolchain for Morello as armv8.2 (without capabilities and without any use of capability registers)

Note that this option may be used to eliminate the use of capability registers. See https://community.arm.com/developer/research/morello/f/forum/51278/are-capability-registers-used-when-compiling-for-normal-morello-without-pure-capability-with-llvm 

* LLVM C Compiler options:
    * In Target, enter `aarch64-none-elf`
    * In Architecture, enter `morello+noa64c`

* LLVM Assembler options:
    * In Target, enter `aarch64-none-elf`

* LLVM C Linker options:
    * In Target, enter `aarch64-none-elf`
    * In Architecture, enter `morello+noa64c`

Click **Apply and Close**. If you are prompted to rebuild the index, click **Yes**.


## Configure the toolchain for Morello pure capability

* LLVM C Compiler options:
    * In Target, enter `aarch64-none-elf`
    * In Architecture, enter `morello+c64`
    * In ABI(-mabi), enter `purecap`

* LLVM Assembler options:
    * In Target, enter `aarch64-none-elf`
    * In Architecture, enter `morello+c64`
    * In ABI(-mabi), enter `purecap`
    
* LLVM C Linker options:
    * In Target, enter `aarch64-none-elf`
    * In Architecture, enter `morello+c64`
    * In ABI(-mabi), enter `purecap`

Click **Apply and Close**. If you are prompted to rebuild the index, click **Yes**.

Note: If assembly code is present, the LLVM Assembly options also need to be set to purecap.

Note: c64 is a variant of the A64 ISA (instruction set architecture) that has been extended to be able to manipulate and use capabilities.


## Assembly files for low level code

Where examples include **assembly code** that can be compiled for both *Morello* and *Morello-purecap*, the files need to be prepocessed by the **LLVM compiler**. This is because they include `#ifdef __CHERI_PURE_CAPABILITY__`  statements to include code depending upon which mode it is being compiled for. 

* For automatic preprocessing by the **LLVM compiler** the assembly file **must have** an **uppercase** `S` extension.

* If the assembly file has a **lowercase** `s` extension, preprocessing does not take place correctly, and may not necessarily show up as an error. From testing, the **LLVM compiler** will compile the `#ifdef __CHERI_PURE_CAPABILITY__` code by default, regardless of the compile options specified.
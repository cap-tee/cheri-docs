# developmentStudio/trustedFW_EL2 - Trusted Firmware Tests

 [Go back to Morello Getting Started Guide.](./../../../../morello-getting-started.md)

 ## Overview

This example uses a [small loop assembly code](./../EL2N_s_loop/EL2N_s_loop.md) that is built into an image to replace the uefi boot code at 0x14200000. At this point in the boot flow, the trusted firmware has been loaded and the exception level is at EL2 - normal world. **Development Studio** is then used to load a program to run at EL2N (See [BootFlowOverview](./../../BootFlowOverview/BootFlowOverview.md)). The program performs some basic interface tests to the trusted firmware. 

Part of this code has been extracted from the Trusted Firmware-A tests repository

 https://git.trustedfirmware.org/TF-A/tf-a-tests.git/ d6715f10.....

 under the following license (https://spdx.org/licenses/BSD-3-Clause.html):

 Copyright (c) 2018, Arm Limited. All rights reserved.
 
 SPDX-License-Identifier: BSD-3-Clause

 ## Trusted Firmware MMU setup at EL3

The MMU map shows the mapping at EL3 when the trusted firmware is loaded through the boot flow process (The EL3 MMU map was captured by performing an SMC command at EL2 and stepping through the code to give access to EL3 in the debugger). By inspecting the disassembly view, it can be seen that the trusted firmware resides in the BOOT section of the memory map, which is marked as SP - secure physical. The NP - non secure physical address is also mapped for the DRAM0 memory. Since the trusted firmware code resides in the BOOT section, the whole of the DRAM0 is available to run normal world code.

 ![mem map](./trustedfirmwareMEMMAP.gif)

 ## Program Structure

 The program structure is given below. The SMC calling convention is used to interact with the trusted firmware.

  ![program](./trustedfirmware.gif)

 ## Image entry point for EL2N

This example uses the bare metal llvm compiler with a basic **morello initialisation code for EL2**, so that the **image entry point** is `_startel2`.



 ## Example code set up

 **MMU set up:**
The MMU is set up as follows:
* EL2NMMU
    *  0x00000000 - 1GB device memory
    *  0x40000000 - 1GB device memory
    *  0x80000000 - 1GB program memory
    *  0xC0000000 - 1GB program memory

 **UART:** This example uses  the pl011 uart at a base address of 0x1C090000.

 ## Files

* EL2N files:
    * setupel2.S - contains the _startel2 image entry point
    * trustedFW_EL2.c - main c code main() at EL2N
    * el2mmusetup_allDRAM.s - MMU setup for EL2N
    * trustedFW_EL2asmfuncs.s - interface to trusted firmware assemblyfunctions
    * trustedFW_EL2funcs.c - interface to trusted firmware c functions
    * trustedFW_EL2funcs.h - interface to trusted firmware c functions header
    * uart_fputcRedirect.c - uart c functions
    * uart_fputcRedirect.h - uart c functions header
    * linker-script-el2.ld - linker script for el2

## SMC - Secure Monitor Calls

The test program uses secure monitor calls (SMC) calls to interface to the Trusted Firmware-A. For more information on SMC calls see [EL1ToEL3SMC](./../../../BareMetalOnMorello/BareMetalExamples/EL1ToEL3SMC/EL1ToEL3SMC.md), or https://developer.arm.com/architectures/system-architectures/software-standards/smccc. 

Only a very small number of tests have been included within this example to minimise the complexity and demonstrate the basic principle. The SMC calling convention is documented here https://developer.arm.com/documentation/den0028/b/ from which the following Arm Architecture calls are documented and used within the test code:

* SMCCC_VERSION - Retrieve the implemented version of the SMC Calling Convention
* SMCCC_ARCH_FEATURES - Determine the availability and capability of Arm Architecture Service functions
* SMCCC_ARCH_SOC_ID - service function discovery of - "Obtain a SiP defined SoC identification value"
* SMCCC_ARCH_WORKAROUND_1 - service function discovery of "Execute the mitigation for CVE-2017-5715 on the calling PE"
* SMCCC_ARCH_WORKAROUND_2 - service function discovery of "Enable or disable the mitigation for CVE-2018-3639 on the calling PE"


## Build the Project
Build the project within Development Studio. First check the linker is pointing to the linker script correctly. Select the project, and right click, then **Properties -> C/C++ Build -> settings -> LLVM C Linker 11.0.0 -> Miscellaneous**.

```
-T/<directory name>/morello-baremetal-examples/developmentStudio/<project name>/src/linker-script.ld -v
```
Then build the project. **Project -> Build Project**

## Connect to the FVP Model
Launch the FVP model by running the automated script to build and include the [small loop assembly code](./../EL2N_s_loop/EL2N_s_loop.md) for EL2N in place of the uefi boot code at 0x14200000. The program will sit in a loop at EL2N waiting for the debugger to load the trusted firmware test program. To ensure that the boot flow and small loop code runs prior to downloading the test program from Development Studio, include the `--run` option in the fvp command line of the script as detailed in the [small loop assembly code](./../EL2N_s_loop/EL2N_s_loop.md). Navigate to `commandLine/bootflow/EL2N_s_loop` directory and run the script:


```
./make_el2.sh
```


 Double click `<Project>Debug.Launch` and then select `Debug`. The Debugger should connect to the target. 

## Run the Code
In the **Debug Control** window, Either run or step through the code. If it is working correctly you should see the test results appear on a uart terminal window.


To stop the software and FVP, firstly disconnect the target from within Development Studio, and then type `CTRL+C` in the console from which the FVP was launched.


# Important information for writing programs for Morello - things that might catch you out

 [Go back to Morello Getting Started Guide.](./../../../../morello-getting-started.md)

This Document provides details of important things to consider when writing bare metal programs for Morello. This document is a work in progress and will be added to as more important things are discovered. 

## Enabling Morello Instructions

You need to unset a bit in an EL2 register in order to be able to use the Morello instructions beyond EL3 without being trapped. Even when you are not compiling for capability mode, under some circumstances, such as the printf funtion, the capability registers will still be used and if you do not unset this bit your program may terminate due to an exception. The **enable morello instructions** bit is turned off by default (set to 1) in the `_start` code. Note that this is a reserved bit in the armv8a base version so it is not obvious that this bit needs checking.  It is important to note that when this bit is set it traps execution at EL2, EL1, or EL0 of Morello instructions and system registers to EL2.
* CPTR_EL2 - TC, bit [9] when set to 1 traps morello instructions like using capability registers which the printf function uses.

See for more details: https://community.arm.com/developer/research/morello/f/forum/51278/are-capability-registers-used-when-compiling-for-normal-morello-without-pure-capability-with-llvm
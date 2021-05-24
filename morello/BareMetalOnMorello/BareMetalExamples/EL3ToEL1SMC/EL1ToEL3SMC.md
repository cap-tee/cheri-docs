# EL1ToEL3SMC

 [Go back to Morello Getting Started Guide.](./../../../../morello-getting-started.md)

## Overview of EL1ToEL3SMC

After the boot sequence to EL1N, an SMC is used to call into EL3.

## Program Structure

![Program structure](./EL3MMUEL1MMUUART.gif)


## Files

## Calling into the Secure world from the non secure world

After the boot up sequence to EL1, for example after performing an ERET from EL3 to EL1, all subsequent transitions between the non secure world and the secure world need to pass through EL3 (the Monitor) using secure monitor calls (SMC). 

## SMC - Secure Monitor Calls

An SMC is a type of synchronous exception. Synchronous exceptions share a common entry in the vector table.  Within the handler you have to read the Exception Syndrome Register (ESR_ELx) to get the type of synchronous exception (EC field), and any additional information the processor can give you (ISS field).

For example, for a SMC exception ESR_ELx.EC would read b010011 (if from AArch32) or b010111 (if from AArch64).
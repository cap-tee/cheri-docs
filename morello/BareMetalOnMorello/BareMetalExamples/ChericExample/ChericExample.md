# ChericExample

 [Go back to Morello Getting Started Guide.](./../../../../morello-getting-started.md)

## Overview of ChericExample

This example explores some of the CHERI functionality in c code using the cheri.h API functions and the cheriintrin.h built-in functions. The example sets up capability pointers, and retreives and prints their properties. This example can be compiled for either *Morello* or *Morello-purecap*, but the capability operations can only be conducted when compiled for *Morello-purecap*.

The cheri c/c++ programming guide can be found here: https://github.com/CTSRD-CHERI/cheri-c-programming/releases/tag/UCAM-CL-TR-947 

## Program structure

The program generates a number of VALID and NON-VALID capabilities and uses the cheri functions to inspect and manipulate them. Operations include:
* Creating capabilities
* Extracting and printing capability parameters such as length, base, permissions, tag and offset.
* Retreiving the capability program counter (PCC) and Default Data Capability (DDC) using cheri functions.
* Changing the bounds and offset of a capability using the cheri functions.


## Files

* EL3 files:    
    * ChericExample.c - main c code main() at EL3 which either calls code to use the cheri.h or cheriintrin.h functions.
    * using_cherih.c - uses the cheri.h API functions
    * using_cheriintrinh.c - uses the cheriintrin.h built-in functions


## Compiling with *Morello-purecap*

Code written for *Morello-purecap*, uses `#ifdef __CHERI_PURE_CAPABILITY__` so the compiler knows which code to compile.


## Build the Project

Build the project. **Project -> Build Project**

## Connect to the FVP Model
Ensure that you have already launched the FVP model. Double click `<Project>Debug.Launch` and then select `Debug`. The Debugger should connect to the target. 

## Run the Code
In the **Debug Control** window, run the code to observe the paramters such as the base address, bounds and permissions for each capability generated.


To stop the software and FVP, firstly disconnect the target from within Development Studio, and then type `CTRL+C` in the console from which the FVP was launched.
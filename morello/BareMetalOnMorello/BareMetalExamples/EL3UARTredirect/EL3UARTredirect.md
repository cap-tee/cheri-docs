# EL3UARTredirect - Example showing how to redirect an embedded printf function to the uart 

 [Go back to Morello Getting Started Guide.](./../../../../morello-getting-started.md)

## Overview of EL3UARTredirect

Example showing how to redirect an embedded printf function to the uart.

This example code uses an embedded printf function from https://github.com/mpaland/printf d3b98468..... under an **MIT license** to enable the _putchar() function to be re-directed to the uart (It was not clear how to re-direct the default printf function that comes with LLVM since it did not call a putc/fputc function).

## Files

The files consist of the following

* EL3UARTredirect.c - main c code main() at EL3
* el3mmusetup.S - MMU setup for EL3
* uart_redirect.c - uart functions
* uart_redirect.h - uart header file
* printf.c - printf functions
* printf.h - printf functions header file

Capability files
* capboot.S - code to set up UART capability that would go in bootcode
* capfuncs.c - function to print capability properties
* capfuncs.h - function to print capability properties header file

## MMU set up

The MMU is set up as follows:
* EL3MMU
    *  0x00000000 - 1GB device memory - UART base address 0x1C090000
    *  0x40000000 - 1GB device memory
    *  0x80000000 - 1GB program memory
    *  0xC0000000 - 1GB program memory


## PL011 UART

This example uses the pl011 uart at a base address of 0x1C090000.

## Build the Project

Build the project. **Project -> Build Project**

## Connect to the FVP Model
Ensure that you have already launched the FVP model. Double click `<Project>Debug.Launch` and then select `Debug`. The Debugger should connect to the target. 

## Run the Code
In the **Debug Control** window, Either run or step through the code. A message will appear in the console from EL3, and in the UART.

To stop the software and FVP, firstly disconnect the target from within Development Studio, and then type `CTRL+C` in the console from which the FVP was launched.
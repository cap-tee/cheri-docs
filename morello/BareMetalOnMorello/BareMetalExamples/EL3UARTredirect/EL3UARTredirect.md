# EL3UARTredirect - Example showing how to redirect an embedded printf function to the uart 

 [Go back to Morello Getting Started Guide.](./../../../../morello-getting-started.md)

## Overview of EL3UARTredirect

Example showing how to redirect an embedded printf function to the uart.

This example code uses an embedded printf function from https://github.com/mpaland/printf d3b98468..... under an **MIT license** to enable the _putchar() function to be re-directed to the uart (It was not clear how to re-direct the default printf function that comes with LLVM since it did not call a putc/fputc function).

## Files

The files consist of the following

* EL3UARTredirect.c - main c code main() at EL3
* el3mmusetup.s - MMU setup for EL3
* uart_redirect.c - uart functions
* uart_redirect.h - uart header file
* printf.c - printf functions
* printf.h - printf functions header file

## MMU set up

The MMU is set up as follows:
* EL3MMU
    *  0x00000000 - 1GB device memory - UART base address 0x1C090000
    *  0x40000000 - 1GB device memory
    *  0x80000000 - 1GB program memory
    *  0xC0000000 - 1GB program memory


## PL011 UART

Documentation and register offsets for the pl011 uart are detailed here:

Technical Reference Manual: https://developer.arm.com/documentation/ddi0183/latest/ 

An ARM example using the pl011 uart (in a different way to used here - the example retargets c functions) can be found here:

https://developer.arm.com/documentation/102440/0100/Retarget-functions-to-use-UART

## Build the Project

Build the project. **Project -> Build Project**

## Connect to the FVP Model
Ensure that you have already launched the FVP model. Double click `<Project>Debug.Launch` and then select `Debug`. The Debugger should connect to the target. 

## Run the Code
In the **Debug Control** window, Either run or step through the code. A message will appear in the console from EL3, and in the UART.

To stop the software and FVP, firstly disconnect the target from within Development Studio, and then type `CTRL+C` in the console from which the FVP was launched.
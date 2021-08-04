# commandLine/bootflow/trustedFW_EL2 - Trusted Firmware Tests

 [Go back to Morello Getting Started Guide.](./../../../../morello-getting-started.md)

 ## Overview

This example builds an image of the [trusted firmware test program](./TrustedFirmwareDS.md) to replace the uefi boot code at 0x14200000. At this point in the boot flow, the trusted firmware has been loaded and the exception level is at EL2 - normal world. **Development Studio** is not required to run this example. 

## Prerequisites

* A **new linker file** for the image script to work for EL2 has been created as detailed in the [small loop assembly code for EL2](./../EL2N_s_loop/EL2N_s_loop.md) example.
* A **new image generation script** to work with EL2 has been created as detailed in the [small loop assembly code for EL2](./../EL2N_s_loop/EL2N_s_loop.md) example.



## Running the Program

1. Navigate to `commandLine/bootflow/trustedFW_EL2` directory and open 'make_trustedFW_el2.sh` in an editor.
2. Check that the **clang, lld, and android build** paths are correct for your system.
3. Run the script. The script will build the project, load the image into the fvp, and run the model. If it is working correctly you should see the test results appear on a uart terminal window.

```
./make_trustedFW_el2.sh
```

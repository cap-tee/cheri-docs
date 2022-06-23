

# How to modify the default initialisation files for an EL2 project
 [Go back to Morello Getting Started Guide.](./../../../morello-getting-started.md)

This document describes the process to modify the initialisation files that run before the `main` function to be used by an **EL2 bare metal program**. The initiialisation files set up the system for programs starting at EL3, but could also provide a suitable baseline for setting up an EL2 program, such as a TF-A payload (BL33) with some small modifications.  See [Understanding the Default Initialisation Sequence for Morello](./../DefaultSetup/InitSequence/InitSequence.md) for more information regarding what happens before `main`.

For running **pure cap** programs at EL2, this kind of initialisation set up is required.

## Obtain a copy of the initialisation files

First, download the initialisation files (see [How to modify the default initialisation files for your project at EL3](./Modifycrt0.md)), and create a new standard Hello World project in Development Studio. Exclude the default initialisation from the build using `-nostartfiles` and create a file with stubs for `_init` and `_fini`.

## Build the project

Build your project as normal. You may wish to verify that the project still works on the FVP at EL3 as normal before going further.

## Modify the initialisation crt0.S file for EL2

The example used here is based on release-1.3. A different release may have different line numbers and code. Documented is the minimal changes needed to enable a "hello world" program to run in purecap mode at EL2.

* **line 118 - CPTR_EL3** - Comment out 3 lines. When building the BL33 binary into the `fip.bin`, use `ENABLE_MORELLO_CAP=1` to ensure the EC bit is set in EL3 (EC=1) (by the Trusted Firmware) so that the Morello instructions are not trapped at any exception level. See section **"A note on the Package options"** from [FVP/Hardware: Create a TF-A baremetal payload running at EL3](./../TFAbaremetalPayload/TFApayload.md)
    ```
    //mrs    x0, CPTR_EL3
    //orr    x0, x0, #(1 << 9) /* set EC */
    //msr    CPTR_EL3, x0
    ```
* **line 121 - CPTR_EL2** - Setting TC traps Morello instructions at EL2, so these three lines need to be modified to ensure this bit is cleared (TC=0), especially if `purecap` programs are to be built. One way is to change this:
    ```
    mrs    x0, CPTR_EL2
    orr    x0, x0, #(1 << 9) /* set TC */
    msr    CPTR_EL2, x0
    ```
    to something like this:
    ```
    mrs    x0, CPTR_EL2
    mov    x1, #0xFDFF
    movk   x1, #0xFFFF, LSL #16
    and    x0, x0, x1 /* clear TC */
    msr    CPTR_EL2, x0
    ```
* **line 132 - SCTLR_EL3** - Comment out SCTLR_EL3
    ```
    //mrs    x0, SCTLR_EL3
    //bic    x0, x0, #(1 << 20) /* clear CD0 */
    //bic    x0, x0, #(1 << 22) /* clear CD */
    //msr    SCTLR_EL3, x0
    ```
* **line 139 - CCTLR_EL3** - Modify for EL2. 
Change the adrdp base from EL3 to EL2 as follows:
From:
    ```
    msr    CCTLR_EL3, x0
    ```
    to:
    ```
    msr    CCTLR_EL2, x0
    ```
* **line 142 - CPTR_EL3** - Comment out CPTR_EL3

    ```
    //mrs    x0, CPTR_EL3
    //bic    x0, x0, #(1 << 10) /* clear TFP */
    //msr    CPTR_EL3, x0
    ```

## Other modifications to crt0.S to consider

* **line 268 - DDC Null** - Here the DDC is nulled, if you wish to add boot code you may need to change when the DDC is nulled.
    ```
    //WRITESYS     DDC, czr
    ```
* **line 279 - clear execute bit** - You may need to keep the execute bit, depending on what other changes you make to your boot code and your program requirements.
    ```
    //clrperm c1, c1, x2
    ```
* **line 499 - branch somewhere else instead of main**. By default after the setup, there will be a branch to `main`. If your starting function is called something else, or you have additional bootcode before `main` you can change where to branch to.
    ```
    bl	FUNCTION (main)
    ```

## Modify the initialisation rdimon-aem-el3.S file for EL2

* **line 60 - vbar_el3** - Comment out setting the EL3 vector address.
    ```
    //msr	vbar_el3, x0
    ```
* **line 78 - cvbar_el3** - Comment out setting the EL3 capability vector address.
    ```
    //WRITESYS	cvbar_el3, c0
    ```
* **line 162 - ttbr0_el3** - Modify to EL2.
    From:

    ```
    msr	ttbr0_el3, x0
    ```
    to:
    ```
    msr	ttbr0_el2, x0
    ```
* **line 201 - tcr_el3** - Modify to EL2.
From:

    ```
    msr	tcr_el3, x0
    ```
    to:
    ```
    msr	tcr_el2, x0
    ```
* **line 204 - mair_el3** - Modify to EL2.
From:

    ```
    msr	mair_el3, x0
    ```
    to:
    ```
    msr	mair_el2, x0
    ```
* **line 207 and 214 - sctlr_el3** - Modify to EL2.
From:

    ```
    mrs	x0, sctlr_el3
    msr	sctlr_el3, x0
    ```
    to:
    ```
    mrs	x0, sctlr_el2
    msr	sctlr_el2, x0
    ```


## Other modifications to rdimon-aem-el3.S to consider

* Set up a different MMU configuration by modifying or replacing the `_flat_map` function.
* Modify the exception vector table by modifying or replacing the `_init_vectors` function.
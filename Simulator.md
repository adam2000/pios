# Introduction #

The simulator provides a means of testing functionality in OS X without having an actual Raspberry Pi present.  It works by running the kernel in a thread and running the "hardware" in another thread.  So, for instance, the system timer is increment by the hardware thread approximately every microsecond.

The simulator is implemented as a Cocoa non document based application.

# PIOS\_SIMULATOR define #

In various parts of the code, it is necessary to do extra stuff in the simulator that you don't need with real hardware.  For example, writing to the GPIO.  There are also functions required for each subsystem that will allow the simulator to have access to internals that the normal thread won't.  These are all protected with ` #if defined PIOS_SIMULATOR ` guards.

# Limitations and Issues #

## Memory Management ##
Memory management has to be done totally differently in the simulator.  On the device, the kernel has full access to the entire physical address space.  This means that there is a lot of code required in the simulator to set up the memory map not needed in the real device.

## Pointers ##

Modern Cocoa apps are 64 bit.  The size of an address is therefore different as compared to the 32 bit address of the ARM CPU.  Thus any structures containing addresses - the videocore postbox and the memory boot tag, for instance are of different sizes in the simulator.

## Aliased RAM ##

The Broadcom SoC maps the SDRAM to three places in the chip's physical address space (note, not the ARM physical address space) each with different cache characteristics.  This is pretty much impossible to emulate in the simulator.
## PIOS ##


This is a simple operating system kernel targeted at the Raspberry Pi.  It's inspired by the BakingPi tutorial at http://www.cl.cam.ac.uk/freshers/raspberrypi/tutorials/os/ but will go much further and will b written almost entirely in C.

Hopefully this will provide a useful learning resource.

## Status As Of 07/12/2012 ##

I have added a framework for handling interrupts, although the only kind that works at the moment is the swi (software interrupt).  We even have two system calls.

## Status As Of 27/11/2012 ##

Things have moved on a lot.

The code now has support for testing with the QEMU emulator.  This means that for most of the hardware drivers there is now an abstraction pattern whereby each driver has a set of virtual functions to abstract hardware away from the OS.  Thus, although the graphics hardware of the Pi is programmed in a totally different way to the PL110 of the QEMU VersatilePB emulation, to the GDI, the frame buffer looks identical.

The other big piece of added functionality is that the OS now supports threads.  So far, the threading model is cooperative.  This means that each thread has to explicitly call the `thread_reschedule()` function to force a context switch because I have not yet added support for interrupts.  The tag for this is `context_switch_demo`.  The demo creates a thread that decrements a counter in a tight loop (with a call to `thread_reschedule()` in each iteration) while the main thread displays the value of the counter in a tight loop.

## Status As Of 27/10/2012 ##

The status is now that we are at the end of Screen03 but extra features.  The console module is able to print hex numbers and the tags module recognises a limited set of tags, including all the ones that the Pi seems to send.

As a bonus, once the Pi has printed all the tags to the screen, it continuously displays the microsecond counter in the system timer.

## Status As Of 25/10/2012 ##

The code as of today is effectively up to the Baking Pi tutorials, lesson Screen03.  This is to say that it is capable of displaying a sequence of characters on the screen.  Note that the current code base has not been tested on the Raspberry Pi as yet so there may still be bugs.
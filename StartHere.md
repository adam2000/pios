# Introduction #

This is the PIOS Wiki.

# Details #

## What is Here ##

There are two targets for this project at the moment

  1. A pure C image which is the operating system kernel.
  1. A Mac OS X application which simulates some features of the Pi hardware so that code can be tested without access to the Pi.

## Compilation ##

To compile the OS, you need an ARM EABI cross compiler.  I got mine from MacPorts.  Once you have that and have amended the first line of the makefile to point to it, just type _make_ to build the kernel image.

If you have a Mac, get Xcode and run the BakingPi target.  Alternatively, run the simulator target to get the simulator.

## Installation ##

The easiest way to install the code is to get an SD card with a Raspberry Pi Linux on it and replace kernel.img with the compiled kernel (see above).  If you have a recent Linux install, you will need to add

> ` kernel_old=1 `

to _config.txt_.  This is because the linker currently produces a binary that it expects to load at address 0 rather than 0x8000 which is where recent Pi Linuxes expect to load.
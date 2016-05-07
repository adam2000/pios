# Introduction #

Here we have a description of the major components of the program.

The code is constructed of a number of modules.  The basic pattern for each one
is the same:

  * There is an opaque data type that is used to bundle data and functionality.  In many cases, there can be only one object of the type in a real Raspberry Pi.  For instance, there is only one GPIO controller.
  * Functionality is implemented with C functions, passing a pointer to the   object as the first parameter.
  * Modules build on other modules e.g. The console uses a GDI context, which in turn uses the frame buffer.

# Physical Memory #

The physical memory object is fundamental to just about everything else.  It
provides access to the memory map e.g. where the hardware devices are and also
access to the configuration of the memory + the lowest level of memory
allocation.

# GPIO #

Provides access to GPIO functionality including setting pin functions,
inputting and outputting from/to the GPIO pins.

GPIO pin 16 is wired to the OK LED on the Raspberry Pi.  Generally speaking,
the code switches it off (by setting the pin true) when performing some new
function that might fail and then switching it back on again when the function
has finished successfully.  This provides the most basic diagnostic facility.

# System Timer #

Provides access to the SoC system timer including a function that waits for a
number of microseconds.

# Frame Buffer #

Provides access to frame buffers.  The frame buffer module is divided into an
abstraction layer and a hardware specific layer.  When the frame buffer is
first initialised, it is done so with a specific hardware driver that provides
a set of virtual functions that access hardware services.

# GDI #

Provides access to low level graphics utilities.  The GDI abstracts the frame
buffer into a two dimensional grid of 32 bit RGBA pixels.  The GDI performs
translation of RGBA values into device colours and some graphics primitives.  It
also contains a simple 8 x 16 font character drawing function.

# Console #

Abstracts the GDI screen into a character stream.  You write to the console in a
way analogous to a file and the console draws the characters on the screen,
remembering the cursor position etc and handling line feeds.

# Tag #

A library for reading ARM boot tags.

# klib #

A library of useful functions that replace common standard C lib functions.  The
C library is obviously not linked with the OS kernel.
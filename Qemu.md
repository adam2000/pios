# Introduction #

After a point, the simulator isn't really any good for testing stuff.  So we
progress to using QEMU which is amongst other things, a proper ARM system
emulator.

# Building #

Use the makefile ` make.qemu ` to build an image for QEMU.  The main
difference is in where the program loads from (0x10000 instead of 0x8000).
Also, the hardware is different too, so there may be other changes there.

# QEMU start #

The command line needed is:

```
qemu-system-arm -M versatilepb -kernel kernel.img -serial none -monitor stdio
```

versatilepb is a reasonable machine architecture I think.  -kernel loads the
kernel image at address 0x10000.

The script ` qemu_run.sh ` runs QEMU with the right parameters.

# Tags #

We seem to get only two tags:

```
0x00000005 0x54410001 0x00000001 0x00001000 0x00000000 
0x00000004 0x54410002 0x08000000 0x00000000 
0x00000000 0x00000000
```

The first tag is the core tag and the second is the memory tag.  There's no
command line, so we can use that to differentiate between QEMU and Pi.

# Debugging #

The easiest way to debug seems to be to run gdb remotely.  Start QEMU with the
options to stop QEMU immediately on load.  The script takes an option ` --debug ` that will do this.

Then run the debugger
```
arm-none-eabi-gdb
```

Connect to the remote target and load the elf file to give debugging symbols.
See the below example session

```
(gdb) target remote localhost:1234
Remote debugging using localhost:1234
0x00000000 in ?? ()
(gdb) file build.qemu/output.elf 
A program is being debugged already.
Are you sure you want to change the file? (y or n) y
Reading symbols from /Users/jeremyp/dev/pios/build.qemu/output.elf...(no debugging symbols found)...done.
(gdb) stepi
0x00000004 in ?? ()
(gdb) 
0x00000008 in ?? ()
(gdb) 
0x0000000c in ?? ()
(gdb) 
0x00010000 in ?? ()
(gdb) 
0x00010004 in _start ()
(gdb) disassemble
Dump of assembler code for function _start:
=> 0x00010004 <+0>:	ldr	sp, [pc, #12]	; 0x10018 <loop$+4>
   0x00010008 <+4>:	mov	r0, #1
   0x0001000c <+8>:	ldr	r1, [pc, #8]	; 0x1001c <loop$+8>
   0x00010010 <+12>:	bl	0x11ba8 <main>
End of assembler dump.
(gdb) 

```
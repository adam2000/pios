# Threads #

## Structure ##

A thread consists of the processor state and a stack.  Apart from a few minor
details that's it.  Active threads are held in the ready queue if they can be
executed or in the currentThread variable if they are being executed.  The
exception to this rule is the main thread which has responsibility for
managing all of the other threads.  Only the main thread is allowed to
manipulate the data structures associated with threading.  This obviates the
need for any locking of the thread structures.

## Context Switches ##

In any context switch, the main thread must be one of the two participating
threads.  We either switch from the main thread to the first thread on the
ready queue or we switch from another thread to the main thread.  In the
second case, the first thing the main thread does after regaining control is to
put the old thread on the back of the ready queue (unless the old thread is no
longer runnable).

# Interrupts #

## General ##

To keep things simple for now, we set each interrupt vector to load an absolute
address from a vector table to the PC.  The interrupt vectors for ARM are
actually ARM CPU instructions, so the IRQ might be of the form of
```
  b irqVector
```
where irqVector is the address of the IRQ routine.  This particular form is
limited though in that the address is only a 28 bit address (4 byte aligned).
We use a `ldr` instruction to load the address from a vector table.  In fact,
if the vectors are in the same order as the ARM vectors, we can use the same
instruction for all of the interrupt vectors namely:
```
	ldr	pc, [pc, #24]
```
as long as our vector table immediately follows the ARM vectors (by the time
the PC is loaded, it is 2 words beyond the location of the instruction).  The
above instruction is 0xE59FF018

## System calls ##

System calls are effected through the swi interrupt handler.  Each syscall
takes a parameter that is the system call number and an in and out pointer.

The interrupt handler for swi saves the saved PSR and the link register on the
stack.  It then adjusts the parameters to add the SWI number to the front of
the parameter list before calling the dispatcher.  On return from the
dispatcher, it restores all of the above.

We should also save the stack pointer of the mode we came from, but this is not
necessary when coming from SVC mode.
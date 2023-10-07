
# Memory Model

This page documents how Tractor applications interact with memory.

## Memory Organization

Each target platform has a "memory organization", which consists of the following properties:

* Pointer size
* Endianness
* Word size

At comptime, Tractor emulates the memory organization of the target platform so that values have the same arrangement of bytes. This facilitates the same code to work at both comptime and runtime.

Comptime pointers are represented as a single "dense byte" followed by padding to match the pointer size of the target platform. The compiler may store more than one byte of information in the dense byte, which enables the compiler to use a larger address space than the target platform would otherwise permit. The dense byte cannot be used in integer arithmetic operations, but it can be copied from one location to another.

In order to determine the size and arrangement of foreign structs, the Tractor compiler queries the lower-level compiler of the target platform. The exact query mechanism depends on which lower-level compiler is used. If the lower-level compiler has an API to query information about structs, the API will be the preferred mechanism. Otherwise, Tractor will pass dummy code through the lower-level compiler, and then read compiled output files to determine struct size and arrangement.

## Memory Regions

Tractor can store values in two memory regions:

* The stack, which is part of RAM
* The "fixed data region", which may be non-volatile on certain target platforms

The stack is mutable, while the fixed data region is immutable. Examples of fixed data regions include the following:

* The PROGMEM space in an AVR microcontroller
* Flash memory in a PIC microcontroller
* ROM in the address space of a 6502 microprocessor

On target platforms where RAM is abundant, the fixed data region may be in RAM, even though Tractor code considers the fixed data region to be immutable.

The stack contains one or more frames, each of which stores the data of application variables. At the beginning of the stack's life cycle, the stack contains a single frame to store global variables. When a function is invoked, a new frame is added to the stack to store local variables of the function. When function invocation finishes, the frame is removed from the stack.

At comptime, Tractor emulates a stack and a fixed data region. Because runtime and comptime have the same memory regions, functions can be shared more easily between evaluation times.



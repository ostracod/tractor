
# Evaluation Order

This page documents the order in which expressions and statements are evaluated.

## Evaluation Time

In Tractor, the life cycle of each application consists of two "evaluation times":

* Compile-time ("comptime") occurs when the application is compiled from Tractor into a lower-level language.
* Runtime occurs when the end-user executes the compiled Tractor application.

Expressions and statements may be marked with a "time suit", which determines the evaluation times at which the code can be evaluated. Code may have the following time suits:

* "Comp-suit" code is able to be evaluated at comptime.
* "Run-suit" code is able to be evaluated at runtime.
* "Any-suit" code is able to be evaluated at both comptime and runtime.

Values and types are also associated with a time suit, which determines the evaluation times at which the content of the items is known. Items may have the following time suits:

* Comp-suit items have content known at comptime.
* Run-suit items have content known at runtime.
* Any-suit items have content known at both comptime and runtime.

Some categories of items have time suit restrictions. For example:

* Types and macros cannot be run-suit, because the runtime environment has no representation of types or macros.
* Values imported from foreign (non-Tractor) code cannot be comp-suit, because the Tractor compiler does not read values in foreign code.

When Tractor code references an item with an incompatible time suit, many operations on the item are forbidden. For example:

* Comp-suit code cannot add two run-suit integers.
* Run-suit code cannot dereference a comp-suit pointer.
* Any-suit code cannot invoke a run-suit function.

As a notable exception, code can pass and store items even when the items have an incompatible time suit. For instance, comp-suit code can hold run-suit items, and then later transfer the items to run-suit code.

## Evaluation Phase

Each expression and statement may exist in one of two "evaluation phases", which is a distinct concept from evaluation time:

* During "prep-phase", the compiler gathers type information about operands in the code, and precomputes items which may be needed for evaluation.
* During "flow-phase", the code is evaluated, and is subject to the rules of control flow.

Code must complete prep-phase before it can enter flow-phase. Once prep-phase is finished, the code may enter flow-phase one or more times. Furthermore, flow-phase may occur during comptime or runtime, depending on the time suit of the code.

Each expression has one of three "phase grades", which influences the order in which expressions are evaluated. The phase grade of an expression is determined by the bracket delimiters which enclose the expression. Expressions may have the following phase grades:

* "Prep-grade" expressions are evaluated during prep-phase of their parent code.
* "Flow-grade" expressions are evaluated during flow-phase of their parent code.
* "Never-grade" expressions are never evaluated. Instead, the compiler may derive type information from never-grade expressions.

Prep-grade expressions are always evaluated during comptime, even when they are nested in run-suit code.

Variables are also associated with a phase grade, which influences the order in which variables are initialized. The phase grade of a variable is determined by the first keyword in the variable's declaration statement. Variables may have the following phase grades:

* Prep-grade variables are initialized during prep-phase of their parent code.
* Flow-grade variables are initialized during flow-phase of their parent code.

Each variable has a "phase access", which indicates the evaluation phases during which code may access the variable. The phase access of a variable is determined by the first keyword in the variable's declaration statement. Variables may have the following phase accesses:

* "Prep-access" variables may be accessed during prep-phase of their parent code.
* "Flow-access" variables may be accessed during flow-phase of their parent code.
* "Any-access" variables may be accessed during prep-phase or flow-phase of their parent code.



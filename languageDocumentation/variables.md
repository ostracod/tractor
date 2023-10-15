
# Variables and Arguments

This page documents variables and arguments in Tractor.

## Variables

Each variable declaration statement provides the following information:

* A keyword, which may be `prepFrame`, `flowFrame`, `anyFrame`, `prepFixed`, `anyFixed`, `prepDef`, or `anyDef`
* An identifier, which identifies the variable in the current scope
* An optional "boundary type"
* An optional "initialization item", which determines the initial item stored in the variable

The keyword determines the phase access and phase grade of the variable:

* `prepFrame`, `prepFixed`, and `prepDef` variables are prep-access and prep-grade.
* `flowFrame` variables are flow-access and flow-grade.
* `anyFrame`, `anyFixed`, and `anyDef` variables are any-access and prep-grade.

Each variable keyword is also associated with a storage type:

* The storage type associated with `prepFrame`, `flowFrame`, and `anyFrame` variables is `frameT`.
* The storage type associated with `prepFixed` and `anyFixed` variables is `fixedT`.
* The storage type associated with `prepDef` and `anyDef` variables is `defT`.

Each variable has a "constraint type", which is the type of the variable when referenced in an expression. The constraint type of a variable is the intersection of the following types:

* The storage type associated with the keyword of the variable
* The boundary type of the variable, if the variable has a boundary type
* The constraint type of the initialization item, if the variable has an initialization item

Furthermore, the constraint type of a variable conforms to `~mutT` during prep-phase.

Neither comp-suit nor run-suit invocables can reference non-global variables in the parent scope during flow-phase.

## Arguments

When passing an item to a function invocation, the item is copied and stored in the function argument. Each function argument has an identifier and a boundary type. However, function argument statements do not begin with a keyword, and do not specify initialization items. Function arguments behave as `flowFrame` variables. In other words, each function argument is flow-access and flow-grade, and `frameT` is the storage type associated with function arguments. The constraint type of a function argument is the intersection of `frameT` and the boundary type of the argument.

When passing an item to a macro invocation, the item is not copied. Instead, the argument references the item in its original storage location. In a similar fashion to function arguments, each macro argument has an identifier and a boundary type. Macro argument statements do not begin with a keyword, and do not specify initialization items. Macro argument statements are not associated with a storage type. Instead, macro arguments adopt the storage type of the item passed during each macro invocation. The constraint type of a macro argument is the intersection of the argument boundary type and the constraint type of the item passed during macro invocation.

Prep-macro arguments are prep-access, and conform to `~mutT`. Prep-macro type signatures are permitted to reference their own arguments. Flow-macro arguments are flow-access. Flow-macro type signatures are permitted to reference the constraint types of their own arguments.



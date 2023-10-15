
# Data Types

This page documents the data types in Tractor.

## Basic Types

A "basic type" describes the kind of information which an item represents, independent of the manner in which the item is stored. Tractor has the following basic types:

* `itemT` is the most generic basic type. All basic types conform to `itemT`.
* `typeT` is the type of a type. The compiler can use types to constrain the items stored in variables and passed through expressions.
* `valueT` is the type of a value. The compiler cannot use values as constraint types.
* `voidT` is the type of an empty value. Void occupies zero bytes.
* `intT` is the type of an integer with an unknown number of bits and unknown sign.
* `uIntT` and `sIntT` are the types of unsigned and signed integers respectively with an unknown number of bits.
* `int8T`, `int16T`, `int32T`, and `int64T` are the types of integers with the given number of bits and unknown sign.
* `uInt8T`, `uInt16T`, `uInt32T`, and `uInt64T` are the types of unsigned integers with the given number of bits.
* `sInt8T`, `sInt16T`, `sInt32T`, and `sInt64T` are the types of signed integers with the given number of bits.
* `boolT` is the type of a boolean value. Booleans may be true or false.
* `ptrT` is the type of a pointer. A pointer stores the address of an item in memory.
* `nullT` is the type of a null pointer. Null represents a missing item.
* `arrayT` is the type of an array. An array can store one or more elements, each having a common type.
* `strT` is the type of a string. A string is an array of `uInt8T` characters ending with `0x00`.
* `structT` is the type of a struct. A struct can store one or more fields, each having a distinct name and type. The fields do not overlap each other in memory.
* `unionT` is the type of a union. A union can also store one or more fields, but the fields overlap because they all start at the same memory address.
* `invocT` is the type of an invocable. Invocables are able to be invoked with one or more arguments, and return an item.
* `macroT` is the type of a macro. When a macro is invoked, the body of the macro is expanded inline.
* `prepMacroT` is the type of a prep-macro. Prep-macros accept arguments known during prep-phase.
* `flowInvocT` is the type of a flow-invocable. Flow-invocables accept arguments known during flow-phase.
* `flowMacroT` is the type of a flow-macro. Flow-macros accept arguments known during flow-phase.
* `funcT` is the type of a function. Only one copy of each function body is stored in the application. When a function is invoked, control flow jumps to the function body.
* `labelT` is the type of a label. Labels mark statement positions within the body of an invocable.
* `moduleT` is the type of a code file. Each module may export one or more variables.

## Storage Types

A "storage type" describes the manner in which an item is stored, and determines how the item may be accessed. Tractor has the following storage types:

* `concreteT` is the type of an item which can be instantiated. Concrete items must have a well-defined memory arrangement.
* `mutT` is the type of a mutable item. Any byte in a mutable item may be changed after instantiation.
* `compSuitT` is the type of a comp-suit item. The content of comp-suit items is known at comptime.
* `runSuitT` is the type of a run-suit item. The content of run-suit items is known at runtime.
* `anySuitT` is the type of an any-suit item. The content of any-suit items is known at both comptime and runtime.
* `addrT` is the type of an addressable item. Pointers may be created for addressable items.
* `frameT` is the type of an item stored in a stack frame. The stack is stored in RAM.
* `fixedT` is the type of an item stored in the fixed data region. The fixed data region may be non-volatile on certain target platforms.

## Type Relationships

Basic types have the following relationships:

* `valueT` and `typeT` conform to `itemT`.
* `voidT`, `intT`, `ptrT`, `arrayT`, `structT`, `unionT`, `invocT`, `labelT`, and `moduleT` conform to `valueT`.
* `uIntT`, `sIntT`, `int8T`, `int16T`, `int32T`, and `int64T` conform to `intT`.
* `uInt8T`, `uInt16T`, `uInt32T`, and `uInt64T` conform to `uIntT`.
* `sInt8T`, `sInt16T`, `sInt32T`, and `sInt64T` conform to `sIntT`.
* `uInt8T` and `sInt8T` conform to `int8T`.
* `uInt16T` and `sInt16T` conform to `int16T`.
* `uInt32T` and `sInt32T` conform to `int32T`.
* `uInt64T` and `sInt64T` conform to `int64T`.
* `boolT` conforms to `uInt8T`.
* `nullT` conforms to `ptrT`.
* `strT` conforms to `arrayT(uInt8T)`.
* `macroT` and `flowInvocT` conform to `invocT`.
* `prepMacroT` and `flowMacroT` conform to `macroT`.
* `flowMacroT` and `funcT` conform to `flowInvocT`.

Storage types have the following relationships:
* `uInt8T`, `uInt16T`, `uInt32T`, `uInt64T`, `sInt8T`, `sInt16T`, `sInt32T`, and `sInt64T` conform to `concreteT`.
* `voidT`, `labelT`, and `moduleT` also conform to `concreteT`.
* A `ptrT` conforms to `concreteT` if the element type conforms to `frameT` or `fixedT`.
* A non-soft `arrayT` conforms to `concreteT` if the element type conforms to `concreteT`.
* A non-soft `structT` and `unionT` conform to `concreteT` if every field type conforms to `concreteT`.
* An `invocT` conforms to `concreteT` when the arguments and return type are well-defined.
* If an `arrayT` conforms to `mutT`, then the element type conforms to `mutT`.
* If a `structT` or `unionT` conforms to `mutT`, then every field type conforms to `mutT`.
* `fixedT` and `defT` conform to `~mutT`.
* `typeT`, `invocT [compSuit]`, `labelT`, `moduleT`, and `anySuitT` conform to `compSuitT`.
* The types of foreign items conform to `~compSuitT`.
* `invocT [runSuit]` and `anySuitT` conform to `runSuitT`.
* `voidT`, `intT`, `ptrT`, and `invocT [anySuit]` conform to `anySuitT`.
* `arrayT` conforms to a suit type if the element type conforms to the same suit type.
* `structT` and `unionT` conform to a suit type if every field type conforms to the same suit type.
* `frameT` and `fixedT` conform to `addrT`.
* `defT` conforms to `~addrT`.
* The element type of `ptrT` conforms to `addrT`.
* `fixedT` conforms to `~frameT`.
* `frameT` conforms to `~fixedT`.



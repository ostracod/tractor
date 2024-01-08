
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
* `charT` is the type of a character. Characters use ASCII encoding.
* `ptrT` is the type of a pointer. A pointer stores the address of an item in memory.
* `nullT` is the type of a null pointer. Null represents a missing item.
* `arrayT` is the type of an array. An array can store one or more elements, each having a common type.
* `strT` is the type of a string. A string is an array of `charT` ending with `0x00`.
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

* `concreteT` is the type of an item which has a well-defined memory arrangement. All variable constraint types must conform to `concreteT`.
* `mutT` is the type of a mutable item. Any byte in a mutable item may be changed after instantiation.
* `compSuitT` is the type of a comp-suit item. The content of comp-suit items is known at comptime.
* `runSuitT` is the type of a run-suit item. The content of run-suit items is known at runtime.
* `anySuitT` is the type of an any-suit item. The content of any-suit items is known at both comptime and runtime.
* `addrT` is the type of an addressable item. Pointers may be created for addressable items.
* `ramT` is the type of an item stored in RAM. RAM contains both the stack and the heap.
* `fixedT` is the type of an item stored in the fixed data region. The fixed data region may be non-volatile on certain target platforms.
* `defT` is the type of an item which is copied inline every time the item is referenced. Definitions are not stored in RAM or the fixed data region.

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
* `uInt8T` conforms to `uInt16T`, `uInt16T` conforms to `uInt32T`, and `uInt32T` conforms to `uInt64T`.
* `sInt8T` conforms to `sInt16T`, `sInt16T` conforms to `sInt32T`, and `sInt32T` conforms to `sInt64T`.
* `boolT` and `charT` conform to `uInt8T`.
* `nullT` conforms to `ptrT`.
* `strT` conforms to `arrayT(charT)`.
* `macroT` and `flowInvocT` conform to `invocT`.
* `prepMacroT` and `flowMacroT` conform to `macroT`.
* `flowMacroT` and `funcT` conform to `flowInvocT`.

Composite types have the following relationships:

* Suppose that `T1` and `T2` conform to `ptrT`, but not `ptrT(mutT)`. `T1` conforms to `T2` if all of the following conditions are true:
    * The element type of `T1` conforms to the element type of `T2`.
    * The element types of `T1` and `T2` have the same memory arrangement.
* Suppose that `T1` and `T2` conform to `ptrT(mutT)`. `T1` conforms to `T2` only if `T1` and `T2` have identical element types.
* Suppose that `T1` and `T2` conform to `arrayT`. `T1` conforms to `T2` if all of the following conditions are true:
    * `T1` and `T2` have the same length.
    * The element type of `T1` conforms to the element type of `T2`.
* Suppose that `T1` and `T2` conform to `structT`. `T1` conforms to `T2` if all of the following conditions are true:
    * `T1` and `T2` have the same field names.
    * The field types of `T1` conform to the field types of `T2`.
* Suppose that `T1` and `T2` conform to `unionT`. `T1` conforms to `T2` if all of the following conditions are true:
    * `T1` and `T2` have the same field names.
    * The field types of `T1` conform to the field types of `T2`.
    * The field types of `T1` and `T2` have the same memory arrangements.
* Suppose that `T1` and `T2` conform to `funcT`. `T1` conforms to `T2` if all of the following conditions are true:
    * `T1` and `T2` accept the same number of arguments.
    * The argument types of `T2` conform to the argument types of `T1`.
    * The argument types of `T1` and `T2` have the same memory arrangements.
    * The return type of `T1` conforms to the return type of `T2`.
    * The return types of `T1` and `T2` have the same memory arrangement.

Storage types have the following relationships:

* `uInt8T`, `uInt16T`, `uInt32T`, `uInt64T`, `sInt8T`, `sInt16T`, `sInt32T`, and `sInt64T` conform to `concreteT`.
* `typeT`, `voidT`, `invocT`, `labelT`, and `moduleT` also conform to `concreteT`.
* A `ptrT` conforms to `concreteT` if the element type conforms to `ramT` or `fixedT`.
* A non-soft `arrayT` conforms to `concreteT` if the element type conforms to `concreteT`.
* A non-soft `structT` and `unionT` conform to `concreteT` if every non-phantom field type conforms to `concreteT`.
* If an `arrayT` conforms to `mutT`, then the element type conforms to `mutT`.
* If a `structT` or `unionT` conforms to `mutT`, then every field type conforms to `mutT`.
* `fixedT` and `defT` conform to `~mutT`.
* `invocT [compSuit]` and `anySuitT` conform to `compSuitT`.
* The types of foreign items conform to `~compSuitT`.
* `invocT [runSuit]` and `anySuitT` conform to `runSuitT`.
* `typeT`, `labelT`, and `moduleT` conform to `~runSuitT`.
* `voidT` and `invocT [anySuit]` conform to `anySuitT`.
* `arrayT` conforms to a suit type if the element type conforms to the same suit type.
* `structT` and `unionT` conform to a suit type if every field type conforms to the same suit type.
* `ramT` and `fixedT` conform to `addrT`.
* `defT` conforms to `~addrT`.
* The element type of `ptrT` conforms to `addrT`.
* `fixedT` conforms to `~ramT`.
* `ramT` conforms to `~fixedT`.

## Type Conversion

Tractor has three varieties of type conversion:

* "Safe" type conversion does not violate any type relationships, and preserves all data.
    * Occurs explicitly when using the safe type conversion operator (`:`)
    * Occurs implicitly when the operand of a statement or expression has a different type than expected
    * May add well-defined padding to data
* "Proximate" type conversion may convert data between types which are related, but not strictly compatible.
    * Only occurs explicitly when using the proximate type conversion operator (`::`)
    * Disregards nominal typing
    * May truncate data
* "Dangerous" type conversion ignores type restrictions to the greatest extent possible.
    * Only occurs explicitly when using the dangerous type conversion operator (`:::`)
    * May reinterpret bytes as an unrelated type
    * May cause pointers to reference malformed data
    * May add undefined padding to data

Safe conversion from type `T1` to type `T2` is permitted only when `T1` conforms to `T2`.

Proximate conversion from type `T1` to type `T2` is permitted in the following scenarios:

* All scenarios in which safe type conversion is allowed.
* `T1` and `T2` conform to `intT`.
* `T1` conforms to `ptrT($elemT1)`, `T2` conforms to `ptrT($elemT2)`, and all of the following conditions are true:
    * Proximate conversion is allowed from `$elemT1` to `$elemT2`.
    * The memory arrangement of `$elemT1` is compatible with that of `$elemT2`.
* `T1` conforms to `arrayT [len ($len1)] ($elemT1)`, `T2` conforms to `arrayT [len ($len2)] ($elemT2)`, and all of the following conditions are true:
    * `$len2` is not larger than `$len1`.
    * Proximate conversion is allowed from `$elemT1` to `$elemT2`.
* `T1` and `T2` conform to `structT`, and all of the following conditions are true:
    * All field names in `T2` are also in `T1`.
    * Proximate conversion is allowed from field types in `T1` to field types in `T2`.
* `T1` and `T2` conform to `unionT`, and all of the following conditions are true:
    * All field names in `T2` are also in `T1`.
    * Proximate conversion is allowed from field types in `T1` to field types in `T2`.
    * The memory arrangements of field types in `T1` are compatible with those of field types in `T2`.
* `T1` conforms to `funcT [retT ($retT1)]`, `T2` conforms to `funcT [retT ($retT2)]`, and all of the following conditions are true:
    * `T1` and `T2` accept the same number of arguments.
    * Proximate conversion is allowed from argument types in `T2` to argument types in `T1`.
    * Proximate conversion is allowed from `$retT1` to `$retT2`.
    * The memory arrangements of argument types in `T2` are compatible with those of argument types in `T1`.
    * The memory arrangement of `$retT1` is compatible with that of `$retT2`.

Dangerous conversion from type `T1` to type `T2` is permitted in the following scenarios:

* All scenarios in which proximate type conversion is allowed.
* `T1` and `T2` conform to `intT | ptrT | invocT | typeT | labelT | moduleT`.
    * For example: `T1` conforms to `intT`, and `T2` conforms to `ptrT`.
* `T1` conforms to `arrayT($elemT1)`, `T2` conforms to `arrayT($elemT2)`, and dangerous conversion is allowed from `$elemT1` to `$elemT2`.
* `T1` and `T2` conform to `structT`, and dangerous conversion is allowed from field types in `T1` to field types in `T2`.
* `T1` and `T2` conform to `unionT`.



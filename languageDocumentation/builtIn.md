
# Built-In Constants

This page documents built-in constants in Tractor.

## Built-In Items

Tractor includes the following built-in items:

* `null` = Null pointer value
* `true` = True boolean value
* `false` = False boolean value
* `itemT`, `typeT`, `valueT`, `voidT`, `nullT`, and `labelT` = Basic types as described earlier in this documentation
* `intT`, `uIntT`, `sIntT`, `int8T`, `int16T`, `int32T`, `int64T`, `uInt8T`, `uInt16T`, `uInt32T`, `uInt64T`, `sInt8T`, `sInt16T`, `sInt32T`, `sInt64T`, and `boolT` = Integer types as described earlier in this documentation
* `concreteT`, `mutT`, `compSuitT`, `runSuitT`, `anySuitT`, `addrT`, `ramT`, and `fixedT` = Storage types as described earlier in this documentation

## Built-In Invocables

Tractor includes the following built-in invocables:

### Type Invocables:

**Identifier:** `nominalT`  
**Type:** `funcT [compSuit, args [type (typeT)], retT (typeT)]`

Returns a subtype of `type` which defines the same data structure as `type`, but is distinguished through the nominal type system. For example, `nominalT(intT)` conforms to `intT`, but `intT` does not conform to `nominalT(intT)`.

**Identifier:** `literalT`  
**Type:** `flowMacroT [compSuit, args [item (itemT)], retT (typeT)]`

Returns a type which only contains items equal to `item`. For example, `<literalT>(16)` is the type of all numbers which are equal to 16.

**Identifier:** `basicT`  
**Type:** `funcT [compSuit, args [type (typeT)], retT (typeT)]`

Returns the basic type of `type`. For example, `basicT(intT & ramT)` returns `intT`.

**Identifier:** `suitT`  
**Type:** `funcT [compSuit, args [type (typeT)], retT (typeT)]`

Returns the time suit type of `type`. For example, `suitT(intT & runSuitT)` returns `runSuitT`.

**Identifier:** `typeConforms`  
**Type:** `funcT [compSuit, args [type1 (typeT), type2 (typeT)], retT (boolT)]`

Returns whether `type1` conforms to `type2`. For example, `typeConforms(uIntT, intT)` returns true, and `typeConforms(intT, uIntT)` returns false.

### Size Function:

**Identifier:** `size`  
**Type:** `funcT [compSuit, args [type (typeT)], retT (uInt64T)]`

Returns the number of bytes which `type` occupies in memory. For example, `size(uInt32T)` returns 4.

### Length Function:

**Identifier:** `len`  
**Type:** `funcT [compSuit, args [type (typeT)], retT (uInt64T)]`

Returns the number of elements which `type` contains. For example, `len(arrayT [len (5)])` returns 5.

### Element Type Function:

**Identifier:** `elemT`  
**Type:** `funcT [compSuit, args [type <?ptrT | arrayT>], retT (typeT)]`

Returns the element type of `type`. For example, `elemT(arrayT(uInt16T))` returns `uInt16T`.

### Field Invocables:

**Identifier:** `fieldNameT`  
**Type:** `funcT [compSuit, args [type <?structT | unionT>], retT <?ptrT(strT)>]`

Returns the type of field names in `type`. For example, `fieldNameT(structT [fields [first (boolT), second (nullT)]])` returns `<literalT>("first") | <literalT>("second")`.

**Identifier:** `fieldName`  
**Type:** `flowMacroT [compSuit, args [type <?structT | unionT>, index (uInt64T)], retT (fieldNameT<?type>)]`

Returns the name of a field in `type`. For example, `<fieldName>(structT [fields [first (boolT), second (nullT)]], 0)` returns `"first"`.

**Identifier:** `fieldT`  
**Type:** `flowMacroT [compSuit, args [type <?structT | unionT>, name (fieldNameT<?type>)], retT (typeT)]`

Returns the type of a field in `type`. For example, `<fieldT>(structT [fields [first (boolT), second (nullT)]], "second")` returns `nullT`.

**Identifier:** `fieldOffset`  
**Type:** `flowMacroT [compSuit, args [type <?structT | unionT>, name (fieldNameT<?type>)], retT (uInt64T)]`

Returns the number of bytes between the start of `type` and a field. For example, `<fieldOffset>(structT [fields [first (boolT), second (nullT)], pack (1)], "second")` returns 1.

### Signature Functions:

**Identifier:** `argT`  
**Type:** `funcT [compSuit, args [type <?invocT>, index (uInt64T)], retT (typeT)]`

Returns the type of an argument in `type`. For example, `argT(funcT [args [first (boolT), second (nullT)]], 0)` returns `boolT`.

**Identifier:** `retT`  
**Type:** `funcT [compSuit, args [type <?invocT>], retT (typeT)]`

Returns the return type of `type`. For example, `retT(funcT [retT (uInt32T)])` returns `uInt32T`.

### Print Macro:

**Identifier:** `compSuitPrint`  
**Type:** `flowMacroT [compSuit, args [item (itemT)], retT (voidT)]`

Prints `item` during compilation. For example, `<compSuitPrint>("Hello")` prints `"Hello"` during compilation.



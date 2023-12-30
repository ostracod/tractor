
# Example Code

This page provides examples of Tractor code.

The example below demonstrates the restrictions enforced by time suits:

```
anyDef myCompSuitFunc = <func [compSuit] {
    -- Does not throw an error, because `myCompSuitFunc` can be evaluated during comptime.
    (myCompSuitFunc())
    -- Throws an error, because `myRunSuitFunc` can only be evaluated during
    -- runtime, but the parent statement sequence is comp-suit.
    (myRunSuitFunc())
    -- Does not throw an error, because `myAnySuitFunc`
    -- can be evaluated during comptime or runtime.
    (myAnySuitFunc())
}>

anyDef myRunSuitFunc = <func [runSuit] {
    -- Throws an error, because `myCompSuitFunc` can only be evaluated during
    -- comptime, but the parent statement sequence is run-suit.
    (myCompSuitFunc())
    -- Does not throw an error, because `myRunSuitFunc` can be evaluated during runtime.
    (myRunSuitFunc())
    -- Does not throw an error, because `myAnySuitFunc`
    -- can be evaluated during runtime or comptime.
    (myAnySuitFunc())
}>

anyDef myAnySuitFunc = <func [anySuit] {
    -- Throws an error, because `myCompSuitFunc` can only be evaluated during
    -- comptime, but the parent statement sequence is any-suit.
    (myCompSuitFunc())
    -- Throws an error, because `myRunSuitFunc` can only be evaluated during
    -- runtime, but the parent statement sequence is any-suit.
    (myRunSuitFunc())
    -- Does not throw an error, because `myAnySuitFunc`
    -- can be evaluated during runtime or comptime.
    (myAnySuitFunc())
}>

-- Prep-phase always occurs during comptime. As a result, functions
-- which are not comp-suit cannot be invoked during prep-phase.
<myCompSuitFunc()> -- Does not throw an error.
<myRunSuitFunc()> -- Throws an error.
<myAnySuitFunc()> -- Does not throw an error.

entryPoint {
    -- Flow-phase of the application entry point occurs during runtime. As a result, functions
    -- which are not run-suit cannot be invoked during flow-phase of the entry point.
    (myCompSuitFunc()) -- Throws an error.
    (myRunSuitFunc()) -- Does not throw an error.
    (myAnySuitFunc()) -- Does not throw an error.
}
```

The example below demonstrates phase access and variable storage types:

```
entryPoint {
    prepFrame myPrepFrameVar = <10>
    flowFrame myFlowFrameVar = (11)
    flowFrame myMutFrameVar <mutT> = (12)
    anyFrame myAnyFrameVar = <13>
    anyFixed myAnyFixedVar = <14>
    anyDef myAnyDefVar = <15>
    
    -- `prepFrame` variables are prep-access, and can only be accessed during prep-phase.
    prepFrame result1 = <myPrepFrameVar> -- Does not throw an error.
    flowFrame result2 = (myPrepFrameVar) -- Throws an error.
    
    -- `flowFrame` variables are flow-access, and can only be accessed during flow-phase.
    prepFrame result3 = <myFlowFrameVar> -- Throws an error.
    flowFrame result4 = (myFlowFrameVar) -- Does not throw an error.
    
    -- `anyFrame` variables are any-access, and can be accessed during any evaluation phase.
    prepFrame result5 = <myAnyFrameVar> -- Does not throw an error.
    flowFrame result6 = (myAnyFrameVar) -- Does not throw an error.
    
    -- Throws an error, because myFlowFrameVar does not conform to `mutT`.
    (myFlowFrameVar = 20)
    -- Does not throw an error, because myMutFrameVar conforms to `mutT`.
    (myMutFrameVar = 20)
    -- Throws an error, because all variables conform to `~mutT` during prep-phase.
    <myMutFrameVar = 20>
    
    -- The type of `myFramePtr` is `ptrT(uInt8T & ramT)`.
    flowFrame myFramePtr = (ptr(myAnyFrameVar))
    -- The type of `myFixedPtr` is `ptrT(uInt8T & fixedT)`.
    flowFrame myFixedPtr = (ptr(myAnyFixedVar))
    -- Throws an error, because `defT` conforms to `~addrT`.
    flowFrame invalidPtr = (ptr(myAnyDefVar))
    
    -- `result7` stores 13.
    flowFrame result7 = (@myFramePtr)
    -- `result8` stores 14.
    flowFrame result8 = (@myFixedPtr)
}
```

The example below demonstrates import statements:

```
-- Imports the Tractor module with the path "src/moneyUtils.trtr" relative to the
-- current project directory. The module "moneyUtils.trtr" is accessible through
-- the variable `moneyUtils` in the current module. The type of `moneyUtils` depends
-- on the content of "moneyUtils.trtr".
tractorImport <"moneyUtils.trtr"> as moneyUtils

-- Imports the variable `cook` from the Tractor module with the path
-- "src/foodUtils.trtr" relative to the current project directory. `cook` is
-- accessible through the variable `cookFood` in the current module. The type
-- of `cookFood` depends on the content of "foodUtils.trtr".
tractorImport <"foodUtils.trtr"> [vars [var cook as cookFood]]

-- Imports the foreign module with the path "foreign/deviceUtils.h" relative to
-- the current project directory. The module "deviceUtils.h" is accessible
-- through the variable `deviceUtils` in the current module, and has the type
-- specified by the `foreignImport` statement.
foreignImport <"deviceUtils.h"> as deviceUtils <
    moduleT [exports [
        anyDef blinkLight (funcT [runSuit, args [], retT (voidT)])
    ]]
>

-- Imports the variable `defragment` from the foreign module with the path
-- "foreign/fileUtils.h" relative to the current project directory. `defragment` is
-- accessible through the variable `defragmentStorage` in the current module, and
-- has the type specified by the `anyDef` statement.
foreignImport <"fileUtils.h"> [vars [
    anyDef defragment as defragmentStorage <funcT [runSuit, args [], retT (voidT)]>
]]

-- Declares a variable which can be imported by other modules.
anyFrame myExportVar [export] = <30>
-- `myPrivateVar` cannot be imported by other modules, because
-- it does not have an `export` attribute statement.
anyFrame myPrivateVar = <31>

entryPoint {
    -- Invokes functions from the imported modules.
    (<moneyUtils>.payTaxes())
    (cookFood())
    (<deviceUtils>.blinkLight())
    (defragmentStorage())
}
```

For the remainder of the examples on this page, assume that `runSuitPrint` is an imported foreign flow-macro, and prints the argument value during runtime.

The example below demonstrates a primality test function which can be invoked at both comptime and runtime:

```
-- Defines a function which returns whether `value` is prime. The function can
-- be invoked at both comptime and runtime, because the function is any-suit.
anyDef isPrime = <func [anySuit, args [value <uInt32T>], retT <boolT>] {
    flowFrame factor <uInt32T & mutT> = (2)
    while (factor #lt value) {
        if (value % factor #eq 0) {
            ret (false)
        }
        (factor += 1)
    }
    ret (true)
}>

-- Prints whether 123 is prime at comptime ("false").
<<compSuitPrint>(isPrime(123))>

entryPoint {
    -- Prints whether 127 is prime at runtime ("true").
    (<runSuitPrint>(isPrime(127)))
}
```

The example below demonstrates usage of arrays and pointers:

```
-- Sorts the array `nums` in ascending order.
anyDef bubbleSort = <func [anySuit, args [
    nums <ptrT(arrayT(uInt16T) & ramT & mutT)>
    numsLen <uInt8T>
]] {
    flowFrame index <uInt8T & mutT> = (1)
    while (true) {
        flowFrame isSorted <mutT> = (true)
        while (index #lt numsLen) {
            flowFrame num1 = ((@nums)@(index - 1))
            flowFrame num2 = ((@nums)@index)
            if (num1 #gt num2) {
                ((@nums)@(index - 1) = num2)
                ((@nums)@index = num1)
                (isSorted = false)
            }
            (index += 1)
        }
        if (isSorted) {
            break
        }
    }
}>

entryPoint {
    -- Defines an array containing some elements.
    flowFrame numArray <mutT> = (array [elemT <uInt16T>] (743, 179, 785, 799, 471, 224))
    anyDef arrayLen = <len<?numArray>:<uInt8T>>
    -- Sorts the array in-place.
    (bubbleSort(ptr(numArray), arrayLen)
    -- Prints the sorted array.
    flowFrame index <uInt8T & mutT> = (0)
    while (index #lt arrayLen) {
        (<runSuitPrint>(numArray@index))
        (index += 1)
    }
}
```

The example below demonstrates usage of structs and unions:

```
entryPoint {
    -- Defines a "user" struct type containing three fields.
    -- Struct fields do not share any bytes in memory.
    prepDef userT = <structT [fields [
        name (ptrT(strT & ramT))
        score (uInt32T)
        isAdmin (boolT)
    ]]>
    -- Creates an instance of a user.
    flowFrame user <userT> = (struct [fields [
        name = ("Bob")
        score = (10)
        isAdmin = (false)
    ]])
    (<runSuitPrint>(user.name)) -- Prints "Bob".
    (<runSuitPrint>(user.score)) -- Prints "10".
    (<runSuitPrint>(user.isAdmin)) -- Prints "false".
    
    -- Defines a "mixed" union type containing two fields. Every field of a union begins
    -- at the same memory address, so union fields share one or more bytes.
    prepDef mixedT = <unionT [fields [
        bigInt (sInt64T)
        smallInts (arrayT [len (3)] (sInt16T))
    ]]>
    -- Creates an instance of the mixed union type.
    flowFrame mixed <mixedT & mutT>
    (mixed.bigInt = 1234567)
    (<runSuitPrint>(mixed.bigInt)) -- Prints "1234567".
    (mixed.smallInts = array [elemT (sInt16T)] (100, 200, 300))
    (<runSuitPrint>(mixed.smallInts@0)) -- Prints "100".
    (<runSuitPrint>(mixed.smallInts@1)) -- Prints "200".
    (<runSuitPrint>(mixed.smallInts@2)) -- Prints "300".
}
```

TODO: Add more example code.



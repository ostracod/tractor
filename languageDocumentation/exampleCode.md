
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

entryPoint {
    -- Flow-phase of the application entry point occurs during runtime. As a result, functions
    -- which are not run-suit cannot be invoked during flow-phase of the entry point.
    (myCompSuitFunc()) -- Throws an error.
    (myRunSuitFunc()) -- Does not throw an error.
    (myAnySuitFunc()) -- Does not throw an error.
    
    -- Prep-phase always occurs during comptime. As a result, functions
    -- which are not comp-suit cannot be invoked during prep-phase.
    <myCompSuitFunc()> -- Does not throw an error.
    <myRunSuitFunc()> -- Throws an error.
    <myAnySuitFunc()> -- Does not throw an error.
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

TODO: Add more example code.



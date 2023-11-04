
# Statements

This page documents behavior statements and attribute statements in Tractor.

## Behavior Statements

Tractor has the following behavior statements:

### Variable Statements:

```
prepFrame $name <$type> [$attrs] = <$initItem>
```

```
prepFixed $name <$type> [$attrs] = <$initItem>
```

```
prepDef $name <$type> [$attrs] = <$initItem>
```

Declares a prep-access variable with name identifier `$name`, boundary type `$type`, and initialization item `$initItem`. `<$type>` may be excluded.

```
flowFrame $name <$type> [$attrs] = ($initItem)
```

Declares a flow-access variable with name identifier `$name`, boundary type `$type`, and initialization item `$initItem`. `<$type>` and `= ($initItem)` may be excluded. Flow-access variable statements are not permitted at the top level of each module.

```
anyFrame $name <$type> [$attrs] = <$initItem>
```

```
anyFixed $name <$type> [$attrs] = <$initItem>
```

```
anyDef $name <$type> [$attrs] = <$initItem>
```

Declares an any-access variable with name identifier `$name`, boundary type `$type`, and initialization item `$initItem`. `<$type>` may be excluded.

See the section on variables for more details.

### Expression Statement:

```
($expr)
```

Evaluates expression `$expr` to achieve a side-effect.

### Scope Statement:

```
{$behavior}
```

Evaluates `$behavior` in a nested scope.

### Label Statement:

```
label $name
```

Declares a label with name identifier `$name`.

### Jump Statement:

```
jump <$label>
```

Causes control flow to jump to `$label`. `$label` must be defined in the same function as the `jump` statement.

### If Statement:

```
if ($condition1) {$behavior1} elseIf ($condition2) {$behavior2} else {$behavior3}
```

If `$condition1` is true, then `$behavior1` is evaluated. Otherwise if `$condition2` is true, `$behavior2` is evaluated. If neither `$condition1` nor `$condition2` are true, then `$behavior3` is evaluated. `elseIf ($condition2) {$behavior2}` may be excluded or repeated any number of times. `else {$behavior3}` may be excluded.

### While Statement:

```
while ($condition) {$behavior}
```

Enters a loop evaluating `$behavior` until `$condition` is false.

### Loop Control Statements:

```
break
```

Stops the current iteration of the parent `while` statement, and exits the loop.

```
continue
```

Stops the current iteration of the parent `while` statement, and jumps to the beginning of the loop again.

### Return Statement:

```
ret ($item)
```

Stops evaluation of the parent invocable, and returns `$item`. If `($item)` is excluded, then the return item is void.

### Halt Statement:

```
halt
```

Stops compilation or runtime of the application.

### Entry Point Statement:

```
entryPoint {$behavior}
```

Declares the entry point of the current application. When the application launches, `$behavior` will run. Each application must define exactly one entry point.

### Import Statements:

```
tractorImport <$path> as $moduleName [$attrs]
```

Imports the Tractor module located at file path `$path` relative to the `src` directory in the current project. If `as $moduleName` is included, the module will be exposed as a prep-access variable with name identifier `$moduleName` in the current module.

```
foreignImport <$path> as $moduleName <$type> [$attrs]
```

Imports the foreign module located at file path `$path` relative to the `foreign` directory in the current project. If `as $moduleName` is included, the module will be exposed as a prep-access variable with name identifier `$moduleName` in the current module. If `<$type>` is included, the type of the module will be `$type`.

## Attribute Statements

Tractor has the following attribute statements:

### Element Type Statement:

```
elemT <$type>
```

Valid contexts:

* `ptr` special
* `array` special

Asserts that the type of each element in the parent value conforms to `$type`.

### Length Statements:

```
len <$len>
```

Valid contexts:

* `array` special
* `str` special

Asserts that the number of elements in the parent sequence value is `$len`.

```
len ($len)
```

Valid contexts:

* `arrayT` special
* `strT` special

Asserts that the number of elements in the parent sequence type is `$len`.

### Arguments Statement:

```
args [$args]
```

Valid contexts:

* `invocT`, `flowInvocT`, and `macroT` specials
* `prepMacro` and `prepMacroT` specials
* `flowMacro` and `flowMacroT` specials
* `func` and `funcT` specials

Declares the arguments which the parent invocable may accept.

### Argument Statements:

```
$name <$type> [$attrs]
```

Valid contexts:

* `args` statement in one of the following contexts:
    * `prepMacro` special
    * `flowMacro` special
    * `func` special

Declares an argument with name identifier `$name` and boundary type `$type`.

```
$name ($type) [$attrs]
```

Valid contexts:

* `args` statement in one of the following contexts:
    * `invocT`, `flowInvocT`, or `macroT` special
    * `prepMacroT` special
    * `flowMacroT` special
    * `funcT` special

Declares an argument with name identifier `$name` and boundary type `$type`.

See the section on arguments for more details.

### Return Type Statements:

```
retT <$type>
```

Valid contexts:

* `prepMacro` special
* `flowMacro` special
* `func` special

Asserts that the parent invocable value returns an item whose type conforms to `$type`. The default return type is `voidT` when `retT` is excluded.

```
retT ($type)
```

Valid contexts:

* `invocT`, `flowInvocT`, and `macroT` special
* `prepMacroT` special
* `flowMacroT` special
* `funcT` special

Asserts that the parent invocable type returns an item whose type conforms to `$type`. The default return type is `itemT` when `retT` is excluded.

### Suit Type Statements:

```
suitT <$suitType>
```

Valid contexts:

* `prepMacro` special
* `flowMacro` special
* `func` special

Asserts that the time suit of the parent invocable value and its body is `$suitType`. `$suitType` must be `compSuitT`, `runSuitT`, or `anySuitT`.

```
suitT ($suitType)
```

Valid contexts:

* `invocT`, `flowInvocT`, and `macroT` special
* `prepMacroT` special
* `flowMacroT` special
* `funcT` special

Asserts that the time suit of the parent invocable type is `$suitType`. `$suitType` must be `compSuitT`, `runSuitT`, or `anySuitT`.

### Suit Shorthand Statements:

Valid contexts:

* `invocT`, `flowInvocT`, and `macroT` specials
* `prepMacro` and `prepMacroT` specials
* `flowMacro` and `flowMacroT` specials
* `func` and `funcT` specials

```
compSuit
```

Equivalent to `suitT <compSuitT>`.

```
runSuit
```

Equivalent to `suitT <runSuitT>`.

```
anySuit
```

Equivalent to `suitT <anySuitT>`.

### Fields Statements:

Valid contexts:

* `struct` and `structT` specials
* `unionT` special

```
fields [$fields]
```

Declares fields which are stored in the parent data structure.

```
phantomFields [$fields]
```

Declares phantom fields which are associated with the parent data structure. Phantom fields do not occupy any memory in the parent data structure. The types of phantom fields are accessible, but phantom fields store no content.

### Field Statements:

```
$name <$type> = ($initItem)
```

Valid contexts:

* `fields` statement in `struct` special

Declares a field with name identifier `$name`, boundary type `$type`, and initialization item `$initItem`. `<$type>` may be excluded.

```
<$name> <$type> = ($initItem)
```

Valid contexts:

* `fields` statement in `struct` special

Declares a field with name string `$name`, boundary type `$type`, and initialization item `$initItem`. `<$type>` may be excluded.

```
$name ($type)
```

Valid contexts:

* `fields` and `phantomFields` statements in one of the following contexts:
    * `structT` special
    * `unionT` special

Declares a field with name identifier `$name` and boundary type `$type`.

```
($name) ($type)
```

Valid contexts:

* `fields` and `phantomFields` statements in one of the following contexts:
    * `structT` special
    * `unionT` special

Declares a field with name string `$name` and boundary type `$type`.

### Phantom Field Statements:

Valid contexts:

* `phantomFields` statement in `struct` special

```
$name <$type>
```

Declares a field with name identifier `$name` and boundary type `$type`.

```
<$name> <$type>
```

Declares a field with name string `$name` and boundary type `$type`.

### Soft Statement:

```
soft
```

Valid contexts:

* `structT` special
* `unionT` special

Asserts that the parent type contains an unknown number of fields. Even if the type defines particular fields, the type may contain additional fields which are not specified.

### Pack Statements:

```
pack <$alignment>
```

Valid contexts:

* `struct` special

Asserts that the alignment of fields in the parent struct value is `$alignment`.

```
pack ($alignment)
```

Valid contexts:

* `structT` special

Asserts that the alignment of fields in the parent struct type is `$alignment`.

### Export Statement:

```
export
```

Valid contexts:

* Variable statement at top level of module
* `tractorImport` statement
* `foreignImport` statement
* Variable statement in `vars` statement

Asserts that the parent variable may be imported by other modules from the current module.

### Variables Statement:

```
vars [$importVars]
```

Valid contexts:

* `tractorImport` statement
* `foreignImport` statement

Declares the variables to import from the external module.

### Import Variable Statements:

```
var $name1 as $name2 <$type> [$attrs]
```

Valid contexts:

* `vars` statement in `tractorImport` statement

Declares that the variable with name identifier `$name1` in the external module will be exposed with name identifier `$name2` in the current module, and has a type which conforms to `$type`. If `as $name2` is excluded, then the variable will use name identifier `$name1` in the current module. `<$type>` may be excluded.

```
prepFrame $name1 as $name2 <$type> [$attrs]
```

```
prepFixed $name1 as $name2 <$type> [$attrs]
```

```
prepDef $name1 as $name2 <$type> [$attrs]
```

```
flowFrame $name1 as $name2 <$type> [$attrs]
```

```
anyFrame $name1 as $name2 <$type> [$attrs]
```

```
anyFixed $name1 as $name2 <$type> [$attrs]
```

```
anyDef $name1 as $name2 <$type> [$attrs]
```

Valid contexts:

* `vars` statement

Declares an imported variable with the given phase-access and storage type, and otherwise uses the same rules as described above.

### Exports Statement:

```
exports [$exportVars]
```

* `moduleT` special

Declares the variables which are exported from the parent module type.

### Export Variable Statements:

```
prepFrame $name <$type>
```

```
prepFixed $name <$type>
```

```
prepDef $name <$type>
```

```
flowFrame $name <$type>
```

```
anyFrame $name <$type>
```

```
anyFixed $name <$type>
```

```
anyDef $name <$type>
```

Valid contexts:

* `exports` statement

Declares an exported variable with name identifier `$name`, boundary type `$type`, and the given phase-access and storage type.



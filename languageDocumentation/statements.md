
# Statements

This page documents behavior statements and attribute statements in Tractor.

## Behavior Statements

Tractor has the following behavior statements:

### Variable Declaration Statements:

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

Stops evaluation of the parent invocable, and returns `$item`. If `($item)` is excluded, then the return item is `void`.

### Entry Point Statement:

```
entryPoint {$behavior}
```

Declares the entry point of the current application. When the application launches, `$behavior` will run. Each application must define exactly one entry point.

### Assert Statement:

```
assert <$condition>
```

Verifies whether `$condition` is true. If `$condition` is false, compilation will abort with an error message.

### Tractor Import Statements:

```
tractorPath <$path> as $moduleName [$attrs]
```

Imports the Tractor module located at file path `$path` relative to the `src` directory in the current project. If `as $moduleName` is included, the module will be exposed as a prep-access variable with name identifier `$moduleName` in the current module.

```
tractorBuiltIn <$name> as $moduleName [$attrs]
```

Imports the built-in Tractor module with name string `$name`, and otherwise uses the same rules as described above.

### Foreign Import Statements:

```
foreignPath <$path> as $moduleName <$type> [$attrs]
```

Imports the foreign module located at file path `$path` relative to the `foreign` directory in the current project. If `as $moduleName` is included, the module will be exposed as a prep-access variable with name identifier `$moduleName` in the current module. If `<$type>` is included, the type of the module will be `$type`.

```
foreignBuiltIn <$name> as $moduleName <$type> [$attrs]
```

Imports the built-in foreign module with name string `$name`, and otherwise uses the same rules as described above.

## Attribute Statements

TODO: Write this section.



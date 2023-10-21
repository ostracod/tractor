
# Operators and Specials

This page documents operators and specials in Tractor.

## Operators

Supose that `$item1` and `$item2` are expressions with type `intT | ptrT`. The type of the following expressions is also `intT | ptrT`:

* `$item1 + $item2` = Sum of `$item1` and `$item2`
* `$item1 - $item2` = Difference between `$item1` and `$item2`

Supose that `$int1` and `$int2` are expressions with type `intT`. The type of the following expressions is also `intT`:

* `-$int1` = Negation of `$int1`
* `$int1 * $int2` = Product of `$int1` and `$int2`
* `$int1 / $int2` = Quotient when dividing `$int1` by `$int2`
* `$int1 % $int2` = Remainder when dividing `$int1` by `$int2`
* `~$int1` = Bitwise NOT of `$int1`
* `$int1 | $int2` = Bitwise OR of `$int1` and `$int2`
* `$int1 & $int2` = Bitwise AND of `$int1` and `$int2`
* `$int1 ^ $int2` = Bitwise XOR of `$int1` and `$int2`
* `$int1 #sl $int2` = `$int1` shifted left by `$int2` bits
* `$int1 #sr $int2` = `$int1` shifted right by `$int2` bits

Supose that `$item1` and `$item2` are expressions with type `intT | ptrT`. The type of the following expressions is `boolT`:

* `$item1 #lt $item2` = Whether `$item1` is less than `$item2`
* `$item1 #gt $item2` = Whether `$item1` is greater than `$item2`
* `$item1 #lte $item2` = Whether `$item1` is less than or equal to `$item2`
* `$item1 #gte $item2` = Whether `$item1` is greater than or equal to `$item2`

Tractor has the following equality operators:

* `$item1 #eq $item2` = Whether `$item1` is equal to `$item2`
    * `$item1` and `$item2` are expressions with type `itemT`.
    * The type of `$item1 #eq $item2` is `boolT`.
* `$item1 #neq $item2` = Whether `$item1` is not equal to `$item2`
    * `$item1` and `$item2` are expressions with type `itemT`.
    * The type of `$item1 #neq $item2` is `boolT`.

Supose that `$bool1` and `$bool2` are expressions with type `boolT`. The type of the following expressions is also `boolT`:

* `!$bool1` = Logical NOT of `$bool1`
* `$bool1 || $bool2` = Logical OR of `$bool1` and `$bool2`
* `$bool1 && $bool2` = Logical AND of `$bool1` and `$bool2`
* `$bool1 ^^ $bool2` = Logical XOR of `$bool1` and `$bool2`

Suppose that `$ptr` is an expression with type `ptrT($elemType)`. The type of the expression `@$ptr` is `$elemType`, and returns the item referenced by the pointer.

Tractor has the following member access operators:

* `$array@$index` = Element of `$array` at `$index`
    * `$array` is an expression with type `arrayT($elemType)`.
    * `$index` is an expression with type `intT`.
    * The type of `$array@$index` is `$elemType`.
* `$item.$identifier` = Field of `$item` with name `$identifier`
    * `$item` is an expression with type `structT | unionT`.
    * `$identifier` is an identifier.
    * The type of `$item.$identifier` is the type of the field.
* `$item.<$name>` = Field of `$item` with name `$name`
    * `$item` is an expression with type `structT | unionT`.
    * `$name` is an expression with type `ptrT(strT)`.
    * The type of `$item.<$name>` is the type of the field.
* `<$module>.$identifier` = Variable in `$module` with name `$identifier`
    * `$module` is an expression with type `moduleT`.
    * `$identifier` is an identifier.
    * The type of `<$module>.$identifier` is the type of the variable.

Tractor has the following type operators:

* `$item:<$type>` = Cast of `$item` to `$type`
    * `$item` is an expression with type `itemT`.
    * `$type` is an expression with type `typeT`.
    * The type of `$item:<$type>` is `$type`.
* `$item::<$type>` = Force cast of `$item` to `$type`
    * `$item` is an expression with type `itemT`.
    * `$type` is an expression with type `typeT`.
    * The type of `$item::<$type>` is `$type`.
* `~$storageType` = Complement of `$storageType`
    * `$storageType` is an expression which returns a storage type.
    * `~$storageType` also returns a storage type.
* `$type1 | $type2` = Union of `$type1` and `$type2`
    * `$type1` and `$type2` are expressions with type `typeT`.
    * The type of `$type1 | $type2` is also `typeT`.
* `$type1 & $type2` = Intersection of `$type1` and `$type2`
    * `$type1` and `$type2` are expressions with type `typeT`.
    * The type of `$type1 & $type2` is also `typeT`.

The expression `$ref = $item` assigns the return item of expression `$item` to reference `$ref`. The type of `$item` must conform to the type of `$ref`. `=` may be combined with various binary operators to assign the result of an operation between `$ref` and `$expr`. For example, `$ref += $item` assigns `$ref + $item` to `$ref`. The list of all composite assignment operators is below:

* `+=`, `-=`, `*=`, `/=`, and `%=` perform assignment with arithmetic operation.
* `|=`, `&=`, and `^=` perform assignment with bitwise operation.
* `||=`, `&&=`, and `^^=` perform assignment with logical operation.
* `#sl=` and `#sr=` perform assignment with bitshift.

The return type of all assignment operators is `voidT`.

## Specials

Tractor has the following specials:

### Pointer Specials:

```
ptr ($ref)
```

Creates a pointer value which references `$ref`.

```
ptrT ($elemType)
```

Creates a pointer type whose element type is `$elemType`. If `$elemType` is excluded, the element type will be `itemT`.

### Array Specials:

```
array [$attrs] ($elems)
```

Creates an array value whose elements are `$elems`.

```
arrayT [$attrs] ($elemTypes)
```

Creates an array type whose element types are `$elemTypes`. If `($elemTypes)` is excluded, then element types are determined by the `elemType` attribute statement.

### String Type Special:

```
strT [$attrs]
```

Creates a string type whose length may be described by `$attrs`.

### Struct Specials:

```
struct [$attrs]
```

Creates a struct value whose fields are described by `$attrs`.

```
structT [$attrs]
```

Creates a struct type whose field types are described by `$attrs`.

### Union Type Special:

```
unionT [$attrs]
```

Creates a union type whose field types are described by `$attrs`.

### Invocable Type Specials:

```
invocT [$attrs]
```

Creates an invocable type whose signature is described by `$attrs`.

```
flowInvocT [$attrs]
```

Creates a flow-invocable type whose signature is described by `$attrs`.

```
macroT [$attrs]
```

Creates a macro type whose signature is described by `$attrs`.

### Prep-Macro Specials:

```
prepMacro [$attrs] {$body}
```

Creates a prep-macro value whose signature is described by `$attrs`, and whose behavior is determined by `$body`.

```
prepMacro [$attrs] ($expr)
```

Creates a prep-macro value whose signature is described by `$attrs`, and whose return item is determined by `$expr`.

```
prepMacroT [$attrs]
```

Creates a prep-macro type whose signature is described by `$attrs`.

### Flow-Macro Specials:

```
flowMacro [$attrs] {$body}
```

Creates a flow-macro value whose signature is described by `$attrs`, and whose behavior is determined by `$body`.

```
flowMacro [$attrs] ($expr)
```

Creates a flow-macro value whose signature is described by `$attrs`, and whose return item is determined by `$expr`.

```
flowMacroT [$attrs]
```

Creates a flow-macro type whose signature is described by `$attrs`.

### Function Specials:

```
func [$attrs] {$body}
```

Creates a function value whose signature is described by `$attrs`, and whose behavior is determined by `$body`.

```
func [$attrs] ($expr)
```

Creates a function value whose signature is described by `$attrs`, and whose return item is determined by `$expr`.

```
funcT [$attrs]
```

Creates a function type whose signature is described by `$attrs`.

### Module Type Special:

```
moduleT [$attrs]
```

Creates a module type whose variables are described by `$attrs`.

### Literal Type Special:

```
literalT ($item)
```

Creates a type which only contains items equal to `$item`. For example, `literalT(16)` is the type of all integers which are equal to 16.




# Syntax

This page documents expression syntax and statement syntax in Tractor.

## Expression Syntax

Tractor supports the following types of expressions:

* Value literal
* Variable identifier
* Unary and binary operations
* Function, macro, and special invocations
* Expression sequence which returns one item

Value literals include the following:

* Decimal integer
* Hexadecimal integer with the prefix `0x`
* String enclosed by quotation marks (`"`)
* Character enclosed by apostrophes (`'`)

Value literals have the following constraint types:

* The constraint type of integer literals is `uIntT8`, `uInt16`, `uInt32`, or `uInt64` depending on the size of the integer.
* The constraint type of character literals is `uInt8T`.
* The constraint type of string literals is `strT`.

To create types which refer to specific values, use the `literalT` special. See the section on specials for more details.

Identifiers and keywords may contain the following characters:

* Uppercase and lowercase letters
* Underscore (`_`)
* Dollar sign (`$`)
* Decimal digits

Note that identifiers may not begin with a decimal digit. Also note that throughout this documentation, an identifier beginning with a dollar sign denotes a placeholder. A dollar sign does not otherwise hold special significance.

## Expression Sequences

An "expression sequence" consists of expressions enclosed by bracket delimiters. Expressions in an expression sequence are separated by commas or newlines. Every expression sequence returns an item sequence.

The bracket delimiters used in an expression sequence determine the following properties:

* The phase grade of expressions in the expression sequence
* The return items of the expression sequence

Tractor includes the following expression sequence bracket delimeters:

* `(` and `)` enclose flow-grade expressions, and return the items returned by the expressions.
* `<` and `>` enclose prep-grade expressions, and return the items returned by the expressions.
* `<?` and `>` enclose never-grade expressions, and return the constraint types of the expressions. The return items are resolved during prep-phase of the parent context.

Expression sequences provide arguments to functions, macros, specials, and statements. An expression sequence which returns one item can serve as an expression, and may be nested in other expressions.

With regard to statement arguments and expression operands, bracket delimiters follow these compatibility rules:

* `<` and `>` may be used in any context which accepts `(` and `)`.
* `<?` and `>` may be used in any context which accepts `<` and `>`.

For example, given that the structure of a return statement is defined to be `return ($item)`, the form `return <$item>` is also valid.

## Statement Syntax

Tractor has two types of statements:

* "Behavior statements" declare variables, perform operations, and determine control flow.
* "Attribute statements" define properties of data structures.

A "statement sequence" consists of statements enclosed by bracket delimiters. Statements in a statement sequence are separated by commas or newlines. The bracket delimiters determine the type of statements which the statement sequence contains:

* `{` and `}` enclose behavior statements.
* `[` and `]` enclose attribute statements.

Each statement contains one or more of the following components:

* Keyword
* Identifier
* Expression sequence
* Statement sequence
* Equal sign

Note that attribute statement sequences are always optional in statements, and may be excluded when empty.

A comment with the prefix `--` may be placed at the end of any line.

## Invocation Syntax

Invocation of an invocable item accepts an argument item sequence, and returns an item. Invocation syntax depends on the type of invocable item:

* Prep-macros are invoked with the syntax `<$prepMacro><$args>`.
* Flow-invocables are invoked with the syntax `<$flowInvoc>($args)`.
* Functions are invoked with the syntax `$func($args)`.

A "special" is an identifier which may be invoked in a similar fashion to a function. A special accepts both expression sequences and statement sequences which are placed immediately after the special. For example, `$special [$attrs] ($exprs)` invokes `$special` with both `[$attrs]` and `($exprs)`.

Unlike functions, specials are always invoked when embedded in an expression, even when the special does not precede an expression sequence or statement sequence. For example, `($special)` invokes `$special`. As a result, specials cannot be passed as items. Instead, only the items returned by their invocation can be passed.

Note that attribute statement sequences are always optional in specials, and may be excluded when empty.



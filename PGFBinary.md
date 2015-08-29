# Basic Types #

  * Int8
  * Int16
  * Int
  * String
  * Float
  * List

# PGF #

| **type**       | **description**                 |
|:---------------|:--------------------------------|
| `Int16`        | major PGF version               |
| `Int16`        | minor PGF version               |
| `[`[Flag](#Flag.md)`]`     | flags for the whole grammar     |
| [Abstract](#Abstract.md)   | abstract syntax                 |
| `[`[Concrete](#Concrete.md)`]` | list of all concrete syntaxes   |

# Flag #

| **type**     | **description**                 |
|:-------------|:--------------------------------|
| `String`     | flag name                       |
| [Literal](#Literal.md)  | flag value                      |

# Abstract #

| **type**     | **description**                 |
|:-------------|:--------------------------------|
| `String`     | abstract syntax name            |
| `[`[Flag](#Flag.md)`]`   | flags for the abstract syntax   |
| `[`[AbsFun](#AbsFun.md)`]` | list of abstract functions      |
| `[`[AbsCat](#AbsCat.md)`]` | list of abstract categories     |

# AbsFun #

| **type**       | **description**                            |
|:---------------|:-------------------------------------------|
| `String`       | function name                              |
| [Type](#Type.md) | function's type signature                  |
| `Int`          | function's arrity                          |
| `Int8`         | if this is 0 the function is constructor and doesn't have equations |
| `[`[Equation](#Equation.md)`]` | definitional equations for this function if this is not a constructor |
| `Float`        | Probability                                |

# AbsCat #

| **type**       | **description**                        |
|:---------------|:---------------------------------------|
| `String`       | category name                          |
| `[`[Hypo](#Hypo.md)`]`     | list of hypotheses for this category   |
| `[`[CatFun](#CatFun.md)`]`   | list of functions for this category in source-code order |

# CatFun #

| **type**       | **description**                        |
|:---------------|:---------------------------------------|
| `String`       | Function name                          |
| `Float`        | Probability                            |

# Type #

| **type**         | **description**                      |
|:-----------------|:-------------------------------------|
| `[`[Hypo](#Hypo.md)`]`       | list of hypotheses for this type     |
| `String`         | the name of the return category      |
| `[`[Expression](#Expression.md)`]` | arguments for the return category    |

# Hypo #

| **type**                | **description**                                  |
|:------------------------|:-------------------------------------------------|
| [BindType](#BindType.md) | binding type                                     |
| `String`                | variable name or `'_'` if no variable is bound   |
| [Type](#Type.md)          | the type of the variable                         |

# Equation #

| **type**                    | **description**        |
|:----------------------------|:-----------------------|
| `[`[Pattern](#Pattern.md)`]`  | sequence of patterns   |
| [Expression](#Expression.md)  | expression             |

# Pattern #

| **type**      | **description**  |
|:--------------|:-----------------|
| `Int8`        | pattern tag      |

1. tag=0 - application (`c p1 p2 ... pn`)
| **type**      | **description**                          |
|:--------------|:-----------------------------------------|
| `String`      | abstract function name                   |
| `[`[Pattern](#Pattern.md)`]` | list of sub patterns for the arguments   |

2. tag=1 - variable
| **type**      | **description** |
|:--------------|:----------------|
| `String`      | variable name   |

3. tag=2 - variable@pattern
| **type**      | **description** |
|:--------------|:----------------|
| `String`      | variable name   |
| [Pattern](#Pattern.md)   | sub-pattern     |

4. tag=3 - wildcard (`_`). No more fields

5. tag=4 - literal (`"aa"`, `1`, `3.14`)
| **type**      | **description** |
|:--------------|:----------------|
| [Litteral](#Literal.md)   | literal value   |

6. tag=5 - pattern matching over implicit argument ({p})
| **type**               | **description** |
|:-----------------------|:----------------|
| [Pattern](#Pattern.md)   | variable name   |

7. tag=6 - inaccessible pattern
| **type**            | **description** |
|:--------------------|:----------------|
| [Expr](#Expr.md)      | the content of the inaccessible pattern |

# Expression #

| **type**     | **description**  |
|:-------------|:-----------------|
| `Int8`       | expression tag   |

1. tag=0 - lambda abstraction (`\x -> ...`)
| **type**       | **description**                      |
|:---------------|:-------------------------------------|
| [BindType](#BindType.md)   | expression tag                       |
| `String`       | variable name                        |
| [Expression](#Expression.md) | the body of the lambda abstraction   |

2. tag=1 - application (`f x`)
| **type**       | **description**                           |
|:---------------|:------------------------------------------|
| [Expression](#Expression.md) | the left expression in the application    |
| [Expression](#Expression.md) | the right expression in the application   |

3. tag=2 - literal (`"aa"`, `1`, `3.14`)
| **type**    | **description**             |
|:------------|:----------------------------|
| [Literal](#Literal.md) | the value of the literal    |

4. tag=3 - meta variable (`?0`,`?1`)
| **type** | **description**               |
|:---------|:------------------------------|
| `Int`    | the id of the meta variable   |

5. tag=4 - abstract function name (`PredVP`, `IndefArt`)
| **type**   | **description**          |
|:-----------|:-------------------------|
| `String`   | abstract function name   |

6. tag=5 - variable
| **type** | **description**                       |
|:---------|:--------------------------------------|
| `Int`    | the de Bruijn index of the variable   |

7. tag=6 - expression with type annotation (`<e : t>`)
| **type**       | **description**              |
|:---------------|:-----------------------------|
| [Expression](#Expression.md) | the annotated expression     |
| [Type](#Type.md)       | the type of the expression   |

8. tag=7 - implicit argument (`{e}`)
| **type**       | **description**                   |
|:---------------|:----------------------------------|
| [Expression](#Expression.md) | the expression for the argument   |

# BindType #

| **type** | **description** |
|:---------|:----------------|
| `Int8`   | tag             |

The tag has two possible values:
  * tag=0, explicit argument
  * tag=1, implicit argument

# Concrete #

| **type**         | **description**                                           |
|:-----------------|:----------------------------------------------------------|
| `String`         | concrete syntax name                                      |
| `[`[Flag](#Flag.md)`]`       | flags for the concrete syntax                             |
| `[`[PrintName](#PrintName.md)`]`  | list of all print names defined in this concrete syntax   |
| `[`[Sequence](#Sequence.md)`]`   | list of sequences                                         |
| `[`[CncFun](#CncFun.md)`]`     | list of concrete functions                                |
| `[`[ProductionSet](#ProductionSet.md)`]` | list of production sets                                     |
| `[`[CncCat](#CncCat.md)`]`     | list of concrete categories                               |
| `Int`            | total number of forest ids allocated for the grammar      |

# PrintName #

| **type**   | **description**                                                          |
|:-----------|:-------------------------------------------------------------------------|
| `String`   | name of abstract function or category for which the print name applies   |
| `String`   | the print name itself                                                    |

# Sequence #

| **type**     | **description**                                |
|:-------------|:-----------------------------------------------|
| `[`[Symbol](#Symbol.md)`]` | list of symbols that constitute the sequence   |

# Symbol #

Symbols are the terminals and non-terminals of the grammar. There are different kind os symbols (terminals, non terminals, variables...). the first byte is a tag indicating the kind of symbol.

## Category ##

This is the main non-terminal. It is represented by two integers : the  argument index is the index of this category in the functions arguments and the constituent index is the index of the constituent of the category which is used in this position.

| **type** | **description**                              |
|:---------|:---------------------------------------------|
| `Int8`   | Tag which says what kind of symbol this is. **tag=0** |
| `Int`    | argument index                               |
| `Int`    | constituent index                            |

## SymLit ##

TODO

| **type** | **description**                              |
|:---------|:---------------------------------------------|
| `Int8`   | Tag which says what kind of symbol this is. **tag=1** |
| `Int`    |                                              |
| `Int`    |                                              |

## Variables ##

TODO

| **type** | **description**                              |
|:---------|:---------------------------------------------|
| `Int8`   | Tag which says what kind of symbol this is. **tag=2** |
| `Int`    |                                              |
| `Int`    |                                              |

## Tokens ##


This is the main terminal symbol. it represents a list of tokens.
The reason it is a list and not a single token is to be consistent with the other kind of terminal, the `pre` construct. The possibility of having multiple token in a single symbol forces the parser to keeps a trie of future agendas.

| **type**     | **description**      |
|:-------------|:---------------------|
| `Int8`       | Tag which says what kind of symbol this is. **tag=3** |
| `[String]`   | sequence of tokens   |

## Token with alternatives ##

This is the second kind of terminal symbol. It represents the `pre` construct in the gf source language. The first list of strings is the default list of tokens (used if none of the prefixes matches.) The [Alternatives](#Alternative.md) are the others possibilitiesm each associated with a prefix that should match the following tokens.

| **type**          | **description**            |
|:------------------|:---------------------------|
| `Int8`            | Tag which says what kind of symbol this is. **tag=4** |
| `[String]`        | sequence of tokens         |
| `[`[Alternative](#Alternative.md)`]` | sequence of alternatives   |

# Alternative #

Alternatives are used in the `pre` construct in the gf language. For example, if the following construct is present in the source :

`pre { beau ; bel / ami}`

then the pair `bel / ami` will be one alternative, represented by the pair `(["bel"],["ami"])`

| **type**        | **description**                                |
|:----------------|:-----------------------------------------------|
| `[String]`      | The tokens to use if the prefix matches        |
| `[String]`      | The prefix matched with the following tokens   |

# CncFun #

| **type**        | **description**                                   |
|:----------------|:--------------------------------------------------|
| `String`        | the name of the corresponding abstract function   |
| `[Int]`         | list of indices into the sequences array          |

# ProductionSet #

Every production set groups all productions that have one and the same result category.

| **type**         | **description**                         |
|:-----------------|:----------------------------------------|
| `Int`            | the forest id of the result category    |
| `[`[Production](#Production.md)`]` | list of productions for this category   |

# Production #

There are two kinds of productions:
  * application of some concrete function to a list of arguments:
> > `A -> f[B,C]`
> > where `f` is the function, `B` and `C` are the arguments and A is
> > the result category.

  * coercion from one concrete category to another:
> > `A -> _[B]`
> > where `B` is the initial category and `A` is the final i.e. the
> > result category.

Every production starts with a tag (`Int8`) which says what kind of production this is

1. if tag=0 then the production is of application kind and there are two more fields:
| **type**              | **description**  |
|:----------------------|:-----------------|
| `Int`                 | the index of the function in the functions array |
| `[`[PArg](#!PArg.md)`]` | list of arguments.   |

2. if tag=1 then the production is coercion and there is one more field:
| **type**   | **description**                     |
|:-----------|:------------------------------------|
| `Int`      | forest id of the initial category   |

# PArg #

A PArg is a productions argument.
| **type**  | **description**  |
|:----------|:-----------------|
| `[Int]`   |                  |
| `Int`     | Category         |

# CncCat #

| **type**        | **description**                                   |
|:----------------|:--------------------------------------------------|
| `String`        | the name of the corresponding abstract category   |
| `Int`           | the first corresponding forest id                 |
| `Int`           | the last corresponding forest id                  |
| `[String]`      | list of label names                               |

# Literal #

| **type** | **description** |
|:---------|:----------------|
| `Int8`   | literal type    |

1. type=0 - string
| **type**   | **description** |
|:-----------|:----------------|
| `String`   | value           |

2. type=1 - integer
| **type** | **description** |
|:---------|:----------------|
| `Int`    | value           |

3. type=2 - float
| **type**  | **description** |
|:----------|:----------------|
| `Float`   | value           |
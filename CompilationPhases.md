# Dependency Analyses #

Traverses the dependencies of the top modules and determines which modules and in which order should be compiled. Circular dependences are not allowed and are reported as error.

# Parsing #

# Renaming #

  * Constructor `Cn` in the abstract syntax is replaced with `Q`/`QC` for global functions and with `Vr` for local variables. The local variables are represented with de Brujin indices. The `Cn` is not used further.

  * Ambiguous references are reported as warnings. The compiler picks the first choice and tries to compile with it. (Fishy?)

# Grammar Checking #

In this phase we check the grammar for consistency. This includes among other things type checking.

The first thing is to check whether:

  * for every _lin_ there is a corresponding _fun_ or _data_ definition. The same with _cat_ and _lincat_. Linearization definitions without corresponding match in the abstract syntax are ignored. A warning is printed.

  * for every abstract syntax category of function there is a corresponding in the concrete syntax. If there is missing _lincat_ then the default linearization type `{s:Str}` is inserted. If there is missing _lin_ and the target declaration category has linearization type `{}` then a default
```
\_,_,..._ -> <>
```
> linearization function is included. In all other cases a warning for missing linearization function is printed.

  * for every lin declaration, a type signature for the concrete syntax is generated which is based on the abstract type of the same function

After that all declarations are type checked. The type checking is done in dependency order to avoid using invalid declarations. There are two different type checkers for the abstract and the concrete syntax because they have different requirements.


# Partial Evaluation #

  * missing lindef declarations are automatically generated using the default strategy. (Why here?)

  * Uses call-by-name with nondeterminism. The nondeterminism comes from the usage of variants. We use the same semantics as in [Curry](http://curry-language.org/).

# PMCFG generation #
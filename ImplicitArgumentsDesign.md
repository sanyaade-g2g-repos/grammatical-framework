# Introduction #

The goal is to have implicit arguments in the style of Agda. The current design is to use curly braces around the variable names when they are declared:
```
fun inhz : ({c1,c2} : Class) -> SubClass c1 c2 -> Inherits c1 c2 ;
    inhs : ({c1,c2,c3} : Class) -> SubClass c1 c2 -> Inherits c2 c3 -> Inherits c1 c3 ;
```
In this case c1,c2 and c3 will be implicit arguments and they don't have to be supplied when functions inhz and inhs are called. If there are this two constants:
```
fun AB : SubClass A B ;
    BC : SubClass B C ;
```
then this are valid expressions:
```
<inhz AB : Inherits A B>
<inhs AB (inhz BC) : Inherits A C>
```

If you still want to give an explicit value for an implicit argument then you have to surround the value in curly braces and place it in the right place in the expression. For example, here:
```
<inhz {A} {B} AB : Inherits A B>
```
A and B are explicit values for the first two arguments of `inhz`.
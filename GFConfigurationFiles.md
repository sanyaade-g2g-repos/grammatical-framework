The idea is to be able to annotate every function or category in the abstract syntax of a grammar with some extra information. For example we would like to be able to specify the probability of some syntactic construction.

This is the current design for the syntax:
```
PredVP {
  printname = "predication";
  prob = 0.5;
  deps = arg head
}

UseV {
  printname = "use verb";
  prob = 0.7;
  deps = head
}

S {
  printname = "Sentence"
}
```

The annotations are stored in a separate file and could be loaded by the compiler or by the interpreter. When the compiler is given an annotation file then it collects the annotations and stores them in the compiled PGF file. When more than one configuration files are specified then the annotations from all of them are merged. For example we could have one configuration file with probabilities:
```
PredVP {
  prob = 0.5;
}
```
and another with dependency labels:
```
PredVP {
  deps = arg head
}
```
If both files are supplied then the probability for PredVP is taken from the first and its dependency labels are taken from the second.

The advantages of using separate file for annotations are two:
  * we don't have to define a new keyword in the language every time when we find that we need new type of annotation. If the same was implemented by introducing new judgements type then we would have to define new keyword for every type of annotation. In the current design 'printname', 'prob' and 'deps' are only labels and not keywords.
  * we could have different configuration files for one and the same grammar. For example probabilities retrieved from different tree banks.

The values in the annotations should be one of the following types: String, Int, Float or Identifier. The annotation could be applied to the function or the category itself or to its arguments. In the example above the annotation 'deps' is annotation which supplies one identifier for each argument of function 'PredVP':
```
fun PredVP : NP -> VP -> S ;
```

The different kinds of annotations, their default values and the corresponding types should be specified in a single module in the compiler. This will make it easier to add more annotations.

The annotation values should be stored in two different structures, one for functions and one for categories:
```
data FunAnnotation
  = FunAnnotation
     { funPrintName :: String
     , funProb      :: Double
     , funDeps      :: [CId]
     }

data CatAnnotation
  = CatAnnotation
     { catPrintName :: String
     }
```
There will be one instance of this structures per function/category. It will be accessible via the funs/cats fields in the structure Abstr in PGF.
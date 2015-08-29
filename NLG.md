The dependent types in GF are powerful tool that can be used for many purposes. One of them is for natural language generation from logical formula. The grammar:
```
examples/nlg/NLGEng.gf
```
is an example of how this can be done.

The example contains two modules Logic and NLG. The first one defines few predicates and logical operators, and the second one defines a simple grammar with semantics in first-order logic. The general principle is that every syntactic category has index which is the semantics of the corresponding syntactic construction. For example instead of:
```
cat S
```
we have:
```
cat S Prop
```
where the index of type `Prop` is the semantics of the sentence.

Now we can do several exciting things with the grammar. For example, we can parse a sentence and get immediately its semantics:
```
> p "John loves Mary"
UttS (love john mary) 
     (UseCl PPos (PredVP (UsePN john_PN) 
                         (ComplSlash (SlashV2a love_V2) 
                                     (UsePN mary_PN))))
```
here the first argument of `UttS` is the semantic representation and the second one is the abstract syntax tree as it is defined in the resource grammar. Note that if you try this in the GF shell you will see some extra arguments in the abstract tree surrounded with curly braces. This are implicit arguments that the interpreter infers automatically, and they can be skipped when the trees are written by the user. For example, we can ask the interpreter to find the semantic representation for given abstract syntax tree:
```
> ai (UseCl PPos (PredVP (UsePN john_PN) (ComplSlash (SlashV2a love_V2) (UsePN mary_PN))))
Type:        S (love john mary)
```
Now the interpreter answers that the syntactic category is sentence, i.e. S, and the semantic representation is `(love john mary)`.

Of course we should not forget that when we have quantifiers in the sentence, then we can get scope ambiguities. If you try the famous example "somebody loves everybody", you get two possible abstract trees:
```
> p "somebody loves everybody"
UttS (exists (\v0 -> forall (\v1 -> love v0 v1)))
     (UseCl PPos (PredVP somebody_NP (ComplSlash (SlashV2a love_V2) everybody_NP)))
UttS (forall (\v0 -> exists (\v1 -> love v0 v1)))
     (UseCl PPos (ComplClSlash (SlashVP somebody_NP (SlashV2a love_V2)) everybody_NP))
```

We can go in the opposite direction too, i.e. from semantics to natural language. For example:
```
> gt -cat="S (and (smart john) (smart mary))" -depth=8 | l
John is smart and Mary is smart
John and Mary are smart
```
here we generate all abstract syntax trees with the semantics `(and (smart john) (smart mary))`, and after that every tree is linearized back to string. Since we have both sentence level and NP level coordination we get two possible renderings in English.

The grammar is actually tiny subset of the resource grammar so other experiments are possible as well. Note that the generation is still expensive operation but there is a lot of room for optimizations. Don't forget to add the option -depth=N which limits the proof search to trees with maximal depth N. This improves the search performance a lot.
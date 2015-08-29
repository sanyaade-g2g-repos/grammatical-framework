The GF parser returns some results even if the input sentence cannot be fully parsed or is incomplete. You could try this in the GF shell. For example:
```
Lang> p -bracket ""
(_)
```
```
Lang> p -bracket "the"
(_ the)
```
```
Lang> p -bracket "the red"
(_ the (A red))
```
```
Lang> p -bracket "the red hat"
(Phr (Utt (NP (Det (Quant the)) (CN (AP (A red)) (CN (N hat))))))
```
```
Lang> p -bracket "the red hat is"
(_ (NP (Det (Quant the)) (CN (AP (A red)) (CN (N hat)))) is)
```
```
Lang> p -bracket "the red hat is very"
(_ (NP (Det (Quant the)) (CN (AP (A red)) (CN (N hat)))) (VP (VP is)) (AdA very))
```
From the examples above, only "the red hat" is complete phrase.

Every bracket marks a single phrase in the input string. In general the grammar could be ambiguous and for every string there might many different analyses. However the bracketing API will mark only those phrases which are unambiguous. For example:
```
Lang> p -cat=S -bracket "I saw the man with the telescope"
(S (Cl (NP (Pron I)) (VP (V2 saw)) (VP (Det (Quant the)) (CN (N man)) (Adv (Prep with) (NP (Det (Quant the)) (CN (N telescope)))))))
```

This sentence have three different parse trees with the resource grammar but the parser will produce only one bracketing which marks only those phrases which exists in all possible analyses. If you want to see the brackets for all distinct analyses then try:
```
Lang> p -cat=S "I saw the man with the telescope" | l -bracket -lang=LangEng
```

When the input is not complete then only those phrases which are unambiguous and are complete are marked. If there isn't any top-level bracket then a new artificial phrase with category `_` is added. For example the bracketing for the empty string is `(_)`.

When the input contains syntax error then the returned bracketed string contains only the tokens up to the spot where the error is. Calculating the length of the bracketed string is the easiest way to find the error location.

The bracketed string is actually representation of the parse tree (or the intersection of all trees) for a given string and could be rendered with Graphviz:
```
graphvizBracketedString :: BracketedString -> String
```
Just like it was possible with:
```
graphvizParseTree :: PGF -> Language -> Tree -> String
```
before. The old function is still available for convenience.

Furthermore the bracketed string also keeps information for the abstract syntax tree that produced it. This is especially handy for online translation. The following Haskell code could be used for translation of incomplete sentences:
```
import PGF
import Data.Maybe

main = do
  pgf <- readPGF "Lang.pgf"
  sent <- getLine
  let (_,bs) = parse_ pgf (fromJust $ readLanguage "LangEng") (fromJust $ readType "Phr") sent
  putStrLn (showBracketedString bs)
  putStrLn (translate pgf bs)

translate pgf (Leaf w) = w
translate pgf (Bracket _ _ _ es bss) =
  case es of
    []     -> unwords (map (translate pgf) bss)
    (e:es) -> linearize pgf (fromJust $ readLanguage "LangGer") e   -- we ignore ambiguities
```

Examples:
```
the boy
(Phr (Utt (NP (Det (Quant the)) (CN (N boy)))))
der Junge
```
```
the boy is
(_ (NP (Det (Quant the)) (CN (N boy))) is)
der Junge is
```
```
the boy is very
(_ (NP (Det (Quant the)) (CN (N boy))) (VP (VP is)) (AdA very))
der Junge zu werden sehr
```
```
I am here and
(_ (NP (Pron I)) am (Adv here) and)
ich am hier and
```
```
I am here and there
(Phr (Utt (S (Cl (NP (Pron I)) (VP am) (VP (Comp (Adv (ListAdv (Adv here)) (Conj and) (ListAdv (Adv there)))))))))
ich bin hier und dahin
```
Note that sometimes the translation is mixture of English and German, but it improves when we extend the sentence.

Finally since now we have a way to construct single phrase structure for every string we could easily combine the completion based document authoring with the traditional syntax editor.
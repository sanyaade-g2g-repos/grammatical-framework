## News ##

<wiki:gadget url="http://google-code-feed-gadget.googlecode.com/svn/trunk/gadget.xml" up\_feeds="http://grammatical-framework.blogspot.com/atom.xml" width="500px" height="540px" border="0"/>

## What is GF ##

GF, Grammatical Framework, is a programming language for multilingual grammar applications. It is

  * a **special-purpose language for grammars**, like YACC, Bison, Happy, BNFC, but not restricted to programming languages
  * a **functional language**, like Haskell, Lisp, OCaml, Scheme, SML, but specialized to grammar writing
  * a **natural language processing framework**, like LKB, XLE, Regulus, but based on functional programming and type theory
  * a **categorial grammar formalism**, like ACG, CCG, but different and equipped with different tools
  * a **logical framework**, like Agda, Coq, Isabelle, but equipped with concrete syntax in addition to logic

Don't worry if you don't know most of the references above - but if you do know at least one, it may help you to get a first idea of what GF is.

## Applications ##

GF can be used for building

  * text translators
  * speech translators
  * natural-language interfaces
  * multilingual authoring systems
  * dialogue systems
  * natural language resources

## Availability ##

GF is open-source, licensed under GPL (the program) and LGPL (the libraries). It is available for
  * Linux
  * Mac OS X
  * Windows
  * via compilation to JavaScript, almost any platform that has a web browser

## Projects ##

GF was first created in 1998 at [Xerox Research Centre Europe](http://www.xrce.xerox.com/), Grenoble, in the project Multilingual Document Authoring. At Xerox, it was used for prototypes including a restaurant phrase book, a database query system, a formalization of an alarm system instructions with translations to 5 languages, and an authoring system for medical drug descriptions.

Later projects using GF and involving third parties include, in chronological order,

  * [GF-Alfa](http://www.cs.chalmers.se/~hallgren/Alfa/Tutorial/GFplugin.html): natural language interface to formal proofs
  * [Efficient](http://efficient.citi.tudor.lu/index_noframe.html): authoring tool for business models.
  * [GF-KeY](http://www.key-project.org/): authoring and translation of software specifications
  * [TALK](http://www.talk-project.org): multilingual and multimodal spoken dialogue systems
  * [WebALT](http://webalt.math.helsinki.fi/): multilingual generation of mathematical exercises (commercial project)
  * [SALDO](http://spraakbanken.gu.se/sal/): Swedish morphological dictionary based on rules developed for GF and Functional Morphology

Academically, GF has been used in four PhD theses and resulted in around fifty scientific publications (see GF publication list).


## Programming in GF ##

GF is easy to learn by following the tutorial. You can write your first translator in 15 minutes.

GF has an interactive command interpreter, as well as a batch compiler. Grammars can be compiled to parser and translator code in many different formats. These components can then be embedded in applications written in other programming languages. The formats currently supported are:

  * Haskell
  * Java
  * JavaScript
  * Prolog
  * Speech recognition: HTK/ATK, Nuance, JSGF


The GF programming language is high-level and advanced, featuring
  * static type checking
  * higher-order functions
  * dependent types
  * pattern matching with data constructors and regular expressions
  * module system with multiple inheritance and parametrized modules


## Libraries ##

Libraries are at the heart of modern software engineering. In natural language applications, libraries are a way to cope with thousands of details involved in syntax, lexicon, and inflection. The GF resource grammar library has support for an increasing number of languages, currently including

  * Arabic (partial)
  * Bulgarian
  * Catalan (partial)
  * Danish
  * Dutch
  * English
  * Finnish
  * French
  * German
  * Hindi/Urdu (fragments)
  * Interlingua
  * Italian
  * Norwegian (bokmål)
  * Polish
  * Romanian
  * Russian
  * Spanish
  * Swedish
  * Thai (fragments)

Adding a language to the resource library takes 3 to 9 months - contributions are welcome!


## Development ##

If you want to start hacking on GF then read the DevelopersPage
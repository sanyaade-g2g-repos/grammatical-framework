This page is a list of project ideas for students willing to participate in Google Summer of Code 2010. The list is not final, we could add more ideas or modify some of the existing ones later. We see it as important, that all participants should enjoy the work that they are doing, so we could adjust the project's scope and goal after an interview with the candidates. However the projects should also be useful in the context of GF, so if the student has his/her idea for project, it will be reviewed by the GF developers.

The project ideas are divided into two groups. The first group contains projects which are related to the creation of some reusable language resources while the second group has more traditional software projects i.e. concrete applications or some extensions to the GF compiler.

# Language Engineering Projects #

Although the projects in this section will require programming, in some of the projects, we are not so interested in the code itself but in the output from the program. For example it might be necessary to write some program which converts some raw data to GF grammar. In this case we are interested in the generated GF code and not so much in the software that was used to generate the code. Some of the projects might require some knowledge in computational linguistics but this is not strictly necessary because it could just as well be learned during the project.

## Development of Resource Grammar for New Language ##

One of the most valuable resource in GF is the Resource Grammar Library. This is a library of grammars for currently 16 languages: Bulgarian, Catalan, Danish, Dutch, English, Finnish,
French, German, Interlingua, Italian, Norwegian, Polish, Romanian, Russian, Spanish, Swedish. We always welcome contributions with grammars for new languages. Contrary to the usual, we do not require that the grammar developers for the Resource Grammar Library are experienced linguists. Our experience showed that it is much more valuable if the developer has good programming skills and it is just enough if he/she has good knowledge of the target language.

The candidate should propose a language which hi/she knows well and for which we do not have grammar already. Usually the candidates suggest their native language but this is not a requirement. If the candidate is proficient enough in some second language then this could be an option as well. Some fragments of the grammars for Arabic, Turkish and Thai are already developed, so we welcome candidates willing to complete them.

## Extensions to the Resource Grammar Library ##

There are some aspects of the everyday language which are still not covered by the resource library. For example we often find it useful to be able to express dates and times using the library i.e. how would you say:

The time is ten to three.

in your native language? Unfortunately this is not supported yet. For some language pairs the translation is more than just word-by-word translation and actually it requires some simple math. Another useful extension would be to have library of different kinds of greetings (i.e. hello, hi, good morning, good evening, welcome, etc) in all languages of the resource grammar.

It is desirable to cover as many languages as possible in this project. Candidates that can speak many languages will have an advantage. However one good aspect of this project is that you could find information about the syntax of dates and times in internet or in any textbook for learning the corresponding language. The same applies to the greetings. Part of the work will be to search for materials for the languages that the candidate doesn't know.

Other extensions could be proposed and implemented during the project. For example by using the grammar for dates and times it is possible to implement grammar for arranging meetings which after that could be used for automatic extraction of the meeting schedule from an e-mail.

## Import of Lexicons ##

In linguistics a lexicon is a collection of words in a given language together with some information about the usage of the words. In GF we are mostly interested in morphological lexicons i.e. lexicons that contain information about how a given word is inflected.

The resource grammar library implements the most common syntactic constructions available in the different languages, but it provides very little information about the words used in the language. Basically there is only a small lexicon of only about 300 words which is used mostly for debugging purposes. In this project we aim at importing more lexical information in the library.

There are three kinds of lexicons which we find interesting in GF:
  * large coverage monolingual morphological lexicons - This are lexicons of about 50-80 thousand words which aim at having very good coverage for a single language. Such lexicons are used for example in the spell checkers for MS Words and Open Office. There are already large scale lexicons for three languages - Bulgarian, English and Swedish.

  * small multilingual lexicons - There are also lexicons which are multilingual. In the multilingual lexicons the words in each language are aligned with the words in the other languages in a way which preserves the meaning. Such lexicons are very difficult to build and usually have limited coverage. Despite this having such a lexicon in the resource library is good for testing purposes. We already have small testing lexicon but it would be good to import other similar resources like [LWT](http://wold.livingsources.org/).

  * domain specific lexicons - there are some domain specific lexicons which could still be reused in many applications. For example lexicons with country and city names, spelled in different languages, medical terms, chemical terminology, etc.


## GF Treebank ##

Until recently GF was used only for parsing with relatively small domain specific grammars. However now there is an efficient parsing algorithm for GF so it become feasible to think about large coverage parsers based on GF. The large coverage lexicons mentioned in the previous project are important step in this direction. Unfortunately this is not enough, we need some more information about the syntactic behavior of the words and some statistics about their usage. Fortunately this could be achieved by gathering statistics from different treebanks.

In this project we will focus on English and we will use the [Susanne](http://www.grsampson.net/RSue.html) treebank. Although this treebank is relatively small, it has the advantage that it is open-source. From the treebank we will extract statistics for the words from the large scale English lexicon that we already have imported in GF. There are several interesting things to extract - how often a given word is used as a noun, verb, etc; what kind of arguments the verbs have, i.e. does the verb require direct object or not (transitive/intransitive verb); how the behavior of the word depend on the context; how common some grammatical constructions are used compared to some other; ...

The extracted statistics will be used to build statistical disambiguation system for the English resource grammar library.

# Software Projects #

This group of applications contains proposals for building concrete applications using GF or ideas for extensions in the GF compiler.

## Web Applications ##

There are several existing web-based user interfaces for GF. Some of them are written directly in `JavaScript`, some others are written in Java with [Google Web Toolkit](http://code.google.com/intl/bg-BG/webtoolkit/). The different user interfaces has different advantages and disadvantages. In this project we want to unify them and create a library where the user interface could be customized for every specific application. It is also possible to implement some new demo applications. The project is suitable for students with experience in Web programming who are interested in how it is possible to use advanced natural language technologies on the Web.

## Google Android Applications ##

Mobile computing is a hot topic in the last years. There is already a work in progress project to port the GF interpreter to Java in order to use it on Google Android. This is still half the way to make GF usable on mobile phones. Students with interesting ideas for applications that involve translation and generation of natural language text on mobile phone are welcome to suggest a topic.

## Integrated Development Environment for GF ##

In this project we want to implement a prototype of a development environment for GF. This will be a plugin to some existing and popular development environment like Eclipse, Visual Studio, Emacs, etc. The choice of the concrete development environment will be made in the beginning of the project after looking at the advantages and the disadvantages of the different choices.


## Compiler from GF to Prolog ##

The grammars in GF have the distinctive feature that they make clear separation between concrete syntax i.e. English, French, etc. and abstract syntax i.e. some pure semantic description of a sentence. The abstract syntax is based on Martin Löf's type theory but have the interesting characteristic that it is restricted to first-order types. According to the Curry-Howard correspondence every type corresponds to a predicate in logic. This let us to see the types in the abstract syntax as a Prolog program. In fact we need a little but more than what is possible to do in Prolog but fortunately there is a dialect of Prolog which already has all this extras - [λProlog](http://www.lix.polytechnique.fr/~dale/lProlog/).

In this project we will generate λProlog programs from the definitions in the abstract syntax of GF. This will let us to use GF for simple logical reasoning and also we will be able to produce sentences in natural language which conforms to given semantic restrictions. This project would be interesting for students who has general interests in compilers for non-conventional programming languages.


## Framework for Annotations in GF ##

In this project we aim to extend the GF language in a way which will make it possible
to add custom information to every function or a category in a grammar. For example if someone wants to add statistical information to some function then he/she could do it without changing the core of the GF interpreter. For comparison in .Net a similar role is played by the attributes and in Java the equivalent are the recently added annotations. You could find information about the current design of the language extension [here](GFConfigurationFiles.md).
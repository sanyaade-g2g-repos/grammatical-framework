# GF Web Service API #

The GF Web Service is a small application which exposes the PGF API as Web Service. The application uses [FastCGI](http://www.fastcgi.com) as communication protocol to talk with the web server. The data protocol that we use is [JSON](http://www.json.org/). Information for how to compile and install the service could be found [here](LaunchWebDemos.md).

A compiled GF grammars could be used in web applications in the same way as JSP, ASP or PHP pages are used. The compiled PGF file is just placed somewhere in the web site directory. When there is a request for access to a .pgf file then the web server redirects the request to the GF web service. The service knows how to load the grammar and interpret the parameters given in the URL.

If my\_grammar.pgf is a grammar placed in the root folder of the web site for localhost then the grammar could be accessed using this URL:

`http://localhost/my_grammar.pgf`

Since there aren't any parameters passed in this case, the web service will respond with some general information about the grammar, encoded in JSON format. To perform specific command you have to tell what command you want to perform. The command is encoded in the parameter `command` i.e.:

`http://localhost/my_grammar.pgf?command=cmd`

where `cmd` is the name of the command. Usually every command also requires specific list of other arguments which are encoded as parameters as well. The list of all supported commands follows:

# Commands #


---


## Grammar ##

This command provides some general information about the grammar. This command is also executed if no `command` parameter is given.

### Input ###
| Parameter | Description                       | Default      |
|:----------|:----------------------------------|:-------------|
| command   | should be 'grammar'               | -            |

### Output ###
Object with three fields:

| Field        | Description                                      |
|:-------------|:-------------------------------------------------|
| name         | the name of the abstract syntax in the grammar   |
| userLanguage | the concrete language in the grammar which best matches the default language, set in the user's browser |
| categories   | list of all abstract syntax categories defined in the grammar |
| functions    | list of all abstract syntax functions defined in the grammar |
| languages    | list of concrete languages available in the grammar |

Every language is described with object having this two fields:

| Field        | Description                                      |
|:-------------|:-------------------------------------------------|
| name         | the name of the concrete syntax for the language |
| languageCode | the two character language code according to the [ISO standard](http://www.loc.gov/standards/iso639-2/php/code_list.php) i.e. en for English, bg for Bulgarian, etc. |

The language codes should be specified in the grammar because they are used to identify the user language. The web service receives the code of the language set in the browser and compares it with the codes defined in the grammar. If there is a match then the service returns the corresponding concrete syntax name. If no match is found then the first language in alphabetical order is returned.


---


## Parsing ##

This command parses a string and returns a list of abstract syntax trees.

### Input ###
| Parameter | Description                                  | Default      |
|:----------|:---------------------------------------------|:-------------|
| command   | should be 'parse'                            | -            |
| cat       | the start category for the parser            | the default start category for the grammar |
| input     | the string to be parsed                      | empty string |
| from      | the name of the concrete syntax to use for parsing | all languages in the grammar will be tried |
| limit     | limit how many trees are returned (gf>3.3.3) | no limit is applied |

### Output ###
List of objects where every object represents the analyzes for every input language. The objects have three fields:

| Field      | Description                               |
|:-----------|:------------------------------------------|
| from       | the concrete language used in the parsing |
| brackets   | the bracketed string from the parser      |
| trees      | list of abstract syntax trees             |
| typeErrors | list of errors from the type checker      |

The abstract syntax trees are sent as plain strings. The type errors are objects with two fields:

| Field      | Description                               |
|:-----------|:------------------------------------------|
| fid        | forest id which points to a bracket in the bracketed string where the error occurs |
| msg        | the text message for the error            |

The current implementation either returns a list of abstract syntax trees or a list of type errors. By checking whether the field trees is not null we check whether the type checking was successful.


---


## Linearization ##

The command takes an abstract syntax tree and produces string in the specified language(s).

### Input ###
| Parameter | Description                           | Default      |
|:----------|:--------------------------------------|:-------------|
| command   | should be 'linearize'                 | -            |
| tree      | the abstract syntax tree to linearize | -            |
| to        | the name of the concrete syntax to use in the linearization  | linearizations for all languages in the grammar will be generated |

### Output ###
| Field  | Description                                      |
|:-------|:-------------------------------------------------|
| to     | the concrete language used for the linearization |
| tree   | the output text                                  |


---


## Translation ##

The translation is a two step process. First the input sentence is parsed with the source language and after that the output sentence(s) are produced via linearization with the target language(s). For that reason the input and the output for this command is the union of the input/output of the commands for parsing and the one for linearization.

### Input ###
| Parameter | Description                       | Default      |
|:----------|:----------------------------------|:-------------|
| command   | should be 'translate'             | -            |
| cat       | the start category for the parser | the default start category for the grammar |
| input     | the input string to be translated | empty string |
| from      | the source language               | all languages in the grammar will be tried |
| to        | the target language               | linearizations for all languages in the grammar will be generated |
| limit     | limit how many parse trees are used (gf>3.3.3) | no limit is applied |

### Output ###

The output is a list of objects with these fields:
| Field          | Description                               |
|:---------------|:------------------------------------------|
| from           | the concrete language used in the parsing |
| brackets       | the bracketed string from the parser      |
| translations   | list of translations                      |
| typeErrors     | list of errors from the type checker      |

Every translation is an object with two fields:
| tree           | abstract syntax tree                      |
|:---------------|:------------------------------------------|
| linearizations | list of linearizations                    |

Every linearization is an object with two fields:
| Field          | Description                                     |
|:---------------|:------------------------------------------------|
| to             | the concrete language used in the linearization |
| text           | the sentence produced                           |

The type errors are objects with two fields:

| Field      | Description                               |
|:-----------|:------------------------------------------|
| fid        | forest id which points to a bracket in the bracketed string where the error occurs |
| msg        | the text message for the error            |

The current implementation either returns a list of translations or a list of type errors. By checking whether the field translations is not null we check whether the type checking was successful.


---


## Random Generation ##

This command generates random abstract syntax tree where the top-level function will be of the specified category. The categories for the sub-trees will be determined by the type signatures of the parent function.

### Input ###
| Parameter | Description                          | Default      |
|:----------|:-------------------------------------|:-------------|
| command   | should be 'random'                   | -            |
| cat       | the start category for the generator | the default start category for the grammar |
| limit     | maximal number of trees generated    | 1            |

### Output ###
The output is a list of objects with only one field:

| Field     | Description                        |
|:----------|:-----------------------------------|
| tree      | the generated abstract syntax tree |

The length of the list is limited by the limit parameter.


---


## Word Completion ##

Word completion is a special case of parsing. If there is an incomplete sentence then it is first parsed and after that the state of the parse chart is used to predict the set of words that could follow in a grammatically correct sentence.

### Input ###
| Parameter | Description                       | Default      |
|:----------|:----------------------------------|:-------------|
| command   | should be 'complete'              | -            |
| cat       | the start category for the parser | the default start category for the grammar |
| input     | the string to the left of the cursor that is already typed | empty string |
| from      | the name of the concrete syntax to use for parsing | all languages in the grammar will be tried |
| limit     | maximal number of trees generated    | all words will be returned |

### Output ###
The output is a list of objects with two fields which describe the completions.
| Field     | Description                       |
|:----------|:----------------------------------|
| from      | the concrete syntax for this word |
| text      | the word itself                   |


---


## Abstract Syntax Tree Visualization ##

This command renders an abstract syntax tree into image in PNG format.

### Input ###
| Parameter | Description                        | Default      |
|:----------|:-----------------------------------|:-------------|
| command   | should be 'abstrtree'              | -            |
| tree      | the abstract syntax tree to render | -            |
| format    | output format (gf>3.3.3)           | png          |

### Output ###
Byy default, the output is an image in PNG format. The content-type is set to 'image/png', so the easiest way to visualize the generated image is to add HTML element <img /> which points to URL for the visualization command i.e.:

```
<img src="http://localhost/my_grammar.pgf?command=abstrtree&tree=..."/>
```

The output can also be in gif ('image/gif'), svg ('image/svg+xml') or gv (graphviz) format by setting the 'format' option


---


## Parse Tree Visualization ##

This command renders the parse tree that corresponds to a specific abstract syntax tree. The generated image is in PNG format.

### Input ###
| Parameter | Description                        | Default      |
|:----------|:-----------------------------------|:-------------|
| command   | should be 'parsetree'              | -            |
| tree      | the abstract syntax tree to render | -            |
| from      | the name of the concrete syntax to use in the rendering   | -            |
| format    | output format (gf>3.3.3)           | png          |
| _options_ | additional rendering options (gf>3.4) | -            |

The additioal rendering options are: noleaves, nofun and nocat (booleans, false by default);
nodefont, leaffont, nodecolor, leafcolor, nodeedgestyle and leafedgestyle
(strings, have builtin defaults).

### Output ###
By default, the output is an image in PNG format. The content-type is set to 'image/png', so the easiest way to visualize the generated image is to add HTML element <img /> which points to URL for the visualization command i.e.:

```
<img src="http://localhost/my_grammar.pgf?command=parsetree&tree=..."/>
```

The output can also be in gif ('image/gif'), svg ('image/svg+xml') or gv (graphviz) format by setting the 'format' option


---


## Word Alignment Diagram ##

This command renders the word alignment diagram for some sentence and all languages in the grammar. The sentence is generated from a given abstract syntax tree.

### Input ###
| Parameter | Description                        | Default      |
|:----------|:-----------------------------------|:-------------|
| command   | should be 'alignment'              | -            |
| tree      | the abstract syntax tree to render | -            |
| format    | output format (gf>3.3.3)           | png          |
| to        | list of languages to include in the diagram (gf>3.4) | all languages supported by the grammar |

### Output ###
By default, the output is an image in PNG format. The content-type is set to 'image/png', so the easiest way to visualize the generated image is to add HTML element <img /> which points to URL for the visualization command i.e.:

```
<img src="http://localhost/my_grammar.pgf?command=alignment&tree=..."/>
```

The output can also be in gif ('image/gif'), svg ('image/svg+xml') or gv (graphviz) format by setting the 'format' option
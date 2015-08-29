The Syntax Editor is written in JavaScript and runs in a web browser. In order to use it you need the JavaScript version of the PGF interpreter together with some HTML and CSS and some pictures. This are the steps that you need to follow if you want to use the editor with some grammar:

  * Download the runtime system from:

> http://code.google.com/p/grammatical-framework/downloads/list

> or if you already have the latest sources of GF then you could find the code in src/runtime/javascript.

  * The grammar should be converted to format usable in JavaScript. Basically this is PGF encoded as JavaScript code. Compile the grammar by:
```
gf -make -output-format=js grammar1 grammar2 ...
```

  * either replace grammar.js in the runtime system by the resulting file or change the reference to grammar.js in editor.html to the new file name (line 9)

  * in editor.html, line 13, replace the abstract syntax name Food by the new grammar name
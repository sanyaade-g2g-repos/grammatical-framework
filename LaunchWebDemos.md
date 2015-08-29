# Required Software #

We use [lighttpd](http://www.lighttpd.net/) as web server and this is the one that we recommend. Previously we also tried Microsoft IIS and Apache but we found that lighttpd is easier to install and it has better support for [FastCGI](http://www.fastcgi.com/drupal/).

The client side of the web applications is written using [Google Web Toolkit](http://code.google.com/intl/bg-BG/webtoolkit/) which you also need to download and install. We are still depending on version 1.5.3 of the toolkit so you need exactly this one.

The 'Translate' application shows diagrams with abstract syntax trees, parse trees and word alignment. For this to work, you need to have [Graphviz](http://www.graphviz.org/) installed on the server. In particular the 'dot' command should be available in the search path.

You must have already compiled and installed the GF runtime library in Haskell. The normal way do to this is to follow the steps:
```
runghc Setup.hs configure
runghc Setup.hs build
runghc Setup.hs install
```
in the root directory of GF. For more information see [here](DevelopersPage.md).

# Compilation #

The first step in the compilation is to compile the PGF service. This is service on the server side which communicates via the web server using FastCGI as protocol. It provides services like parsing, linearization and translation. It is written in Haskell and it could be compiled in the usual way using Cabal. The source code is located in subdirectory **src/server**. The compilation is in three simple steps:

```
runghc Setup.hs configure
runghc Setup.hs build
runghc Setup.hs install
```

For this I assume that you already have installed the Haskell compiler GHC and four third party libraries:

```
cgi >= 3001.1.7.0,
fastcgi >= 3001.0.2.1,
json >= 0.3.3,
utf8-string >= 0.3.1.1
```

The simplest way to obtain them is to go to http://hackage.haskell.org

Once you have compiled the PGF service you should compile also the client side of the applications. There are two main applications which are located in subdirectory src/server/gwt. There are two shell scripts:

```
./Translate-compile
./Fridge-compile
```

which compile the source of those applications. You must look at the beginning of the scripts, there are two environment variables that have to be set to the installation location of Google Web Toolkit. There is an example in the scripts.

# Running #

Before to run the applications you have to deploy some grammars. First you need file grammars.xml placed at
```
gf/src/server/gwt/www/grammars/grammars.xml
```
The file describes which grammars are available for the applications. An example content of the file is:
```
<grammars>
   <grammar name="Foods.pgf"/>
   <grammar name="Bronzeage.pgf"/>
</grammars>
```
Later we might add other attributes here.

When you have everything in place you can try to run the applications. From directory src/server you run:

```
/usr/sbin/lighttpd -D -f lighttpd.conf
```

This will start the web server. After that you could try to open:

```
http://localhost:41296/translate
```
or
```
http://localhost:41296/fridge
```
in any web browser.
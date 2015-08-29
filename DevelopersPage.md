# Setting up your system for building GF #

Before building GF from sources you need to install some tools on your system.
GF is written in Haskell, so first of all you need recent version of the Haskell compiler GHC.
Currently we use GHC 6.12.1 and we recommend that you should use the same version
as well. This version is not backward compatible with the previous major releases
so you cannot use previous versions. GHC is available from here:

http://www.haskell.org/ghc/

Once you have installed GHC, open a terminal (Command Prompt on Windows) and try
to execute the following command:
```
$ ghc --version
```
This command should show you which version of GHC you have. If the installation
of GHC was successful you should see message like:
```
The Glorious Glasgow Haskell Compilation System, version 6.12.1
```
The other two tools that we use are the lexer generator for Haskell - [Alex](http://www.haskell.org/alex/) and the parser generator - [Happy](http://www.haskell.org/happy/).
Again after the installation check that the tools are available from the terminal.
If they are not then probably you have to update the current search path in your system.

All this tools are also part of the [Haskell Platform](http://hackage.haskell.org/platform/) which you could choose to install instead. However our experience shows that on some platforms the installation of the platform is not so straight forward. In this case just install the minimal set of tools.

You also need the library [Haskeline](http://hackage.haskell.org/package/haskeline) installed. If you want to check whether, you already have it then you can use the following command:
```
$ ghc-pkg list
```
which shows the list of all installed libraries. You either have to download and install Haskeline manually or if you have the Haskell Platform then do:
```
$ cabal install haskeline --global
```
which will download and install the library automatically.

There are three different ways to get the source code of GF:
  1. download the latest source package from http://www.grammaticalframework.org/download/index.html
  1. if you have the Haskell Platform then pull the sources from Hackage:
```
$ cabal install gf --global
```
  1. fetch the latest development version of GF from our Darcs repository.

[Darcs](http://darcs.net/) is a decentralized revision control system. There are precompiled packages for many platforms available at http://darcs.net/DarcsWiki/CategoryBinaries. There is also source code if you want to compile it yourself. Darcs is also written in Haskell and so you can use GHC to compile it.


# Getting the sources #

Once you have all tools in place you can get the GF sources. If you just want to compile and use GF
then it is enough to have read-only access. It is also possible to make changes in the sources but if
you want these changes to be applied back to the main sources you will have to send the changes to us.
If you plan to work continuously on GF then you should consider to get read-write access.

## Read-only access ##

### Getting a fresh copy for read-only access ###

Anyone can get the latest development version of GF by running (all on one line):

```
$ darcs get --lazy --set-scripts-executable http://www.grammaticalframework.org/ gf
```

This will create a directory called ```gf``` in the current
directory.


### Updating your copy ###

To get all new patches from the main repo:
```
$ darcs pull -a
```
This can be done anywhere in your local repository, i.e. in the ```gf```
directory, or any of its subdirectories.
Without ```-a```, you can choose which patches you want to get.


### Recording local changes ###

Since every copy is a repository, you can have local version control
of your changes.

If you have added files, you first need to tell your local repository to
keep them under revision control:

```
$ darcs add file1 file2 ...
```

To record changes, use:

```
$ darcs record
```

This creates a patch against the previous version and stores it in your
local repository. You can record any number of changes before
pushing them to the main repo. In fact, you don't have to push them at
all if you want to keep the changes only in your local repo.

If you think there are too many questions about what to record, you
can use the ```-a``` flag to ``record``. Or answer ``a`` to the first
question. Both of these record all the changes you have in your local
repository.


### Submitting patches ###

If you are using read-only access, send your patches by email to
someone with write-access. First record your changes in your local
repository, as described above. You can send any number of recorded
patches as one patch bundle. You create the patch bundle with:

```
$ darcs send -o mypatch.patch
$ gzip mypatch.patch
```

(where ```mypatch``` is hopefully replaced by a slightly more
descriptive name). Since some e-mail setups change text attachments
(most likely by changing the newline characters) you need to send
the patch in some compressed format, such as GZIP, BZIP2 or ZIP.

Send it as an e-mail attachment. If you have
sendmail or something equivalent installed, it is possible to send the
patch directly from darcs. If so, replace ```-o mypatch.patch``` with
```--to=EMAIL``` where ``EMAIL`` is the address to send it to.





## Read-write access ##

If you have a user account on www.grammaticalframework.org, you can get read-write access over SSH
to the GF repository.


### Getting a fresh copy ###

Get your copy with (all on one line),
replacing ```bringert``` with your own username on code.haskell.org:

```
$ darcs get --lazy --set-scripts-executable bringert@www.grammaticalframework.org:/usr/local/www/GF gf
```

The option ```--lazy``` means that you do not download all of the
history for the repository. This saves space, bandwidth and CPU time,
and most people don't need the full history of all changes in the
past.


### Getting other people's changes? ###

Get all new patches from the main repo:

```
$ darcs pull -a
```

Without ```-a```, you can choose which patches you want to get.



### Commit your changes ###

There are two steps to commiting a change to the main repo. First you
have to record the changes that you want to commit, then you push them
to the main repo. For instructions on recording your changes locally,
see "Recording local changes" above. Then you can push the patch(es) to
the main repo. If you are using ssh-access, all you need to do is:

```
$ darcs push
```

If you use the ```-a``` flag to push, all local patches which are not in
the main repo are pushed.



### Apply a patch from someone else ###

Use:

```
$ darcs apply < mypatch.patch
```

This applies the patch to your local repository. To commit it to the
main repo, use ```darcs push```.

## Further information about Darcs ##


For more info about what you can do with darcs, see http://darcs.net/manual/


# Compilation from sources #

The build system of GF is based on Cabal (see http://www.haskell.org/cabal/ for more information).
Cabal is installed by default together with the GHC compiler. This is actually a library which could
be used from Haskell to compile projects written in Haskell. The entry point is a script
called Setup.hs which is placed in the top directory of every project managed with Cabal.
The three main steps that are needed for compilation are much like what you do in a project
written in C, you have: configure, build and install:

```
$ runghc Setup.hs configure
$ runghc Setup.hs build
$ runghc Setup.hs install
```

For the last command you may need root permissions. Setup.hs also has other options which are custom for GF. See UsingSetupHs for more details.


# Compilation with make #

If you feel more comfortable with Makefiles then there is a thin Makefile
wrapper arround Cabal for you. If you just type:
```
$ make
```
the configuration phase will be run automatically if needed and after that
the sources will be compiled. If you don't want to compile the resource library
every time then you can use:
```
$ make gf
```
For installation use:
```
$ make install
```
For cleaning:
```
$ make clean
```
and to build source distribution archive run:
```
$ make sdist
```
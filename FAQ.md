# Frequently Asked Questions #


---

Q: I installed new version of GF and now the compiler complains that there are .gfo files compiled with different version of GF.

A: Remove all .gfo files that you have and recompile. These files are generated as output of compilation, so you don't loose any important information. Alternatively you could use option -src when compiling. This will force the compiler to ignore all existing .gfo files and to generate fresh one.

---

Q: I try to load a grammar but the compiler complains that it could not find some module.

A: The compiler doesn't know where to find the module. You must specify the search path. There is a page about how to setup the [search path](SearchPath.md).

---

Q: Is it possible to use GF with languages other than Haskell?

A: Yes. GF is a compiler which compiles grammars to format called PGF (Portable Grammar Format). The format is designed to be as simple as possible with the goal to make it easy to write interpreters in different languages. So far there are working interpreters in Haskell and JavaScript. There is also a Web Service written in Haskell which you could call from almost any language. There was also an old interpreter written in Java but it worked only with now obsolete version GF. There was some preliminary work on interpreter for C as well.

---

Q: How I can run the Web applications on my own server?

A: See the page LaunchWebDemos.

---

Q: I get a "Stack overflow" error when compiling a grammar. What I can do?

A: There is an option to the Haskell runtime which controls the stack size. When running gf add the options +RTS -K64M -RTS i.e.:
```
gf +RTS -K64M -RTS
```
The option -RTS is not obligatory if this is the last option in the command line. Here 64M means that you allocate 64 megabytes of memory for stack. You could experiment with different values.

---

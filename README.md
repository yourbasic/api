# Your basic API

### Thoughts on good API design

This text will contain thoughts on good API design.


## Introduction: Go, go and garbage collection

![go](go.jpg)

Most examples in this text will be written in Go, the language itself
being an outstanding example of good API design, with clean and simple
interfaces on top of a huge and complex implementation.

The `go` keyword is particularly striking: the syntax couldn't
be simpler and the semantics are explained in
[eight lines of text](https://golang.org/ref/spec#Go_statements)
in the language specification. Still, you rarely hear complaints
about thread support being limited in Go.

Garbage collection is another remarkable example.
The implementation is hideously complicated, while the API
is null and void: there is no syntax and the language specification
doesn't even mention the concept explicitly.

The aim of this text is to explore API design and try to find
some strategies and rules that could help us create code libraries
that are safe, efficient and easy to use.


## Basics

Let's start with the basic stuff. The rules never to be broken.
The rules you can't even argue with, but that we still need
to be reminded of, becuase we occasionally forget about them.

### Tell me what it is!

> A software repository should have a `README` file,
> and this file should say what the project is about.
> Preferably in the very first sentence.

If people don't know what it is, they can't use it, duh.
Still, it's not uncommon to see README files that starts with
"Candide now supports the Pangloss 3.2 file format"
but never tells what Candide is and what it has to offer.

In the [github.com/yourbasic/fenwick][fenwick] repository
I stick my neck out and attempt to implement a small library
as best I can. Take a look, and don't hesitate to open an issue
if the purpose of the library is unclear.


#### Stefan Nilsson – [korthaj](https://github.com/korthaj)

*This text is licensed under a [CC BY 3.0 License][CCBY30]*

[CCBY30]: https://creativecommons.org/licenses/by/3.0/
[gospec]: https://golang.org/ref/spec
[fenwick]: https://github.com/yourbasic/fenwick
[fenwickREADME]: https://github.com/yourbasic/fenwick/blob/master/README.md


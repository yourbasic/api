# Your basic API

### Thoughts on good API design

I plan to put my thoughts on good API design in this document.

### Go, go and garbage collection

![go](go.jpg)

Most examples in this text will be written in Go, the language itself
being an outstanding example of good API design, with clean and simple
interfaces on top of a huge and complex implementation.

The `go` keyword is particularly striking: the syntax couldn't be simpler
and the semantics are explained in
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


#### Stefan Nilsson – [korthaj](https://github.com/korthaj)

*This text is licensed under a [Creative Commons Attribution 3.0 Unported License][CCBY30]*

[CCBY30]: https://creativecommons.org/licenses/by/3.0/
[gospec]: https://golang.org/ref/spec


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
The rules we can't even argue with, but that we still need
to be reminded of, becuase we occasionally forget about them.

### 1. Tell me what this thing is!

#### The README file

> A software repository should have a `README` file,
> and this file should say what the project *is about*.
> Preferably in the very first sentence.

If people don't know what it is, they can't use it, duh.
Still, it's not uncommon to see README files that starts with
"Candide now supports the Pangloss 3.2 file format"
but never tells what Candide is and what it has to offer.

In the [github.com/yourbasic/fenwick][fenwick] repository
I stick my neck out and attempt to implement a small library
as best I can. Take a look, and don't hesitate to open an issue
if the purpose of the library is unclear.

### 2. Tell me what the code does!

> An API should say what the code *does*.

If a potential user gets past the README file, and dives into the
[fenwick documentation][fenwickDOC], she probably wants the full story.
Telling it three times is often a good approach.

First a short sentence stating the purpose of the package.

    Package fenwick provides a list data structure supporting prefix sums.

Then a slightly longer explanation.

    A Fenwick tree, or binary indexed tree, is a space-efficient
    list data structure that can efficiently update elements and
    calculate prefix sums in a list of numbers.
    
And finally, all the nitty-gritty details for those who intend to
acually use the code.

    Compared to a common array, a Fenwick tree achieves better
    balance between element update and prefix sum calculation –
    both operations run in O(log n) time – while using the same
    amount of memory. This is achieved by representing the list
    as an implicit tree, where the value of each node is the sum
    of the numbers in that subtree.


### 3. Don't tell me how it works!

> If at all possible, an API *shouldn't* reveal any
> details of the implemenatation.

Threading and garbage collection in Go are two great examples of interfaces
that get this right. In general, a Go programmer doesn't have to,
and doesn't want to, worry about the intricacies of garbage collection
and threading. "It just works."

In the [fenwick documentation][fenwickDOC] I didn't manage to hide
the implementation details completely, even though I tried:

- The data type is called `List`, not `Tree`. That's because it *does*
  the job of a list, even though it's implemented as a tree.
  I surely got that right.
 
- The cheesy stock picture in the README file depicts a list.
  Perhaps I got that right.

- The data structure is known as a Fenwick tree, or binary indexed tree.
  I couldn't change that. But it was my decision to include a few
  implementation details at the very end of the package documentation.
  Sorry about that.

#### Stefan Nilsson – [korthaj](https://github.com/korthaj)

*This text is licensed under a [CC BY 3.0 License][CCBY30]*

[CCBY30]: https://creativecommons.org/licenses/by/3.0/
[gospec]: https://golang.org/ref/spec
[fenwick]: https://github.com/yourbasic/fenwick
[fenwickREADME]: https://github.com/yourbasic/fenwick/blob/master/README.md
[fenwickDOC]: https://godoc.org/github.com/yourbasic/fenwick


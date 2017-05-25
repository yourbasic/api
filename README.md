# Your basic API

### Thoughts on good API design

This text will contain thoughts on good API design.


## Go, go and garbage

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


### 1. Tell me what this thing is

> A software repository should have a `README` file,
> and this file should say what the project **is about**.
> Preferably in the very first sentence.

If people don't know what it is, they can't use it, duh.
Still, it's not uncommon to see README files that starts with
"Candide now supports the Pangloss 3.2 file format"
but never tells what Candide is and what it has to offer.

In the [Fenwick repo][fenwick] I stick my neck out and try to
implement a small library that follows all of the basic rules.
Its README file starts out like this:

    # Your basic Bloom filter

    ### Golang probabilistic set data structure

    A Bloom filter is a fast and space-efficient probabilistic
    data structure used to test set membership. A membership test
    returns either ”likely member” or ”definitely not a member”.

### 2. Tell me what it does

> An API should say what the code **does**.

If a potential user gets past the README file, and dives into
the [Fenwick documentation][fenwickDOC], she probably wants
the full story. Telling it three times is often a good approach.

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


### 3. Don't tell me how it works

> If at all possible, an API **shouldn't** reveal any
> details of the implemenatation.

Threading and garbage collection in Go are two great examples of interfaces
that get this right. In general, a Go programmer doesn't have to,
and doesn't want to, worry about the intricacies of garbage collection
and threading. "It just works."

In the [Fenwick repo][fenwick] I didn't manage to hide
the implementation details completely, even though I tried:

- The data type is called `List`, not `Tree`. That's because it **does**
  the job of a list, even though it's implemented as a tree.
  I surely got that right.
 
- The cheesy stock photo in the README file depicts a list, not a tree.
  Perhaps I got that right.

- The data structure is known as a Fenwick tree, or binary indexed tree.
  I couldn't change that. But it was my decision to include a few
  implementation details at the very end of the package documentation.
  Sorry about that.
  
However, the documentation of the `fenwick.List`data type and its methods
describes what the code does without telling anything about how it works.
This has two major benefits:

- The library should be fairly easy to use. If you know what a list is,
  you're ready to go.
- There is plenty of room for improving and modifying the implementation
  without breaking backwards compatability.
  
That's a nice place to be in.


### 4. Grant me the right to use it

> Every public software project should have a license.

I'm not a lawyer, but it's my understanding that code without
a license can be legally used only by its author.

If your're looking for paying customers, you may want to seek
legal advice on licensing. However, to turn your project into
free and open-source software is easy: just put the proper
license text in the right places.

I like the [BSD 2-Clause License][BSD2]. Not only does it offer
the permissions, conditions and limitations that I want:

- Permissions: private and commercial use, modification, distribution.
- Conditions: license and copyright notice.
- Limitations: liablility and warranty.

It's also easy to apply: you add a single file to the top
directory of your repo.


### 5. Don't change what it does

> A software library needs to be backwards compatible.
> It's fine to improve the documentation, change the implementation,
> and even introduce new features. But, if at all possibe,
> **don't change the behavior**.

This is the tough one. There are two major problems here:

- If you break this rule, you will break other people's code.
- You need to get your API right at the very first shot.
  You may not be able to fix it later on.

It's not enough to just follow this rule, you also need to say that
you are doing so. As a library provider you're in the business of trust.
This is why your library needs to explain its compatability policies,
and why it should use semantic versioning.

#### Compatibility policies

TODO: Use Fenwick as a simple example and Go 1 as an advanced example.

[Go 1 and the Future of Go Programs][gocompat]

#### Semantic versioning

TODO: Briefly explain semantic versioning.

[Semantic Versioning 2.0.0][sv]


#### Stefan Nilsson – [korthaj](https://github.com/korthaj)

[BSD2]: https://opensource.org/licenses/BSD-2-Clause
[gocompat]: https://golang.org/doc/go1compat
[gospec]: https://golang.org/ref/spec
[fenwick]: https://github.com/yourbasic/fenwick
[fenwickREADME]: https://github.com/yourbasic/fenwick/blob/master/README.md
[fenwickDOC]: https://godoc.org/github.com/yourbasic/fenwick
[sv]: http://semver.org/



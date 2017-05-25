# Your basic API

### Thoughts on good API design

The aim of this text is to explore API design and try to find
strategies and rules that can help us create code libraries
that are safe, efficient and easy to use.

![go](go.jpg)

The examples are written in [Go][go], the language itself being
an outstanding example of good API design, with clean and simple
interfaces on top of a huge complex implementation.

The `go` keyword is particularly striking: the syntax couldn't
be simpler and the semantics are explained in
[eight lines of text](https://golang.org/ref/spec#Go_statements)
in the language specification. Still, you rarely hear complaints
about thread support being limited in Go.

Garbage collection in Go is another remarkable example.
The implementation is hideously complicated, while the API
is null and void: there is no syntax and the language specification
doesn't even mention the concept explicitly. It just works.

Even though API design is as much an art as a science, there still
are some fundamental rules that you should be aware of. Rules that
will cost you dearly if you break them. Enough so that they
deserve to be known as **The 5 Commandments**.


# The 5 Commandments

Let's start with the basic stuff. The rules never to be broken.
The rules that we don't need to argue about, but that we still
need to be reminded of, becuase we often forget about them.


### 1. Tell me what this thing is

> A software repository should have a `README` file,
> and this file should say what the project **is about**.
> Preferably in the very first sentence.

If people don't know what it is, they can't use it, duh.
Still, it's not uncommon to see README files that starts with
"Candide now supports the Pangloss 3.2 file format"
but never tells what Candide is and what it has to offer.

In the [Fenwick repo][fenwick] I stick my neck out and try to
implement a small library that follows the commandments.
Its README file starts out like this:

    # Your basic Fenwick tree
    
    ### Golang list data structure supporting prefix sums
    
    A Fenwick tree, or binary indexed tree, is a space-efficient
    list data structure that can efficiently update elements and
    calculate prefix sums in a list of numbers.


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
    
And finally, the nitty-gritty for those who want to know it all.

    Compared to a common array, a Fenwick tree achieves better
    balance between element update and prefix sum calculation –
    both operations run in O(log n) time – while using the same
    amount of memory. This is achieved by representing the list
    as an implicit tree, where the value of each node is the sum
    of the numbers in that subtree.


### 3. Don't tell me how it works

> If at all possible, an API **shouldn't** reveal any implemenatation details.

Threading and garbage collection in Go are two great examples of interfaces
that get this right. In general, a Go programmer doesn't have to,
and doesn't want to, worry about the intricacies of garbage collection
and threading. It just works.

In the [Fenwick repo][fenwick] I didn't manage to hide
the implementation details completely, even though I tried:

- The data type is called `List`, not `Tree`. That's because it **does**
  the job of a list, even though it's implemented as a tree.
  I surely got that one right.
 
- The cheesy stock photo in the README file depicts a list, not a tree.
  Perhaps I got that right.

- The data structure is known as a Fenwick tree, or binary indexed tree.
  That wasn't my choice, but I'm the one who included some implementation
  details at the end of the package documentation. Sorry about that.
  
However, the documentation of the `fenwick.List`data type and its methods
describes what the code does without telling anything about how it works.
This has two major benefits:

- The library should be fairly easy to use. If you know what a list is,
  you're ready to go.
- There is plenty of room for improving and modifying the implementation
  without breaking backwards compatability.
  
That's a nice place to be in.


### 4. Grant me the right to use it

> Every public software project needs a license.

I'm not a lawyer and this is not legal advice, but it's my understanding
that code without a license can only be legally used by its author.

If your're looking for paying customers, you may want to seek
actual legal advice on licensing. However, to turn your project into
free and open-source software is easy: just put the proper
license text in the right places.

I like the [BSD 2-Clause License][BSD2]. Not only does it offer
the permissions, conditions and limitations that I want:

- Permissions: private and commercial use, modification, distribution.
- Conditions: license and copyright notice.
- Limitations: liablility and warranty.

It's also easy to apply: you add a single file to the top
directory of your repo.


### 5. Don't change it

> A software library needs to be backwards compatible.
> It's fine to improve the documentation, change the implementation,
> and even introduce new features. But, if at all possibe,
> **don't change the API**.

This is the tough one. There are two major challenges here:

- If you break this rule, you will break other people's code.
- You need to get your API right at the very first attempt.
  You may not be able to fix it later on.

It's not enough to just follow this rule, you also need to say that
you are doing so. As a library provider you're in the business of trust.
This is why your library needs to explain its compatability policy,
and why you should consider using semantic versioning.


#### Compatibility policies

Fenwick's compatibility policy is very simple, but still explicitly stated:

    ### Roadmap
    
    * The API of this library is frozen.
    * Version numbers adhere to semantic versioning.
    
    The only accepted reason to modify the API of this package
    is to handle issues that can't be resolved in any other
    reasonable way.

[Go 1 and the Future of Go Programs][gocompat] is a complex and detailed
compatibility document. It's required study for anyone working with
large-scale library design and maintenance.


#### Semantic versioning

[Semantic versioning][sv] is a convention for specifying compatibility
using a three-part version number: `major.minor.patch`. You increment

- `major` when you make incompatible API changes,
- `minor` when you add functionality in a backwards-compatible manner, and
- `patch` when you make backwards-compatible bug fixes.

The semantic versioning specification itself currently sits at version
number `2.0.0`. This means that it broke  the fifth commandment,
and that no new featues or patches have been introduced since then.
Even so, it's a good convention to follow. And, once again,
the Go project gets it right.


# Keep it simple

![scissors](scissors.png)

*Image by [ZooFari][ZF], [CC BY 3.0][CCBY3].*

Even though API design often requires us to make diffcult trade-offs,
a simpler API is almost always a better API.

TODO:

Packages should be atomic, independent and composable.

#### Atomic

#### Independent

#### Composable

### Math is simple

If you can model your API on a mathematical abstraction, such as a set
or an interval, you're almost home free.

### Consistent is simple


# Don't rush it

![rest](rest.jpg)

*Rest at harvest, painting by [William-Adolphe Bouguereau][wab], public domain.*

The only sure-fire way to know if an API works as intended
is to use it over an extended period of time on different
types of tasks and projects.

TODO

> To be a good API designer you will have to prepare and eat
> your own dog food until you like it.


# Tools of the trade

![tool](tool.jpg)

*Hobel mit Spänen und Zimmermannsbleistift, image by [Uwe Aranas][ua], [CC BY-SA 3.0][CCBYSA3].*

Grand ideas and theories aside, in the end all human artefacts
are built by putting together the right bits and pieces.
This chapter contains a collection of API building blocks
that have proven there worth over time.

TODO

### Immutables

### Naming

### Errors


#### Stefan Nilsson – [korthaj](https://github.com/korthaj)

[BSD2]: https://opensource.org/licenses/BSD-2-Clause
[CCBY3]: https://creativecommons.org/licenses/by/3.0/deed.en
[CCBYSA3]: https://creativecommons.org/licenses/by-sa/3.0/deed.en
[go]: https://github.com/golang/go
[gocompat]: https://golang.org/doc/go1compat
[gospec]: https://golang.org/ref/spec
[fenwick]: https://github.com/yourbasic/fenwick
[fenwickLICENSE]: https://github.com/yourbasic/fenwick/blob/master/LICENSE
[fenwickREADME]: https://github.com/yourbasic/fenwick/blob/master/README.md
[fenwickDOC]: https://godoc.org/github.com/yourbasic/fenwick
[sv]: http://semver.org/
[ua]: https://commons.wikimedia.org/wiki/User:Cccefalon/Profile
[wab]: https://en.wikipedia.org/wiki/William-Adolphe_Bouguereau
[zf]: https://commons.wikimedia.org/wiki/User_talk:ZooFari

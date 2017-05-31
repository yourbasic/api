# Your basic API

### Thoughts on good API design

The aim of this text is to explore API design and try to find
strategies and rules that can help us create code libraries
that are safe, efficient and easy to use.

![go](res/go.jpg)

*Image from [Pixabay][pixabay], [CC0 1.0][CC0].*

Many examples are in [Go][go], the language itself being
a case study of good API design, with clean and simple
interfaces on top of a huge complex implementation.
There are also a few examples in [Java][java], a language
that has had more time to accumulate cruft in the form of
superfluous and dysfunctional elements.

The `go` keyword is particularly striking: the syntax couldn't
be simpler and the semantics are explained in
[eight lines of text](https://golang.org/ref/spec#Go_statements)
in the language specification. Still, you rarely hear complaints
about thread support being limited in Go.

Garbage collection in Go is another impeccable example.
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
need to be reminded of.


### 1. Tell me what this thing is

> A software repository should have a `README` file,
> and this file should say what the project **is about**.
> Preferably in the very first sentence.

If people don't know what it is, they can't use it, duh.
Still, it's not uncommon to see a `README` file that starts with
"Candide now supports the Pangloss 3.2 file format",
but never tells what Candide is and what it has to offer.

In the [Fenwick repo][fenwick] I stick my neck out and try to
implement a small library that follows The 5 Commandments.
Its `README` file starts like this:

    # Your basic Fenwick tree
    
    ### Golang list data structure supporting prefix sums
    
    A Fenwick tree, or binary indexed tree, is a space-efficient
    list data structure that can efficiently update elements and
    calculate prefix sums in a list of numbers.


### 2. Tell me what it does

> An API should say what the code **does**.

If a potential user gets past the `README` file, and dives into
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

#### Corollary

The 2nd Commandment comes with an obvious corollary:

> An API should say what **every exported function** does.

Once again, if you don't know what it does you can't use it, duh.
Undocumented functions are close to worthless. The only way to
use them safely is to perform a complete code review, including
necessary testing. That tends to be more work than writing
the &$#! code yourself.

A descriptive function name is a good start, but only rarely does
the name tell the full story. And the full story is what we need.
In programming, there is no room for guesswork.


### 3. Don't tell me how it works

> If at all possible, an API **shouldn't** reveal any implementation details.

Threading and garbage collection in Go are two examples of interfaces
that get this right. In general, a Go programmer doesn't have to,
and doesn't want to, worry about the intricacies of garbage collection
and threading. It just works.

In the [Fenwick repo][fenwick] I didn't manage to fully hide the implementation:

- The data type is called `List`, not `Tree`. That's because it **does**
  the job of a list, even though it's implemented as a tree.
  I surely got that one right.
 
- The cheesy stock photo in the `README` file depicts a list, not a tree.
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
  without breaking backwards compatibility.
  
That's a nice place to be in.


### 4. Grant me the right to use it

> Every public software project needs a license.

I'm not a lawyer and this is not legal advice, but it's my understanding
that code without a license can only be legally used by its own author.

If you're looking for paying customers, you may want to seek
actual legal advice on licensing. However, to turn your project into
free and open-source software is easy: just put the proper
license text in the right places.

I like the [BSD 2-Clause License][BSD2]. Not only does it offer
the permissions, conditions and limitations that I want:

- Permissions: private and commercial use, modification, distribution.
- Conditions: license and copyright notice.
- Limitations: liability and warranty.

It's also easy to apply: you add a single file to the top
directory of your repo.


### 5. Don't change it

> A software library needs to be backwards compatible.
> It's fine to improve the documentation, change the implementation,
> and even introduce new features. But, if at all possible,
> **don't change the API**.

This is the tough one. There are two major challenges here:

- If you break this rule, you will break other people's code.
- You need to get your API right at the very first attempt.
  You may not be able to fix it later on.

It's not enough to just follow this rule, you also need to say that
you are doing so. As a library provider you're in the business of trust.
This is why your library needs to explain its compatibility policy,
and why you should consider using semantic versioning.


#### Compatibility policies

Fenwick's compatibility policy is very simple, but still explicitly stated:

    ### Roadmap
    
    * The API of this library is frozen.
    * Version numbers adhere to semantic versioning.
    
    The only accepted reason to modify the API of this package
    is to handle issues that can't be resolved in any other
    reasonable way.

[Go 1 and the Future of Go Programs][gocompat] is a full detailed
compatibility document. It's required study for anyone working with
large-scale library design and maintenance.

For comparison, [Compatibility Guide for JDK 8][javacompat] is
an entry point to the complex world of Java compatibility.


#### Semantic versioning

[Semantic versioning][sv] is a convention for specifying compatibility
using a three-part version number: `major.minor.patch`. You increment

- `major` when you make incompatible API changes,
- `minor` when you add functionality in a backwards-compatible manner, and
- `patch` when you make backwards-compatible bug fixes.

The semantic versioning specification itself currently sits at version
number `2.0.0`. This means that it broke  The 5th Commandment,
and that no new features or patches have been introduced since then.
Even so, it's a good convention to follow. And, once again,
the Go project gets it right.


# Keep it simple

Even though API design often requires us to make difficult trade-offs,
a simpler API is almost always a better API.


### Don't use complicated constructs where simple ones will do

#### Functions

A function is a simple and beautiful thing. It's easy to use,
easy to understand, easy to test, and it doesn't come with any
hidden side effects. And, perhaps most importantly, functions
can be freely composed. The better part of mathematics
is built from functions. If you can find a nice design using
functions only, good for you.

(I'm talking about pure functions here, the ones that always produce
the same output given the same input; not the ones that output
the time of day or some other nasty surprise.)

#### Java detour

The `static` keyword in Java has a bad reputation. Probably because
it has so many different meanings. A static field is essentially a
global variable, and you typically want to avoid those.

A static method, however, is Java's twisted way to declare
a function, as opposed to a method. This is a case where Java
makes you jump through hoops to do the right thing. Functions
are great also for Java programmers. Don't be afraid to use them.

#### Objects

An object, or `struct` if you like, isn't quite as simple and
beautiful as a function, but it has memory. Many elegant and
powerful abstractions consist of a single object with a few
attached methods. The very best ones tend to be immutable.
At the core this is what object orientation is all about.
The rest is mostly bells and whistles.

#### Inheritance

Inheritance is pretty complicated. The older I get, the more seldom I feel
the need to use it. There are simpler and safer ways to design software.


### Don't use a lot where a little will do

    To paint a little thing like that you smeared
    Carelessly passing with your robes afloat, —
    Yet do much less, so much less, Someone says,
    (I know his name, no matter) — so much less!
    Well, less is more, Lucrezia: I am judged.

*From Andrea del Sarto by Robert Browning, 1855.*

Adding to Browning's advice would be a mistake. Instead, a war story:

The `java.io`, `java.nio`, `java.nio.channels`, `java.nio.channels.spi`,
`java.nio.file`, `java.nio.file.attribute`, `java.nio.file.spi`,
`java.nio.charset`,  and `java.nio.charset.spi` packages have many methods.
In fact, the API is so overwhelming that many of us end up at [Stack Overflow][so]
trying to move streams of bytes in and out of our Java programs.
Unfortunately, many of the friendly people who share code snippets
on Stack Overflow didn't read the full spec either, and got it wrong.

For many years I didn't know that my Java programs used the platform
default character encoding. That's an ugly bug, and I'm not the only one
to fall into this trap.

There probably is no way to design a really good general-purpose
IO library at this point. After all, such a library must support
low-level operations on many diverse platforms. But please, don't
add more fuel to the fire.

The Go `io` package is a fresh new start. The library takes some
getting used to, but it's small and manageable and handles the most
common use cases well. Unfortunately, no amount of API design can
fully protect us from the thorny history of file systems and
fleeting memory technologies.


### One package, one idea

![scissors](res/scissors.png)

*Image by [ZooFari][ZF], [CC BY 3.0][CCBY3].*

Chances are that you have never used the Java `Vector` class.
It was superseded by `ArrayList` already in Java 1.2.
The problem with `Vector` is that it does **two things**:

1. it's a synchronized data structure, and
2. it's a list.

Both of these things are very useful, but most of the time
you only want one of them.

There is nothing wrong with putting a lot of good stuff in you library.
The trick is to come up with suitable units
(classes or packages):

- units that do one well-specified thing,
- are independent of each other, and
- can be easily composed.


### Just say no

> An API shouldn't encourage bad design decisions.

The `add` method in Java's `ArrayList` is a case in point:

    public void add(int index, E element)
    
    Inserts the specified element at the specified position in this list.
    Shifts the element currently at that position (if any) and any subsequent
    elements to the right (adds one to their indices).

This method makes it easy to do the wrong thing. Adding a new element
to the middle of an array is really inefficient; something you should
typically avoid. That's what we have hash tables for.

I know that it's difficult to say no when the kids are throwing a tantrum.
But remember The 5th Commandment (not the one about honouring thy father
and thy mother), the one that says that a software library needs to be
backwards compatible. You might be able to beat your candy addiction,
but you don't get to remove anything from an API.


### Math is simple

> If people do not believe that mathematics is simple, it is only
> because they do not realize how complicated life is.

*John von Neumann*

If you can model your API on a mathematical abstraction,
such as a set or an interval, you're home free.
Mathematical abstractions tend to be atomic, well-specified,
independent, composable entities with a long story of use,
abuse and improvements along the way.

Take a look at [VertexSet][VertexSet], a data structure for
specifying a group of vertices in a graph. There are three
mathematical abstractions here:

- the vertices themselves are identified by **integers**,
- a group of vertices is a **set** with **union**, **intersection**,
  **set difference** and **membership** operations, and
- the constructor takes an **interval** as input.

There are two main reasons why I ended up with this API.
First, I believe that the included operations are useful,
necessary and sufficient. Secondly, they favor designs that
can be efficiently implemented in this particular package.


# Give it time

The only certain way to know if an API works as intended
is to use it over an extended period of time on different
types of tasks and projects. Don't rush it.

![rest](res/rest.jpg)

**Rest at harvest**
*Painting by [William-Adolphe Bouguereau][wab], 1865, public domain.*


![Lorne Greene](res/greene.jpg)

**Eat your own dog food**
*Commercial featuring Lorne Greene, 1970s, public domain.*


![Don Knuth](res/knuth.jpg)

> Thus, I came to the conclusion that the designer of a new system
> must not only be the implementor and the first large-scale user;
> the designer should also write the first user manual. The separation
> of any of these four components would have hurt TeX significantly.
> If I had not participated fully in all these activities, literally
> hundreds of improvements would never have been made, because I would
> never have thought of them or perceived why they were important.

**[The Errors Of TeX][texerrors]** *Don Knuth, 1989.*
*Image from [Wikipedia][wikiknuth], [CC BY-SA 2.5][CCBYSA2.5].*


# Show, don't tell

Tutorials, examples and quick start guides are great tools for
improving an API. The goal is to make it effortless to get started
and easy to perform common tasks.

### Tutorials

[A Tour of Go][gotour] is a nice example of both a quick start and
a tutorial. It's an interactive online tutorial that let's you try
Go programming inside your browser without installing any software.

### Examples

An example can demonstrate how an API is best used and
help clarify subtle points. This [Bloom filter example][bloomexample]
illustrates a typical Bloom filter use case, and it also helps clarify
the tricky semantics of a Bloom filter probabilistic membership test.

Once in a while, an example can fully replace a more standard API element.
Take a look this [DFS example][graphdfs], which shows how to implement
a depth-first search. Implementing DFS as a function with callbacks is
really messy. There are at least four different points in the code where
you may want to insert actions. Let's face it, occasionally cut and paste
is the better approach.


# Tools of the trade

![tool](res/tool.jpg)

**Hobel mit Spänen und Zimmermannsbleistift**
*Image by [Uwe Aranas][ua], 2014, [CC BY-SA 3.0][CCBYSA3].*

Grand ideas and theories aside, in the end all human artefacts
are built by putting together the right bits and pieces.
This is a growing list of tried-and-true API building blocks.

### Keep it consistent

> si fueris Romae, Romano vivito more

When in Rome, do as the Romans do.
In Java it's `toString`, `equals`, and `size`;
in Go it's `String`, `Equal`, and `Len`.
Suck it up, and get it right.

### Write functions that need little and give much

Java's `System.out.println` and Go's `fmt.Println` are real workhorses.
They'll take any input and offer lots of important information in return.
This is something we should strive for in our own API design.

### Find a fitting interface

The reason the print methods in Java and Go are so powerful
is not only that they take any input, they are also able
to handle that input in a sensible way.

In general, you want to find an interface that accepts everything
your code can handle; and little or nothing else.
This is typically very hard to do before you've had a good helping
of your own dog food. That's why I like the Go approach:

> You start by writing concrete code, and in the process
> you discover interfaces that are increasingly accurate.

The [graph.Iterator][graphit] interface is the result of my longest search
for a well-fitting interface so far. I tried numerous graph data structures
and implemented even more graph algorithms before finding a presentable fit.
There is no way I could have designed this up-front.

### Make it generic

A library based on a perfectly fitting interface is
a perfectly generic library. Think about that. 


#### Stefan Nilsson — [korthaj](https://github.com/korthaj)

[bloomexample]: https://godoc.org/github.com/yourbasic/bloom#example-package--Basics
[BSD2]: https://opensource.org/licenses/BSD-2-Clause
[CC0]: https://creativecommons.org/publicdomain/zero/1.0/deed.en
[CCBY3]: https://creativecommons.org/licenses/by/3.0/deed.en
[CCBYSA2.5]: https://creativecommons.org/licenses/by-sa/2.5/deed.en
[CCBYSA3]: https://creativecommons.org/licenses/by-sa/3.0/deed.en
[fenwick]: https://github.com/yourbasic/fenwick
[fenwickDOC]: https://godoc.org/github.com/yourbasic/fenwick
[fenwickLICENSE]: https://github.com/yourbasic/fenwick/blob/master/LICENSE
[fenwickREADME]: https://github.com/yourbasic/fenwick/blob/master/README.md
[go]: https://en.wikipedia.org/wiki/Go_(programming_language)
[gocompat]: https://golang.org/doc/go1compat
[gospec]: https://golang.org/ref/spec
[gotour]: https:tour.golang.org
[graph]: https://github.com/yourbasic/graph
[graphdfs]: https://godoc.org/github.com/yourbasic/graph#ex-package--DFS
[graphit]: https://godoc.org/github.com/yourbasic/graph#Iterator
[java]: https://en.wikipedia.org/wiki/Java_(programming_language)
[javacompat]: http://www.oracle.com/technetwork/java/javase/8-compatibility-guide-2156366.html
[pixabay]: https://pixabay.com/en/hand-finger-button-switch-start-944307/
[so]: https://stackoverflow.com/
[sv]: http://semver.org/
[texerrors]: http://dl.acm.org/citation.cfm?id=66416
[ua]: https://commons.wikimedia.org/wiki/User:Cccefalon/Profile
[VertexSet]: https://godoc.org/github.com/yourbasic/graph/build#VertexSet
[wab]: https://en.wikipedia.org/wiki/William-Adolphe_Bouguereau
[wikiknuth]: https://en.wikipedia.org/wiki/File:KnuthAtOpenContentAlliance.jpg
[zf]: https://commons.wikimedia.org/wiki/User_talk:ZooFari

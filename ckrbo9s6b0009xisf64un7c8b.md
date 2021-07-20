## Nude Models — Part I : Setters

Ye olde Reliable Data Structures and Their Controversial (Write) Access.
------------------------------------------------------------------------

_Using objects as data structures is an established practice that generates many problems associated with the maintainability and evolution of software and misuses brilliant concepts that were stated five decades ago. In this first part we will reflect on the_ **_writing_** _access of these objects._

In his classic [1972 paper](https://www.win.tue.nl/~wstomv/edu/2ip30/references/criteria_for_modularization.pdf), [David Parnas](https://en.wikipedia.org/wiki/David_Parnas) defined a novel and foundational concept for modern software engineering:[**Information Hiding.**](https://en.wikipedia.org/wiki/Information_hiding)

The rule is straightforward:

> If we hide our implementation we can change it as many times as necessary.

Prior to Parnas’ paper, there were no clear rules on information accessing and it was not a questionable practice to dive into data structures, penalizing any change with dreaded ripple effect.

Let’s see how to model a Cartesian point:

[https://gist.github.com/mcsee/d7c4b221d7b8c53a4d3f1a50973d4ec5](https://gist.github.com/mcsee/d7c4b221d7b8c53a4d3f1a50973d4ec5)

Point Struct

Any software component that manipulates these points will be coupled to saving values ​​as Cartesian _x_ and _y_ coordinates (**Accidental implementation**).

Since it's just a data structure without operations, the attribute’s semantics will be different according to every programmer’s criterion.

[https://mcsee.hashnode.dev/coupling-the-one-and-only-software-design-problem](https://mcsee.hashnode.dev/coupling-the-one-and-only-software-design-problem)

Hence, if we want to change the **accidental** implementation of the point to its polar coordinates analogous:

[https://gist.github.com/mcsee/fffd65b045c23c30d0f2731fc7092dfe](https://gist.github.com/mcsee/fffd65b045c23c30d0f2731fc7092dfe)

Polar Point![polar.png](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626762284105/nt-ZBlbkJ.png)

Same point can be represented in two different ways

![special-triangle-polar-and-cartesian.png](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626762287999/r1HWsUEO6.png)

The polar representation (√2, π/8) is equivalent to the Cartesian (1, 1)

Since it is the **same point** in real world, it must **necessarily** be represented by the same object in our bijection.

[https://mcsee.hashnode.dev/the-one-and-only-software-design-principle](https://mcsee.hashnode.dev/the-one-and-only-software-design-principle)

Bijection always depends on the **subjectivity** of the aspects we are trying to model. In order to draw a polygon, the Cartesian (1, 1) and polar (√2, π/8) points are the same point.

The case of trying to represent several possible mathematical representations would be different if we were programming [Wolfram](https://www.wolframalpha.com/) semantics. In such case representation **is part of the problem** se they would be modeled by **different** objects.

The solution is simple: Hide the internal representation.
---------------------------------------------------------

As Parnas predicted, many of the code maintainability issues were fixed by **encapsulating the decisions** within the modules that define them. This is what the magnificent paper is all about.

The next evolutionary step
--------------------------

Upon object oriented programming arrival, the concepts of encapsulation and information hiding were taken to an atomic extreme. We are no longer talking about encapsulating within a **module** but within the same **object**.

Returning to the previous example, we move from:

[https://gist.github.com/mcsee/e85b4194389f46c7b8f07f10f4296ec4](https://gist.github.com/mcsee/e85b4194389f46c7b8f07f10f4296ec4)

towards representation change:

[https://gist.github.com/mcsee/9d367dbe97e94f81ae534c0775f94b4b](https://gist.github.com/mcsee/9d367dbe97e94f81ae534c0775f94b4b)

> A good design is one in which objects are coupled to responsibilities (**interfaces**) and not representations.

Therefore, if we define a good point **interface**, they can arbitrarily change their representation (even on runtime) without propagating any ripple effect.

[https://gist.github.com/mcsee/4c61943f140fd78099ac61d92b6af436](https://gist.github.com/mcsee/4c61943f140fd78099ac61d92b6af436)

when representation changes …

[https://gist.github.com/mcsee/3c518ce3708147f653813bf7a7c1865d](https://gist.github.com/mcsee/3c518ce3708147f653813bf7a7c1865d)

… everything continues to work correctly.

Algorithms and Data
-------------------

If we were working with the old rule:

> programs = algorithms + data structures

… then we could build excellent software with _setters_ and _getters_.

This article assumes that we are eager to build, with declarative objects, models where implementation hides behind the objects’ responsibilities.

These responsibilities will be the same on the bijection between these objects and the real world.

[https://mcsee.hashnode.dev/what-is-wrong-with-software](https://mcsee.hashnode.dev/what-is-wrong-with-software)

Involution
----------

Despite the benefits listed in the examples above, the current state of the art shows us many problems related to coupling and ripple effect. Most are generated by the ingrained habit of using _setters_ and _getters_ (or simply: accessors).

![johannes-plenio-aWDgqexSxA0-unsplash.jpg](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626762297779/Xi8BACRCG.jpeg) Photo by [Johannes Plenio](https://unsplash.com/@jplenio) on [Unsplash](https://unsplash.com/s/photos/evolution)

Let’s look at setters and getters as separate problems.

Setters
-------

Changing the internal state of an object violates the principle of immutability. This is discouraged since, in the real world, objects do not mutate in their **essence**.

[https://mcsee.hashnode.dev/the-evil-powers-of-mutants](https://mcsee.hashnode.dev/the-evil-powers-of-mutants)

> The only method allowed to write to attributes is the atomic initialization. From then on, the variables should be read-only.

If we stay true to bijection, we will notice that there are never messages with the form **_setAttribute_**..() in the real world. These are implementation tricks programmers use, and they break good models.

> We will never be able to explain to a business expert what responsibility these methods have from the name.

Let’s imagine a polygon as a data structure.

[https://gist.github.com/mcsee/ad55d3fbce11fd17cb53da994dde6406](https://gist.github.com/mcsee/ad55d3fbce11fd17cb53da994dde6406)

> Let’s assume that the polygon has at least three vertices.

Being a data structure, we cannot impose such restriction.

Using our amazing IDE with automatic code generation, we add the _setters_ and _getters_ to it.

[https://gist.github.com/mcsee/e0fb2b6319ad6df5f838d01894f46c69](https://gist.github.com/mcsee/e0fb2b6319ad6df5f838d01894f46c69)

Let’s try adding the constraint on the number of vertices in the constructor:

[https://gist.github.com/mcsee/5d67de4e22f76db964ae06ae33dad6de](https://gist.github.com/mcsee/5d67de4e22f76db964ae06ae33dad6de)

From now on, it will be impossible to create a polygon with less than three sides, thus fulfilling the bijection with the real world of Euclidean geometry.

Unless we use our setter …

Nothing prevents us from running this code:

[https://gist.github.com/mcsee/f0353e604ae04b9479ac00b8762f64b2](https://gist.github.com/mcsee/f0353e604ae04b9479ac00b8762f64b2)

At this point we have two options:

1.  Duplicate the business logic in the constructor and in the setter.
2.  Eliminate the setter permanently, favoring immutability

In case of accepting the repeated code, the ripple effect begins to spread when our restrictions grow. For example, if we make the precondition even stronger:

> Let’s assume that the polygon has at least three different vertices.

The correct answer, according to our design axioms, is the second.

Repeated or absent logic of invariant verification
==================================================

Many objects have invariants that guarantee their cohesion and the validity of the representation to maintain real world bijection. Allowing partial setting (an attribute) would force us to control representation invariants in **more than one place**, generating **repeating code**, which is always error-prone when modifying a reference and ignoring other references.

Code generated without control
==============================

Many development environments give us the possibility to automate the generation of setters and getters. This leads to new programmers generations thinking it is a good design practice, generating vices that are difficult to correct.

[https://mcsee.hashnode.dev/lazyness-ii-code-wizards](https://mcsee.hashnode.dev/lazyness-ii-code-wizards)

This facility spreads the problem, having this tool gives the feeling that it is an accepted practice.

Recommendations
===============

*   Do not use _setters_. There are no good reasons for doing so.
*   Having methods named **_setSomething_… ()** is a code smell.
*   Have no public attributes. For practical purposes it is like having _setters_ and _getters_.
*   Have no public static attributes. In addition to what is mentioned above, the classes should be stateless and this is a _code smell_ showing a class that is used as a global variable.
*   Avoid [anemic objects](https://en.wikipedia.org/wiki/Anemic_domain_model) (those containing just attributes without responsibilities). This is a _code smell_ hinting some missing object on the bijection.

[https://mcsee.hashnode.dev/code-smell-01-anemic-models](https://mcsee.hashnode.dev/code-smell-01-anemic-models)

[https://maximilianocontieri.com/code-smell-28-setters](https://maximilianocontieri.com/code-smell-28-setters)

Conclusions
===========

Using setters generates coupling and prevents the incremental evolution of our computer systems. For the arguments stated in this article, we should restrict its use as much as possible.

The Getters
===========

As with setters, getters are discouraged. We develop this topic in depth in this article:

[https://mcsee.hashnode.dev/nude-models-part-ii-getters-ckedliv3e01vutds11ikg3ctt](https://mcsee.hashnode.dev/nude-models-part-ii-getters-ckedliv3e01vutds11ikg3ctt)

* * *

Part of the objective of this series of articles is to generate spaces for debate and discussion on software design.

[https://mcsee.hashnode.dev/object-design-checklist](https://mcsee.hashnode.dev/object-design-checklist)

We look forward to comments and suggestions on this article.

This article is also available in Spanish [here](/diseño-de-software/information-showing-chapter-i-setters-138deb558e5d).
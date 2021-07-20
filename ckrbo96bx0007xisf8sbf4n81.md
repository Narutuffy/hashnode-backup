## Code Smell 02 - Constants and Magic Numbers

_A method makes calculations with lots of numbers without describing their semantics._

> TL;DR: Avoid Magic numbers without explanation. We don't know their source and we are very afraid of changing then.

Problems
========

*   [Coupling](https://maximilianocontieri.com/coupling-the-one-and-only-software-design-problem)
*   Low testability
*   Low readability

Solutions
=========

1) Rename the constant with a semantic and name (meaningful and intention revealing).

2) Replace constants with parameters, so you can mock them from outside.

3) The constant definition is often a different object than the constant (ab)user.

Examples
========

*   Algorithms Hyper Parameters

Sample Code
===========

Wrong
-----

    <?
    
    function energy($mass) {
    
        return $mass * (300000 ^ 2);
    }
    

Right
-----

    <?
    
    function energy($mass) {
    
        return $mass * (LIGHT_SPEED_KILOMETERS_OVER_SECONDS ^ 2);
    }
    

Detection
=========

Many linters can detect number literal in attributes and methods.

Tags
====

*   Hard coded
*   Constants

More info
=========

*   [Refactoring Guru](https://refactoring.guru/es/replace-magic-number-with-symbolic-constant)
*   [How to Decouple a Legacy System](https://maximilianocontieri.com/how-to-decouple-a-legacy-system)

Credits
=======

Photo by [Kristopher Roller](https://unsplash.com/@krisroller) on [Unsplash](https://unsplash.com/s/photos/magic)

* * *

> In a purely functional program, the value of a \[constant\] never changes, and yet, it changes all the time! A paradox!

_Joel Spolsky_

[https://maximilianocontieri.com/software-engineering-great-quotes](https://maximilianocontieri.com/software-engineering-great-quotes)

* * *

This article is part of the CodeSmell Series.

[https://maximilianocontieri.com/how-to-find-the-stinky-parts-of-your-code](https://maximilianocontieri.com/how-to-find-the-stinky-parts-of-your-code)

_Last update: 2021/05/31_
## Code Smell 77 - Timestamps

_Timestamps are widely used. They have a central issuing authority and do not go back, do they?_

> TL;DR: Don't use timestamps for sequence. Centralize and lock your issuer.

Problems
========

*   Bijection Fault.
    
*   Timestamp Collisions.
    
*   Timestamp precision.
    
*   Packet Disorders.
    
*   Bad [Accidental Implementation](https://maximilianocontieri.com/no-silver-bullet) (Timestamp) for an Essential Problem (Sequencing).
    

Solutions
=========

1.  Use a centralizing sequential stamper. (NO, not a [Singleton](https://maximilianocontieri.com/singleton-the-root-of-all-evil)).
    
2.  If you need to model a sequence, [model a sequence](https://maximilianocontieri.com/the-one-and-only-software-design-principle).
    

Sample Code
===========

Wrong
-----

    # using time module
    import time
    
    # ts stores the time in seconds
    ts1 = time.time()
    ts2 = time.time() #might be the same!!
    

Right
-----

    numbers = range(1, 100000)
    #create a sequence of numbers and use them with a hotspot
    
    #or
    sequence = nextNumber()
    

Detection
=========

Timestamps are very popular on many languages and are ubiquitous.

We need to use them just to model... timestamps.

Tags
====

*   Bijection

Conclusion
==========

This smell was inspired by recent [Ingenuity software fault](https://www.hebergementwebs.com/transport/the-autonomous-helicopter-mars-named-ingenuity-is-confused-by-a-time-stamp-issue-providing-insightful-lessons-for-self-driving-cars-ai).

If we don't follow our [MAPPER](https://maximilianocontieri.com/the-one-and-only-software-design-principle) rules and model sequences with time, we will face trouble.

Luckily, Ingenuity is a sophisticated Autonomous vehicle and has a robust fail-safe landing software.

This video describes the glitch

[https://www.youtube.com/watch?v=6IoMiwxL2wU](https://www.youtube.com/watch?v=6IoMiwxL2wU)

Relations
=========

[https://maximilianocontieri.com/code-smell-39-new-date](https://maximilianocontieri.com/code-smell-39-new-date)

[https://maximilianocontieri.com/code-smell-32-singletons](https://maximilianocontieri.com/code-smell-32-singletons)

[https://maximilianocontieri.com/code-smell-71-magic-floats-disguised-as-decimals](https://maximilianocontieri.com/code-smell-71-magic-floats-disguised-as-decimals)

More info
=========

*   [Timestamp proposed changes](https://ieeexplore.ieee.org/document/805196)
    
*   [Build a Mapper](https://maximilianocontieri.com/the-one-and-only-software-design-principle)
    
*   [What is wrong with software](https://maximilianocontieri.com/what-is-wrong-with-software)
    

* * *

> The most beautiful code, the most beautiful functions, and the most beautiful programs are sometimes not there at all.

_Jon Bentley_

[https://maximilianocontieri.com/software-engineering-great-quotes](https://maximilianocontieri.com/software-engineering-great-quotes)

* * *

This article is part of the CodeSmell Series.

[https://maximilianocontieri.com/how-to-find-the-stinky-parts-of-your-code](https://maximilianocontieri.com/how-to-find-the-stinky-parts-of-your-code)
## Code Smell 79 - TheResult

_If a name is already used, we can always prefix it with 'the'._

> TL;DR: don't prefix your variables.

Problems
========

*   Readability
    
*   Meaningless names
    

Solutions
=========

1.  Use intention revealing names.
    
2.  Avoid _Indistinct noise words_.
    

Sample Code
===========

Wrong
-----

    var result;
    
    result = getSomeResult();
    
    var theResult;
    
    theResult = getSomeResult();
    

Right
-----

    var averageSalary;
    
    averageSalary = calculateAverageSalary();
    
    //..
    
    var averageSalaryWithRaises;
    
    averageSalaryWithRaises = calculateAverageSalary();
    

Detection
=========

As with many of our naming conventions, we can instruct our linters to forbid names like _theXxx..._.

Tags
====

*   Readability

Conclusion
==========

Always use intention revealing names.

If your names collide use local names, extract your methods and avoid 'the' prefixes.

Relations
=========

[https://maximilianocontieri.com/code-smell-38-abstract-names](https://maximilianocontieri.com/code-smell-38-abstract-names)

[https://maximilianocontieri.com/code-smell-81-result](https://maximilianocontieri.com/code-smell-81-result)

More info
=========

*   [What is in a name](https://maximilianocontieri.com/what-exactly-is-a-name-part-ii-rehab).
    
*   [How To Be Great At Giving Meaningful Names](https://medium.com/shipmnts/how-to-be-great-at-giving-meaningful-names-54b19de66cdf).
    

Credits
=======

Photo by [Josue Michel](https://unsplash.com/@josuemichelphotography) on [Unsplash](https://unsplash.com/s/photos/chosen-one)

* * *

> One difference between a smart programmer and a professional programmer is that the professional understands that clarity is king. Professionals use their powers for good and write code that others can understand.

_Robert C. Martin_

[https://maximilianocontieri.com/software-engineering-great-quotes](https://maximilianocontieri.com/software-engineering-great-quotes)

* * *

This article is part of the CodeSmell Series.

[https://maximilianocontieri.com/how-to-find-the-stinky-parts-of-your-code](https://maximilianocontieri.com/how-to-find-the-stinky-parts-of-your-code)
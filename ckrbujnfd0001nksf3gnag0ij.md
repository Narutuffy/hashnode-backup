## Code Smell 80 - Nested Try/Catch

_Exceptions are a great way of separating happy path versus trouble path. But we tend to over-complicate our solutions._

> TL;DR: Don't nest Exceptions. Nobody cares of what you do in the inner blocks.

Problems
========

*   Readability

Solutions
=========

1.  Refactor

Sample Code
===========

Wrong
-----

    try {
        transaction.commit();
    } catch (e) {
        logerror(e);
        if (e instanceOf DBError){
          try {
              transaction.rollback();
          } catch (e) {
              doMoreLoggingRollbackFailed(e);
          }
        }
    }
    
    //Nested Try catchs
    //Exception cases are more important than happy path
    //We use exceptions as control flow
    

Right
-----

    try {
        transaction.commit();
    } catch (transactionError) {
        this.withTransactionErrorDo(transationError, transaction);
    }
    
    //transaction error policy is not defined in this function
    //so we don't have repeated code
    //code is more readable
    //It is up to the transaction and the error to decide what to do
    

Detection
=========

We can detect this smell using parsing trees.

Tags
====

*   Exceptions

Conclusion
==========

Don't abuse exceptions, don't create Exception classes no one will ever catch, and don't be prepared for every case (unless you have a good real scenario with a covering test).

Happy path should always be more important than exception cases.

Relations
=========

[https://maximilianocontieri.com/code-smell-73-exceptions-for-expected-cases](https://maximilianocontieri.com/code-smell-73-exceptions-for-expected-cases)

[https://maximilianocontieri.com/code-smell-26-exceptions-polluting](https://maximilianocontieri.com/code-smell-26-exceptions-polluting)

More Info
=========

*   [Nested Try/Catchs](https://beginnersbook.com/2013/04/nested-try-catch/)

Credits
=======

Photo by [David Clode](https://unsplash.com/@davidclode) on [Unsplash](https://unsplash.com/s/photos/fishing-net)

Thanks to [Rodrigo](https://hashnode.com/@rodrigomd) for his inspiration

[https://twitter.com/\_rodrigomd/status/1403359513965731843](https://twitter.com/_rodrigomd/status/1403359513965731843)

* * *

> Writing software as if we are the only person that ever has to comprehend it is one of the biggest mistakes and false assumptions that can be made.

_Karolina Szczur_

[https://maximilianocontieri.com/software-engineering-great-quotes](https://maximilianocontieri.com/software-engineering-great-quotes)

* * *

This article is part of the CodeSmell Series.

[https://maximilianocontieri.com/how-to-find-the-stinky-parts-of-your-code](https://maximilianocontieri.com/how-to-find-the-stinky-parts-of-your-code)
## Code Smell 78 - Callback Hell

_Processing an algorithm as a sequence of nested callbacks is not clever._

> TL;DR: Don't process calls in a callback way. Write a sequence.

Problems
========

*   Readability
    
*   Hard to debug.
    
*   Complexity
    

Solutions
=========

1.  Change callbacks to sequence calls.
    
2.  Extract repeated Code
    
3.  Refactor.
    

Sample Code
===========

Wrong
-----

    var fs = require('fs');
    
    var fileWithData = '/hello.world';  
    fs.readFile(fileWithData, 'utf8', function(err, txt) {  
        if (err) return console.log(err);
    
        txt = txt + '\n' + 'Add Data!';
        fs.writeFile(fileWithData, txt, function(err) {
            if(err) return console.log(err);
            console.log('Information added');
        });
    });
    

Right
-----

    var fs = require('fs');
    
    function logTextWasAdded(err) {  
        if(err) return console.log(err);
        console.log('Information added');
    };
    
    function addData(error, actualText) {  
        if (error) return console.log(error);
    
        actualText = actualText + '\n' + 'Add data';
        fs.writeFile(fileWithData, actualText, logTextWasAdded);
    }
    
    var fileWithData = 'hello.world';  
    fs.readFile(fileWithData, 'utf8', addData);
    

Detection
=========

This problem shines at the naked eye. Many linters can detect this complexity and warn us.

Tags
====

*   Readability
    
*   Complexity
    

Conclusion
==========

Callback Hell is a very common problem in programming languages with futures or promises.

Callbacks are added in an incremental way. There's no much mess at the beginning.

Complexity without refactoring makes them hard to read and debug.

Relations
=========

[https://maximilianocontieri.com/code-smell-06-too-clever-programmer](https://maximilianocontieri.com/code-smell-06-too-clever-programmer)

* * *

> There are two ways to write code: write code so simple there are obviously no bugs in it, or write code so complex that there are no obvious bugs in it.

_Tony Hoare_

[https://maximilianocontieri.com/software-engineering-great-quotes](https://maximilianocontieri.com/software-engineering-great-quotes)

* * *

This article is part of the CodeSmell Series.

[https://maximilianocontieri.com/how-to-find-the-stinky-parts-of-your-code](https://maximilianocontieri.com/how-to-find-the-stinky-parts-of-your-code)
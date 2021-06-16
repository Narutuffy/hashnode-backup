## JSON Datadata and PHP


Please be aware if you don't have JSON encode installed on your PHP install nowadays, you should probably just switch hosts, but this post is left here for historic purposes.

I have noticed with the Yahoo YUI! Javascript library use JSON Data alot for there Datasources.  Now Turning your PHP Array into a JSON Array is very easy namely if you use PHP 5.2+ but even if not there are some easy to get Librarys out there that can convert your array in no time and you don't need full root access to your server to do it.  In this article I will only explain how to do it in the *nix Systems as that is all that I use.  But if you look around enough i'm sure you can find some information for windows too.



## PHP 5.2




* * *



In PHP 5.2 The JSON Library comes bundled with PHP so its very easy and extremely fast.  here is a example of how you would use these new functions

~~~ php
<?php

    $json = array('name'=>'value');
    
    echo json_encode($json);
        
?>

~~~

Which should produce

~~~ javascript
    {"name":"value"}
~~~

## PHP Below 5.2 Without Root Access

* * *

Below 5.2 without root access, the way to go is thru the PERL librarys here is the [link](http://pear.php.net/pepr/pepr-proposal-show.php?id=198) 


~~~ php
<?php

    $json = array('name'=>'value');
    
    echo json_encode($json);
        
?>

~~~


## PHP Below 5.2 With Root Access


* * *


For those of you that need to keep your current version of PHP but want the speed gained by using the PHP JSON extension can download and install it yourself, granted you have root access to the box your on. The installation is very simple, just follow the directions on the web site and you can have this extension installed and working in 10 minutes.

http://www.aurore.net/projects/php-json/

This way you can use the native PHP JSON functions, so the example code for PHP 5.2.0 or higher will work.

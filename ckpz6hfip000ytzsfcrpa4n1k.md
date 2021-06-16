## CodeIgniter - Bonfire Series - Quicktip Module Level Routing


HMVC CodeIgniter will read a routes file located at 

    
      modules/modulename/config/routes.php  
    



The basic gist of using this though is that the very first segment in your route file must be the module's name, so if you had a module named foo your routes will would need to look something like the following and the controller you are looking for is named "bar" in Bonfire your routes file would look something like the following.


~~~ php
<?php

    $route['foo/content/bar/(:any)/(:any)/(:any)/(:any)'] = "bar/$1/$2/$3/$4";
    $route['foo/content/bar/(:any)/(:any)/(:any)']		  = "bar/$1/$2/$3";
    $route['foo/content/bar/(:any)/(:any)']		          = "bar/$1/$2";
    $route['foo/content/bar/(:any)'] 		              = "bar/$1";
    $route['foo/content/bar']				              = "bar";

?>    
~~~


Now this can be frustating to say the least to get set-up and working properly but say you have 2 controllers that are related to the same module, you would want to keep them inside of the same module.  So this will enable you to write requests to your new controller.

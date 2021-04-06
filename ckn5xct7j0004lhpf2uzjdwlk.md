## CodeIgniter - Bonfire Series - View Partials from Modules


# CodeIgniter - Bonfire Series - Using Modules as View Partials


This is one thing that got me a bit confused when I first started using Bonfire, if your gonna use a Module as a View Partial which can be called using the Modules::run method then
you will get a error since the module will be echoed out before the Template is ready.  To fix this issue, or a work around for this issue that I use quite a lot is simply to return a view
and remove the Default Template functions from the Module method that I am calling.  Since this is a quick tip, I'm not gonna load you down with tons of code just a quick example and a way
to use it.

Now in your Module you should have a method that is called, in this method you instead of using Template::render(); we will simply be returning a view variable, which can be echoed out or
whatever you want to do with it.  

  


Now in your Module Method instead of calling the Template Class we will instead be using CodeIgniter's default view library, which can be used for returning JavaScript, HTML, whatever, in this
case we will be returning a View Partial to be used in a Theme.

Basiclly at the end of your Module Method, you will replace any Template calls with this
      
~~~ php
      <php>
        
        $data = array (
                       'variables' => $variable,
                       'variable2' => $variable2
                      )
                      
        return $this->load->view('modulename/viewfile', $data, true);
      </php>
~~~
      



Now in your Template or Your Block or wherever you want to call this View Partial from you will simply need to add the following code
  
I hope this help's someone out in the future, in understanding how Bonfire and HMVC work together.



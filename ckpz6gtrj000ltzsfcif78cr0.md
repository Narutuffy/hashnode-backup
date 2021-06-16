## Working with URLs like a boss in CodeIgniter or Bonfire



This is mostly just a post that I refer to a lot when I'm trying to remember how to get a certain URL param and couldn't remember it, so here's a few of my tricks and goodies out of my bag of treats.

# Working with URL Params in CodeIgniter

Target a Specific Value
=======================

~~~ php

	//-------------------------------------
	//! n=1 for controller, n=2 for method
	//-------------------------------------	
	$this->uri->segment(n); 

~~~

Using the Router Class
======================

~~~ php

	//-------------------------------------
	//! Get Controller Name 
	//-------------------------------------	
	$this->router->fetch_class();

	//-------------------------------------
	//! Get Method Name 
	//-------------------------------------	
	$this->router->fetch_method();

	//-------------------------------------
	//! Get Module Name in HMVC
	//-------------------------------------	
	$this->router->fetch_module();

~~~
	
Now these aren't the only ways to do this, and there are a ton of other options with CodeIgniter in General but, I found these little snippets quite handy and useful.

Enjoy! Cheers and Free Beers!
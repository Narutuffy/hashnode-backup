## CodeIgniter - Bonfire Series - Secure Your Modules


This is a method I learned recently to help secure your controller's,  I personally use it for my modules that are used as view partial's.  It's a simple remap method

~~~ php
<?php
	public function _remap($method)
	{
		if (method_exists($this, $method))
		{
			$this->$method();
		} else {
			show_404();
		}
	}    
?>	
~~~ 


All this method does is remap any calls to your controller and checks if a method exists, if it does then it calls the method otherwise it shows a 404 page.  This can be expanded on greatly but this is just a small code snippet I use a lot.

I will expand on this soon, this is just a very basic method.
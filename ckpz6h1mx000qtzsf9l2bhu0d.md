## Using CodeIgniters set_content Instead of echoing out of the controller


CodeIgniter is designed to allow you to turn caching of the output on/off and even allows you to use MEMCACHE, APC, FILE and even DUMMY!!

So Why would we want to do all this $this->output->set_output() junk when you could be lazy and just say echo($output)

Caching is why! My friends, unless your building a website that won't ever scale up then get used to setting your outputs this way.  I personally set all my output using these method's just incase the app does need to scaleup or I have to profile it and find what's making things slow.

Anyway enough talk, let's get down to business shall we?

**Important:** If you do set your output manually, it must be the last thing done in the function you call it from. For example, if you build a page in one of your controller functions, don't set the output until the end.

Permits you to manually set the final output string. Usage example:
===================================================================

~~~ php

<?php

	//-------------------------------------------------------------------------------
	// This is the command make a snippet like I do!
	//-------------------------------------------------------------------------------
    $this->output->set_output($data);

	//-------------------------------------------------------------------------------
	// This sets the Headers of the output, so you can set JSON, JPG, PNG, etc.
	//-------------------------------------------------------------------------------
    $this->output->set_content_type('application/json');

?>
~~~


Permits you to set the mime-type of your page so you can serve JSON data, JPEG's, XML, etc easily.
====================================================================================================
~~~ php

<?php

	//-------------------------------------------------------------------------------
	// This sets the output you can put a string, json encoded array, whatever in there.
	//-------------------------------------------------------------------------------
	$this->output->set_content_type($data);

	//-------------------------------------------------------------------------------
	// Now let's get funky, let's output a JSON object and even send the correct headers! Smexy!
	//-------------------------------------------------------------------------------
	$this->output->set_content_type('application/json')
	             ->set_output(json_encode(array('foo' => 'bar')));

	//-------------------------------------------------------------------------------
	// Now to the strange and bizare, let's send out a image by reading the file, sending it out!
	//-------------------------------------------------------------------------------
	$this->output->set_content_type('jpeg')
	             ->set_output(file_get_contents('files/something.jpg'));

?>
~~~

Important: Make sure any non-mime string you pass to this method exists in config/mimes.php or it will have no effect.

@TODO: Find Eric Barnes Blog Reference on Output caching
@TODO: Link to CI User Guide's Output Class

Anyway enough snippets for now my friends, go forth and kode shiz now!

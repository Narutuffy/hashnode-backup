## Adding ChromePhp to CodeIgniter Bonfires log methods


Earlier this year a extension and PHP class were released that allowed logging PHP messages to Chrome's Console, similar to FirePHP for FireBug but well for Google's Chrome Browser instead yay!.  Anyway I've grown quite fond of both Chrome and the Console logging of my debug messages recently and decided maybe some of the other Bonfire users might find some use in this.  

Basically Bonfire has 2 helper methods called **logit** one as a application helper and one in the model, these handy helper's make a log entry in CodeIgniter's system log, and use the Console class already included in Bonfire to output any message.  The only problem with this is normally when you have a error the page doesn't finish rendering and the stock Bonfire Console library doesn't display any data since it's not rendered.  So I just made a few quick changes to the logit methods and added the helper.  And now I have better debugging ability!

First get the helper and the extension, my friend [Marco Monterio](http://blog.marcomonteiro.net/post/28986094928/using-chromephp-with-codeigniter) has already explained on his blog how to install the extension and the helper, so check his blog out on installing the ChromePhp library to CodeIgniter.  Once you have it installed, then you can make these changes and you'll be debugging like a kingpin while your off getting your ChromePhp on, I'll gonna drink that beer you bought me :beer: 

Ok, so we only need additions to 2 files and 2 methods, this is easy!  Let's start with MY_Model.  So open up "/bonfire/application/core/MY_Model.php", do a quick search for "logit" and add the code I have below for ChromePhp

~~~ php
<?php
	/**
	 * Logs an error to the Console (if loaded) and to the log files.
	 *
	 * @param string $message The string to write to the logs.
	 * @param string $level   The log level, as per CI log_message method.
	 *
	 * @access protected
	 *
	 * @return mixed
	 */
	protected function logit($message='', $level='debug')
	{
		if (empty($message))
		{
			return FALSE;
		}

		if (class_exists('Console'))
		{
			Console::log($message);
		}

		// Just add from here
		if (class_exists('ChromePhp'))
		{
			if ($level == 'error')
			{
				ChromePhp::error($message);
			}				
			else
			{
				ChromePhp::log($message);
			}				
		}
		// Just to here.

		log_message($level, $message);

	}//end logit()

	//--------------------------------------------------------------------
?>
~~~


Ok so there's the Model changes, now open up "bonfire/application/helpers/application_helper.php" and once again find the "logit" function

~~~ php
<?php
	/**
	 * Logs an error to the Console (if loaded) and to the log files.
	 *
	 * @param $message string The string to write to the logs.
	 * @param $level string The log level, as per CI log_message method.
	 *
	 * @return void
	 */
	function logit($message='', $level='debug')
	{
		if (empty($message))
		{
			return;
		}

		if (class_exists('Console'))
		{
			Console::log($message);
		}

		if (class_exists('ChromePhp'))
		{
			if ($level == 'error')
			{
				ChromePhp::error($message);
			}				
			else
			{
				ChromePhp::log($message);
			}				
		}

		log_message($level, $message);
	}

	//--------------------------------------------------------------------
?>
~~~

And there ya go, now everytime logit is called, you can see what the message was inside of your Chrome Web Inspector Console just like you would a JavaScript console.log method would give.  Amazing stuff!  

Cheers and Free beers again!

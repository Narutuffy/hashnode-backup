## CodeIgniter - Loading HMVC Modules Like Controllers


Here is 2 short little tricks I use a lot in my CodeIgniter Development processes, one is for Cross-Loading Module Controllers
and the Other is Keeping your Application Config separated from the Project App Config.

This is a neat little trick using HMVC CodeIgniter to Access Module functions across from Different Modules.  Normally you can use a Modules::run code to create a View Partial but what if you need to run a Modules method and use the Data in a Different module?  

You can load Module Controllers just like a Library using the following.

~~~ php
<?php
    // Load the Module/Controller
    // If the Controller Name is the Same as the Module You Do not need to add it also.
    $this->load->module('articles');

    // Now we can access this Controller just like a library and run methods inside of it.
    // My Articles Module is Based on Markdown, and this snippet came from my Homepage.
    // By Accessing the Articles Controller and Running it's methods I don't have to worry about dependancies
    $data = $this->articles->fetch_article_byid ( $page_id );
?>    
~~~

Ok hope that makes sense to everyone.  The next one is a real small snippet I use in all my projects to keep my config files separated from my Project Config files.

At the very bottom of your application config file, just add the following code.
~~~ php
<?php
// This might hurt your install, if it does take it out.
if ( file_exists( APPPATH . 'config/app_config.php' ) )
{
	include ( APPPATH . 'config/app_config.php');
}
?>
~~~

That's just a simple way to keep your personal site's application config files separated from a Open Source Projects config files which will make upgrading later easier.

Hope these tips helped someone out.



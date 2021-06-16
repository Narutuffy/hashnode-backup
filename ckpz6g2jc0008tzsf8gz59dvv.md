## CodeIgniter - Bonfire Series - Quick-tips Module Config Files


This is something I haven't seen documented before and is really easy to work with. Currently in Bonfire your Module Menu Name and Sub-menu's are all set in your Module's config.php file. This is basically a array of information so Let's say you created a Articles module, but you want it to show in Bonfire's menu as Pages for some reason, all you have to do is open up your /modulename/config/config.php file and Change the name of it. Here's a example.


~~~ php

<?php if ( ! defined('BASEPATH')) exit('No direct script access allowed');
$config['module_config'] = array(
	'description'	=> 'Albums Module for Photo Gallery',
	'name'		    => 'Photo Albums', // Change this to Modify the menu name
	'version'		=> '0.0.3',
	'author'		=> 'Shawn Crigger',
	'menus'	        => array( 'content'	=> 'albums/content/menu') // to Add Submenu just link a array with the Context name as the key and a Valid View file for the menus.
);
?>

~~~

And the menu view file located at "albums/views/content/menu" looks like this.

~~~ php

<?php
/**
 *  Albums Admin Sub-menu View File,  this adds extra menu items to the main menu.
 *
 *  This work is licensed under the Creative Commons Attribution 3.0 Unported
 *  License. To view a copy of this license, visit
 *  http://creativecommons.org/licenses/by/3.0/ or send a letter to Creative
 *  Commons, 444 Castro Street, Suite 900, Mountain View, California, 94041, USA.
 *
 *  @category   views
 *  @subpackage Albums
 *  @subpackage Admin_Controller
 *  @subpackage Bonfire
 *  @package    CodeIgniter
 *  @author     Shawn Crigger <support@s-vizion.com-->
 *  @copyright  2012 S-Vizion Software and Developments
 *  @license    http://creativecommons.org/licenses/by/3.0/  CCA3L
 *  @version    1.0.0 (last revision: Apr 23, 2012)
 */
?>
<ul>
	<li><a href="&lt;?php echo site_url(SITE_AREA.'/content/albums') ?&gt;">Albums</a></li>
	<li><a href="&lt;?php echo site_url(SITE_AREA.'/content/albums/photos') ?&gt;">Album Photos Management</a></li>
</ul>
~~~

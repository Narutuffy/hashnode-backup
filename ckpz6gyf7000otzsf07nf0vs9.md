## CodeIgniter - Bonfire Series - Database Migration Systems


One of my favorite parts of Bonfire is the built in Migration System, If you already understand what they are for then you can skip down
a few paragraphs and I'll show you how to make them. For those who don't know what a Migration system is, it basicly like version control for
your database schemes. Let's say you created a Media Module and when you created it, a few weeks go by and you decided to add another
Database Table or even a Index Key. Well that's the best part of the Migration system, All you have to do is Create a new Migration controller, stick
it in your module/migrations folder and now you can add to your database or remove from it, Sure beats trying to remember what
you changed on a Module you built a year ago and upgraded 3 or 4 times. Anyway on to how to properly make one.
File Name and Controller Name

The syntax for naming your files and controllers is 000_install_modulename_whatever.php 
Now the numerics at the start should go higher then the last one, so if you have a 001_install then you need to make it 002
Let's start with one I created last night, since I used to Module Builder using a pre-made MySQL Table, so the Module Builder
Created my permissions for me, but didn't create a SQL table incase I want to reuse this module at a later date. So let's go ahead and make a Migration to add
and delete the SQL Table.
View Code PHP

~~~ php
<?php if (!defined('BASEPATH')) exit('No direct script access allowed');

// Class and file name must match or you will kill a kitten and a evil unicorn will rob you
// Ok Now the Filename for this file is 002_Install_media_tables.php
// The Controller name, I just personally remember to remove the numbers and add Migration in front of the file name.


class Migration_Install_media_tables extends Migration {

	/*
		method: up()

		This method is run everytime you upgrade your migration, it handles the actual database creation. 

	*/
	public function up() 
	{
		//Set the Database Prefix

		$prefix = $this->db->dbprefix;

		//You can use SQL or dbforge,  I personally prefer the dbforge, it's well documented in the CodeIgniter User Guide.

		//Since where creating a table, very simple let's setup the fields to add
		$this->dbforge->add_field('`id` int(11) NOT NULL AUTO_INCREMENT');
		$this->dbforge->add_field("`media_cat` int(11) NOT NULL");
		$this->dbforge->add_field("`filename` VARCHAR(200) NOT NULL");
		//Let's set the primary key, since the second parameter is set to true this will become a primary key 
		$this->dbforge->add_key('id', true);
		//Let's set the secondary key, since this table has a N to Many relation ship, I added a key for the media category. 
		$this->dbforge->add_key('media_cat');

		//And now that we have the table all ready to go, let's create it.  Tell me that wasn't faster then trying to figure it out with PhpMyAdmin or something.
		$this->dbforge->create_table('media_files');

	}

	//--------------------------------------------------------------------

	/*
		method : down();

		Performs a quick drop table on the actual table
	*/
	public function down() 
	{
		$prefix = $this->db->dbprefix;
		$this->dbforge->drop_table('media_files');
	}

	//--------------------------------------------------------------------

}		
?>
~~~

Well I hope this helps someone with creating Migrations on there own

Bonfire's Awesome Module Builder is just a quick kick-start to building your modules, learning the Migrations and how to add/remove DB Fields with your
migrations will make your modules more scalable, flexable, and if you only have to build the module one time, Then your building Assets that the next time
you need one, you can just pop it in place and be done with it. The re-usability of the HMVC modules with CodeIgniter is execellent and Bonfire has made it
even better.
If you have any questions or ideas for guides that you would like to see post a comment, send me a message, or catch me on twitter.
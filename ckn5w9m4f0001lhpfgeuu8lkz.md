## Extending Bonfires User system or EAV Users and PHP Development




Bonfire's base user system is based on a **Entity-Attribute-Value** based system where you store the user's meta in a seperate table, very similar to how wordpress is setup.

This has a lot of benefits but can also cause a lot of confusion when it comes down to extending the user system,  as with extending anything you want the end product to be maintainable but still be powerful and do everything you needed.

Bonfire's default user system is setup with a main user table and a meta table, kinda something like this

**User Table**

| UserID | Name   | Email | Pass |
|--------+--------+-------+------|
|   1    | shawnc | email | pass |



**Meta Table**

|  UserID  |   Key      | Value  |
|----------+------------+--------|
|  1       | first_name | shawn  |
|  1       | last_name  | c      |


So basically the meta values are stored in a seperate table and added to the current_user object as needed with **$this->user_model->load_meta**, this does a good job for
most systems I' develop but every so often a piece of the user data that isn't important enough to alter the entire system but I needed to be able to display this data like it was in the same table, so instead of 2 tables it would look like


**Joined table result**

| UserID | Name   | Email | Pass | first_name | last_name |
-----------------------------------------------------------
|   1    | shawnc | email | pass |    shawn   |    c      |

To generate the joined table as above I personally found joining the tables worked very well and did not cause to much server load, any suggestions will be taken if you have a different method but anyway to join the tables what I personally did was add a new method to the user model something like this.

~~~ php
<?php
	/**
	 * Loads user and meta data in one query.
	 *
	 * @param  int  $user_id      user_id or blank for all.
	 * @param  bool $show_deleted [show deleted users]
	 *
	 * @return mixed
	 */
	function load_user_meta($user_id = 0, $show_deleted == FALSE)
	{

		$this->db->select('users.*, roles.role_name');

    	$this->db->select('m1.meta_value AS first_name');

    	// Just keep adding these and alias as you need!
    	$this->db->select('m2.meta_value AS last_name');

    	//
    	$this->db->select('m2.meta_value AS alias');


    	$this->db->join('user_meta', 'user_meta.user_id = users.id', 'left');

    	// Here is the magic baby!
		$this->db->join('user_meta as m1', "m1.user_id = users.id AND m1.meta_key = 'first_name' ", 'left');


		$this->db->join('user_meta as m2', "m2.user_id = users.id AND m2.meta_key = 'last_name' ", 'left');

		// ! Change me
		$this->db->join('user_meta as m2', "m2.user_id = users.id AND m2.meta_key = 'alias' ", 'left');

		if ($show_deleted === FALSE)
		{
			$this->db->where('users.deleted', 0);
		}

		$this->db->where('users.banned =', 0);
		$this->db->group_by('users.id');

		if (is_numeric($user_id) && $user_id > 0)
		{
			return parent::find($user_id);
		}

		return parent::find_all();
	}
?>
~~~

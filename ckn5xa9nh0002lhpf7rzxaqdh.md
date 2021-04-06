## CodeIgniter - Bonfire Series - Events Systems Part 1


# CodeIgniter - Bonfire Series - Events Systems Part 1

*Please Note this is out of date.*

With CodeIgniter you have a Hooks system which allows you to add code without hacking the Core code,  with Bonfire we've taken that a bit futher with system Events, you can find the default Bonfire Events documentation [here](http://guides.cibonfire.com/0.6/system_events.html). which provides execellent documentation on what they are and how they work. Now let's do something with them shall we?  

In this guide, I'm gonna show you how to add extra profile fields to the user table via the user meta table.  The first step is create our "user_extension"  module and add some methods we're gonna need.  Start out with your standard directory structure
	
  * modules/
    * user_extension/
      * controllers/
      * models/			
      * views/

Now let's create a controller named "users_extension.php" to handle our Event callback's.
    
~~~ php
<?php

    public function __construct()
    {
        load->helper('form');
        $this->load->library('form_validation');
        $this->form_validation->CI =& $this;

        if (!class_exists('User_model'))
        {
            $this->load->model('users/User_model', 'user_model');
        }
    }
    
	//--------------------------------------------------------------------

	public function render_user_form( $data )
	{
			$user_id = (int) $data->id;

			$meta = $this->user_model->find_user_and_meta( $user_id );
			$data = array ('user' => $meta );

			echo $this->load->view('user_extension/user_form', $data , true );
	}

	//--------------------------------------------------------------------

	/*
		Method: save_user()

		Saves one or more key/value pairs of additional meta information
		for a user called from System Events.


		Parameters:
			$data		- An array of key/value pairs to save.
	*/
	public function save_user( $data )
	{
			$this->load->helper('array');

			/**
			 * Fetch the User ID, not sure why it's not in the payload? Maybe this needs to be fixed.
			 **/
			$the_user = $this->user_model->select('id')->find_by('email', $data['email']);
			$the_user = (int) $the_user->id;


			/**
			 * @var array   This is the user meta fields, I am using CI's array helper to pull the array keys, I want to use in the meta
			 **/
			$data    = elements( array('street_1', 'street_2', 'city', 'state_code', 'country_iso', 'zipcode', 'editor'), $data );

			$this->user_model->save_meta_for( $the_user, $data );

	}

	//--------------------------------------------------------------------

	/*
	  method: after_login

	  Called after successful login. Payload is an array of ‘user_id’ and ‘role_id’.
	*/
	public function after_login ( $payload )
	{

	}

	//--------------------------------------------------------------------

	/*
	  method: before_logout

	  Called just before logging the user out. Payload is an array of ‘user_id’ and ‘role_id’.
	*/
	public function before_logout ( $payload )
	{

	}

	//--------------------------------------------------------------------

	/*
	  method: after_create_user

	  Called after a user is created. Payload is the new user’s id.
	*/
	public function after_create_user ( $payload )
	{

	}

	//--------------------------------------------------------------------

	/*
	  method: before_user_update

	  Called just prior to updating a user. Payload is an array of ‘user_id’ and ‘data’, where data is all of the update information passed into the method.
	*/
	public function before_user_update ( $payload )
	{

	}

	//--------------------------------------------------------------------

	/*
	  method: before_user_update

	  Called just prior to updating a user. Payload is an array of ‘user_id’ and ‘data’, where data is all of the update information passed into the method.
	*/
	public function after_user_update ( $payload )
	{

	}

//--------------------------------------------------------------------

}

/* End of file user_extension.php */
/* Location: ./modules/controllers/user_extension/user_extension.php */
?>    
~~~
    
This controller has so far every user system event call back in it.  You'll notice I added a method to the meta data and to render the view file, so let's create the view file.

~~~ php
    
	<fieldset>
		<legend>Extra User Meta Stuff</legend>

			<div class="control-group">
				<label class="checkbox" for="editor">Editor</label>
				<div class="controls">
					<label>
						<input type="checkbox" name="editor" value="1" id="editor">
						Editor
					</label>
				</div>
			</div>
	</fieldset>

	<?php $settings_lib->item('auth.use_extended_profile')) :?>
	<fieldset>
		<legend></legend>

		<div class="control-group">
			<label class="control-label" for="street_1"></label>
			<div class="controls">
				<input type="text" name="street_1" value="<?php echo isset($user) ? $user->street_1 : set_value('street_1') ?>" class="span6"></input>
			</div>
		</div>

		<div class="control-group">
			<label class="control-label" for="street_2"></label>
			<div class="controls">
				<input type="text" name="street_2" value="<?php echo isset($user) ? $user->street_2 : set_value('street_2') ?>" class="span6"></input>
			</div>
		</div>

		<div class="control-group">
			<label class="control-label" for="city"></label>
			<div class="controls">
				<input type="text" name="city" value="<?php echo isset($user) ? $user->city : set_value('city') ?>" class="span6"></input>
			</div>
		</div>

		<div class="control-group">
			<label class="control-label" for="iso"></label>
			<div class="controls">
				<?php echo isset($country_iso) ? $user->country_iso : 'US', 'US'); ?>
			</div>
		</div>

		<div class="control-group">
			<label class="control-label" for="state_code"></label>
			<div class="controls">
				state_code : '', 'SC', isset($user) && !empty($user->country_iso) ? $user->country_iso : 'US'); ?>
			</div>
		</div>

		<div class="control-group">
			<label class="control-label" for="zipcode"></label>
			<div class="controls">
				<input type="text" name="zipcode" value="<?php echo isset($user) ? $user->zipcode : set_value('zipcode') ?>" class="span6"></input>
			</div>
		</div>

	</fieldset>
~~~
    

Ok, when the User Profile or Admin Update User is called, this view will be rendered, but before that happens we need to setup the Events config file, so let's open up /bonfire/application/config/events.php


~~~ php
<?php
$config['event_test'],
	'filepath'		=> 'controllers',
	'filename'		=> 'tests.php',
	'class'			=> 'Event_Test',
	'method'		=> 'check_name'
);

$config['render_user_form'][] = array(
	'module'		=> 'user_extension',
	'filepath'		=> 'controllers',
	'filename'		=> 'users_extension.php',
	'class'			=> 'Users_extension',
	'method'		=> 'render_user_form'
);

$config['before_user_update'][] = array(
	'module'		=> 'user_extension',
	'filepath'		=> 'controllers',
	'filename'		=> 'users_extension.php',
	'class'			=> 'Users_extension',
	'method'		=> 'before_user_update'
);

$config['after_user_update'][] = array(
	'module'		=> 'user_extension',
	'filepath'		=> 'controllers',
	'filename'		=> 'users_extension.php',
	'class'			=> 'Users_extension',
	'method'		=> 'after_user_update'
);

/* End of file user_extension.php */
/* Location: ./applications/config/events.php */
?>
~~~
    

Ok that's all I have time for on this guide, I hope this helps someone out there, and you can find the source code on github, in the next guide I'll be expanding on this one so stay tuned to that same bat channel fo sho!

[Github Repo for this demo](http://github.com/svizion/Bonfire-Events-Demo)

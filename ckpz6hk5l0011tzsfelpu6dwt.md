## using bonfire without bootstrap in frontend


Something I've noticed in recent days is using Bonfire without Bootstrap in the front end, for the most part it works fine with just changes to the User Module view files, but one thing that will cause issues is the Form helper since it has been converted to use Bootstrap.  

It's a easy fix thou, imagine that!  Since the Form helper is a Helper and a Library you just need to rename to files and then in the Admin Base Controller (MY_Controller.php) load those files so that any Admin modules will still work properly.

So rename the following to bootstrap-form_helper.php and bootstrap-form.php

 * bonfire/application/helpers/MY_form_helper.php
 * bonfire/application/libraries/form.php

Once those are renamed, open up MY_Controller, find the Admin_Controller and load the helper.

~~~ php
	$this->load->helper('bootstrap-form');
~~~

And that is all.  Over and Out!
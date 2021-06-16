## CodeIgniter - Bonfire Series - Quick Tips


Here's a very short quick tip that a lot of newer Bonfire users get stumped with.  In the "form_validation" library, if you use the unique rule you need to make sure you set a ID to verify against.  

Since the unique validation rule uses checks to make sure that it is unique it needs something to check against.  By adding this little snippet of code before your call to the form validation library you will save yourself some grief when the front-end developer decides to delete the hidden input for a ID or what not.

Anyway here's the snippet enjoy

~~~ php
<?php
    // Makes sure a $_POST ID was inserted to verify some validation rules against.
    
    if ( !$this->input->post('id') )
    {
    	$_POST['id'] = $id;
    }
?>    
~~~
    



That will make sure a id is in the POST array just in case.

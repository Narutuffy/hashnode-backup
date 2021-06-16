## CodeIgniter - Bonfire Series - Quick-tips Fast Database to Form Dropdown menus


In the .6 Development branch there's a feature I added to the Model that makes creating form select box options very very easy.   Basically every-time I needed to make a Drop-down I had to reformat my results which was annoying to me, so I added a quick method in BF_Model to make this easier on everyone.  Since creating Select Boxes from Database results is a very common CRUD related feature, I figured this would make life easier for everyone.  It's not in the current doc's yet so I figured I'd make a quick post to show you how to use it.

It's a very simple method, I normally add a extra method in whatever Model I'm inside of to add a Blank option but that's not really needed.  Here's a quick snippet of code from one of my own models that show's how to use it.


    
~~~ php
    <?php

    /**
     * Returns formatted array of ID, PageTitles for Sub Page Dropdown Menus.
     *
     * @return array
     */
    public function pages_dropdown ( )
    {
        $results = $this->format_dropdown( $this->key , 'name');
        $options = array('0' => 'Please select ....');
        foreach ($results as $key => $value)
        {
            $options[$key] = $value;
        }

    	return $options;
    }
    ?>
~~~
    
You don't even need to add the extra method I added but it does make life a little easier to add a blank option like I do.  Basically the syntax of the method is simple

~~~ php
<?php 
  $this->some_model->format_dropdown('key','label');
?>  
~~~

If your using your primary key then you actually don't need to set the key, so you could just use


~~~ php
<?php
      $this->some_model->format_dropdown('label');
?>
~~~



This will return a properly formatted array for CodeIgniter's form_dropdown function, where ID's are Array Keys with the Field Names as Values.

Hope this tip help's someone out, if you need the code to display the form just make a comment and I'll post that also.


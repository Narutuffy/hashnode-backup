## A quick tip on CodeIgniter and Bonfire's Model for evil!


Yes its 2am and I can't sleep so let's make a quick post on using Bonfire's Model for evil :P Ok that was just joke.  Anyway Basically Bonfire's Model was based on a lot of theory's and code from other CI CRUD happy models.  Let's make it work for You in a way wasn't designed for!  N to 1 Relation's on DB Tables are easy with our MY_Model core,  not many people figured this out but setting up the table and key inside a protected variable, that means we can change them and come replace them at will!  Let's say you have a Photo Album, well Photo Albums normally contain Album Data then have related DB table to store photos, so that requires 2 models? Nah just requires the darkside....

Anyway  here's the situtation I have a DB table originally enough named "albums" and another very originally named "photos"  and I don't want to load to models to do the same thing, so let's just make one model do it all, by adding one simple method to your Modules Model, I'm not very good at explaining this stuff so here's some code and use the source luke!

~~~ php
<?php

    protected $table = 'albums';
    protected $key   = 'id';
    
    /*
     * @var string The other table.
     */
    protected $n_to_n_table = 'photos'
    
    /*
     * @var string The other table the other key.
     */
    protected $n_to_n_key = 'photo_id';
    
    
    
    // Changes Table and Key to Other Stuff
    public function set_table ($table = TRUE)
    {
      // Check 
      if ( $table === TRUE )
      {
        // Personally I just hard code these back to make sure there right.
        $this->table = 'albums';
        $this->key   = 'id';
      } else {
        // Since you called this method with false, set the other table.
        $this->table = $this->n_to_n_table;
        $this->key   = $this->n_to_n_key;
      }
    
      // Make it method chain-able all the cool kids are doing it.
      return $this;
    }
    
    // This is just a quick example of finding all the photos
    // Use your imagination and you can see this could be useful.
    public function find_photos()
    {
      // Ok simple usage, set_tables to Other DB table, Find All then Set Back.
      return $this->set_table(FALSE)->find_all()->set_table(TRUE);
    }

?>    
~~~

Ok I hope the code was enough to explain what I'm doing with my table madness,
another usage of adding some extra methods could be for adding joins, likes, etc into your model.

Here's a example of setting the LIKE clause since MY_Model doesn't have a LIKE in it yet.


    
    
    
~~~ php

<?php
            
    /**
     * Sets the LIKE section of the DB Query to whatever the param is set to.    
     * 
     * @param string $field Field to search in. 
     * @param string $value Value to search for.
     * @param string $how   Where to put the % things, before, after, both.
     *
     * @return  MY_Model|$this
     */
    public function set_likes($field = '', $value = '', $how = 'before')
    {
        // This is just a error prevention thing I do personally.
       if ($field == '' || $value == '')
       {
          return $this;
       }
       
       $this->db->like($field, $value, $how);
       return $this;
    }

?>

~~~    


The above method just Set's the LIKE SQL clause in a method chainable format.

Anyway that's enough ramblings hope someone learned something from it. 

BTW I do drink Guinness more Guinness = more articles.

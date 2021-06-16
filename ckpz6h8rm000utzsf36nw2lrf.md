## CodeIgniter - Bonfire Series - Permissions in the Front Controller!


Here's a quick tip that I learned recently,  since .6 has the user object in all the Base Controllers,  The user Auth library is always loaded.  Since it is loaded,  you can use the permissions system in the Front_Controller thus allowing very cool things such as front-end editing of content based on Permissions!  To me I've always wanted to allow users to edit/make changes to there content without logging into the backend so this was a god send.  

It's not hard and pretty easily, let's take a look at the basics, first in my Constructor I check if they are logged in and have permissions, if so I give them some extra javascript, like so.


~~~ php
<?php
  /**
   * Simple controller example, showing how to add assets if they have the permissions
   * 
   */
  public function __construct()
  {
    parent::__construct();

    if (has_permission('Articles.Content.Edit') )
    {
      Assets::add_module_js('articles', 'jeditable.min.js');
      Assets::add_module_js('articles', 'articles_admin_frontend.js');
    }
      
  }//end __construct()    
?>
~~~


So by checking if they have the permission available, I give them extra javascript to do cool stuff.  Now, what about the dangers of someone posting to the site without the permission ?  Well that's were the next cool feature comes in at.  Let's look at my ajax_content method.
   

~~~
<?php
    public function ajax_articles ( )
    {
      //! If they do not have this Permission then they are sent to a login form!
      $this->auth->restrict('Articles.Content.Edit');
    
      //This is just some general cleanup of my posted values.
      $id    = (int) $this->input->post('id');
      $value = $this->input->post('value');
      $type  = strtolower ( $this->input->post('type') );
    
      if ( strtolower ( $type ) == 'title' )
      {
        //Just to make sure my submit was cleaned properly.
        $value = trim ( strip_tags( $value ) );
    
        //Since I have a few different areas of edit-able content, I have a couple setter methods.
        $this->articles_model->set_page_title( $id, $value );
    
        //This is just to show the edited content, I always use the output lib for caching, but you could echo it.
        $this->output->set_output( $value );
      }
    }
?>    
~~~



Anyway I know this is just a short trick, but I figured some of the newer users who didn't know the permissions system would like to know that you don't need to use the Auth_Controller to check for permissions,  if you want to see an actual demo of editing the content using this method,  make some noise, post some comments, and I might put it up in a public repo for you!

Here's one a bonus trick I had to use to make this work perfectly,  I needed to load my editor from the admin theme, from the default theme.  The template library does have a nifty little feature for getting theme url's but only for the current one.  Try  this out if you need to a resource in a different theme directory.


~~~ php
<?php
  //! Get the current theme and save it.
  $theme = rtrim (Template::theme() , '/');

  //! Switch over to the Admin theme used.
  Template::set_theme('admin');

  //! Load up my assets, this is just one of the ones I use but you get the idea.
  Assets::add_js(Template::theme_url('js/editors/markitup/sets/markdown/showdown.js'));

  //! Switch back to the current theme.
  Template::set_theme($theme);
?>
~~~

There's your extra bonus tip for reading these dam things!

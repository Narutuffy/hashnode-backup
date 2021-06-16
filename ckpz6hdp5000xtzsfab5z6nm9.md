## CodeIgniter - Bonfire - Dynamic Routes sorta


Here's another quick tip and some short functions that I've collected from various sources awhile ago and added my own Bonfire spin to them as I use them.  I would cite the original sources if I knew who they were,  if some of this code looks like yours point it out to me and I'll add link or something sorry I forgot the original authors of this code. 

Anyway,  one common issue that confuses the hell out of newer codeigniter user's is also one of the best parts of using CodeIgniter to start with the routes file.  And one common task anyone that develops lots of website's fast is a need for a dynamic pages system.  I've built a few of them with the craziest one using a complete database table of slugs and using php's mysql functions inside of my routes file to query the database for the routes,  this method actually creates a routes file in the app/cache directory which works out pretty well.  I use it on a few of my sites that just need a quick pages system.  Anyway I've basically just built 2 methods ( functions for the people that still haven't figured out the difference between a method and a function ) one is recursive meaning it calls itself, and the other just creates the routes as a array and writes them to a file.

Anyway, my database fields are probably different then yours unless your using one of my modules :P So the key database fields are "parent_id, id, slug, active"

I'm currently using this in a CMS I wrote and need to expand on it, but anyway here's the code snippet's before I lose them.


    
    
~~~ php
<?php
    /**
     * Helper function to generate routes file which is saved in application/cache/routes.php
     *
     * Make sure you include this file in your application/config/routes.php file.
     */
    public function save_routes()
    {
    	// this simply returns all the pages from my database
    	$routes = $this->articles_model->find_all_by('active', 1);
    	Console::log ( 'Save routes method activited.....');
    	$data   = array();
    
    	// write out the PHP array to the file with help from the file helper
    	if ( !empty( $routes ) )
    	{
    		$route_file = APPPATH . 'cache/routes.php';
    
    
    		// for every page in the database, get the route using the recursive function - _get_route()
    		foreach( $routes as $route )
    		{
    			$data[] = '$route["' . $this->_get_route($route->id) . '"] = "' . "articles/{$route->slug}" . '";';
    		}
    
    		$data = $load->helper('file');
    		}
    
    		write_file( $route_file , $data);
    	}
    }//end save_routes()
    
    //--------------------------------------------------------------------
    
    /**
     * Recurisive function to go through and make the actual routes based on database functions.
     *
     * @todo   Figure out a method to do this that's easier on the database.
     *
     * @param  $id     id field to make route for.
     * @return string  human friendly route to goto.
     */
    private function _get_route($id)
    {
    	// get the page from the db using it's id
    	$page = $this->articles_model->select('parent_id, slug')->where('active', 1)->find($id);
    
    	// if this page has a parent, prefix it with the URL of the parent -- RECURSIVE
    	if($page->parent_id != 0)
    	{
    		$prefix = $this->_get_route($page->parent_id).'/' . $page->slug;
    	} else {
    		$prefix = $page->slug;
    	}
    
    	return $prefix;
    }//end _get_route()
    
?>    
~~~


So I just stuck them in my Articles Content controller for now, and after editing or adding a article I call the save routes method like.


    
~~~
<?php
      $this->save_routes();
?>
~~~


Now this could be very easily expanded on but this is just a quick fix for a issue I was having with the routes.

If you have a better solution I'd love to hear about it.

## CodeIgniter - Bonfire Series - Template Blocks





# CodeIgniter - Bonfire Series - Using Template Blocks


This is a basic guide to using the Template Block methods,  in Bonfire's Template class, you have Block's which I personally use for things 
like Sidebar Navigation, Module Views on Certain Pages, etc, etc.  There's been a little confusion on the proper usage of these so I figured I'd write a quick short guide on how I personally use them in my workflow. 

### Step 1 - Setting the Block in your Theme

Ok Template Blocks, are like view partial's that can be changed inside of your Controller Method's,  the best part of Block's is that you can set a default Block view file, or just set a spot for the block to and leave the default empty so they only appear when needed.  


In this guide we will be using the **default** Template directory, and the **Home**Controller so these files are located on a default Bonfire installation at
  	
  * /bonfire/themes/default/index.php <- Main Template Files 	
  * /bonfire/application/controllers/home.php <- Default Home Controller

 Let's start with creating 2 files. **block1.php** and **block2.php** Both of these files will go in your template directory which is **/bonfire/themes/default/**


###  Let's Start with the Home Controller 

Ok that's the main default home front-end controller,  We have 2 method's inside of it to change Block's, now inside of your Template Index file which is located at /bonfire/themes/default/index.php,  Lets just add a setting to create a space for these blocks,  I am using the Default Template File which I just pulled from Github.
  
~~~ php
    <html lang="en">
    <head>
    	<meta charset="UTF-8"></meta>
    
    	<title></title>
    
    	<link href="/favicon.ico" type="image/x-icon" rel="shortcut icon">
    
    	
    
    	
    </head>
    <body>
    
    	<div class="page">
    
    		
    		<div class="head text-right">
    
    			<h1>Bonfire</h1>
    		</div>
    
    /*
    	Only change I made is to add a Call for the New block here, I did not provide a default block view so only controllers that set a block view will show a block view.
    */
    
    		<div class="main">
            <?php
                    $input->cookie($this->config->item('sess_cookie_name')));
    				$logged_in = isset ($cookie['logged_in']);
    				unset ($cookie);
    
    				if ($logged_in) : ?>
    			<div class="profile">
    				<a href="<?php echo site_url();?>">Home</a> |
    				<a href="<?php echo site_url('users/profile');?>">Edit Profile</a> |
    				<a href="<?php echo site_url('logout');?>">Logout</a>
    			</div>
    			
    
    			
    			
    
    		</div>	
    	</div>	
    
    	<div class="foot">
    		
    			<p style="float: right; margin-right: 80px;">Page rendered in {elapsed_time} seconds, using {memory_usage}.</p>
    		
    
    		<p>Powered by <a href="http://cibonfire.com" target="_blank">Bonfire </a></p>
    	</div>
    
    	<div id="debug"></div>
    
    	<script>
    		head.js(<?php echo Assets::external_js(null, true) ?>);
    		head.js(<?php echo Assets::module_js(true) ?>);
    	</script>
    	
    </body>
    </html>
    
~~~    

I know this is a simple topic, but it seems to have caused some confusion lately so i'm hoping this quick short guide will help people understand how the blocks are set and work.  There usage is amazing, I personally use them for my Template head, footer, sidebar, navbar, and whatever else I need for my blocks.  As always please check the bonfire doc's for more information on there usage.





## CodeIgniter - Bonfire Series - AJAX Form Submission with JSON Results



This short article will show how to make a Ajax form post, while returning a JSON value that replaces a Image SRC with a QR Code made from Google Charts

Since the theory behind this tuturail, is more replacing a image tag on a AJAX Forum Submit, we will be using a Library already created  

You can download it at [Github](https://github.com/eric-famiglietti/gc-qrcode), I didn't create the library but its useful, I originally found it as a Spark but installing Sparks into Bonfire is beyond the scope of this article so just download the library and stick it in your bonfire/application/librarys/ directory for now

Ok Create a Basic Module using the Module Builder, you shouldn't need a database table for this, personally I just suggest creating a Module with a public context, if you wanna add to it later you can but the focus of this article is binding jQuery Form Submission with Creating the 
URL to the New Image and Replacing the Source of the Original Image.

Ok, you have your basic Front_End Controller, Let's take a look at mine and break it down to make sense.


~~~ php
<?php
    public function __construct()
    {
    	load->library('form_validation');
    	$this->form_validation->CI =& $this;

    	//Setup Google Charts Config URL
    	$this->g_config['google_charts_base_url'] = 'https://chart.googleapis.com/chart';
    	//Load GR Code Library
    	$this->load->library('Gc_qrcode', $this->g_config);
    	//Set a Variable for a Starting QR Code to be Generated            
    	$this->baseurl = 'http://blog.shawnc.org/';
    }

    //--------------------------------------------------------------------

	/*
	    method : index()

	    Base Method for Non AJAX Calls and for Inital Display of the Form
	*/
	public function index()
	{                        

		$url = $this->baseurl;

		// Check if input post is submitted, if so set URL to new URL

		if ( $this->input->post('submit') )
		{
	    $url = $this->input->post('linkto');
		}

		$this->gc_qrcode->size(250)->data( $url );

		$img = $this->gc_qrcode->img(array('id' => 'qrimg'));

		// Load the Inline JavaScript to Bind the Submit Handler to Post a AJAX Call to The AJAX Method
		Assets::add_js( $this->load->view('qrjs', null, true), 'inline');
		// Set Template Variable for Image
		Template::set('img', $img );
		// Render Template
		Template::render();
	}

	//--------------------------------------------------------------------

	/*
		method : ajax_qr()

		AJAX Method to Return the JSON Value for the URL of the New Image Tag
		Can be Secured with a Check if IS_AJAX but I'm not gonna add that to the scope of this tut
	*/
	public function ajax_qr()
	{
    $url = $this->baseurl;
    if ( $this->input->post('linkto') )
    {
      $url = $this->input->post('linkto');
    }
    // Create New QR Code
    $this->gc_qrcode->size(250)->data( $url );
    // Get URL to New QR Code
    $img = $this->gc_qrcode->url();
    // Set Output as JSON and Output the Content as JSON array
    $this->output->set_content_type('application/json')->set_output(json_encode(array('img' => $img )));

    return false;
  }

}
/* End file bonfire/modules/makeqr/controllers/makeqr.php */
?>
~~~

Ok that's the main front-end controller,  It would be located in bonfire/modules/makeqr/controllers/makeqr.php

Now we need to create the view's and javascript for this,  I personally use Twitter's Bootstrap V2 on my site's so your gonna get my view's
You should be able to easily figure out how to modify it to fit your site or if your already using Bootstrap then away you go.

Ok first, inside of modules/makeqr/views/index.php, let's replace the file with this one.

~~~ php

<section id="qrcodes">
<div class="page-header">
  <h1>QR-Code <small> Make your own QR Code by filling out the form!</small></h1>
</div>


<div class="row-fluid">
<div class="span12">
  <div class="alert alert-error fade in">
	    <a data-dismiss="alert" class="close">×</a>
      
  </div>
</div>
</div>


<div class="row-fluid">
  <div class="span4">
        
  </div>
  
  <div class="span6">
	uri->uri_string(), 'class="form-horizontal" id="makeqr"'); ?>
  
	<div class="control-group">
		<label class="control-label" for="linkto">Website URL : </label>
		<div class="controls">
			<input name="linkto" type="text" value="<?php echo isset($makeqr) ? $makeqr->linkto : set_value('linkto') ?>" class="span6" placeholder="http://shawnc.org" id="linkto" tabindex="1"></input>
		</div>
	</div>

	<div class="control-group">
		<label class="control-label" for="submit"> </label>
		<div class="controls">
		<input id="submit" type="submit" class="btn btn-success" value="Create QR-Code" name="submit"></input>
		</div>
	</div>
  
	</form>
  
  </div>  
</div>

~~~


Ok so that's the basic form for the QR Code Creation, this can be done with any library or any form, I'm just using something I already made for a personal site,  just remember the ideas and theory's behind this are not restricted to this one application.
So all that's left now is to include the actual JS file that makes the magic happen.  One thing You'll need to find a Loading Image for the
AJAX image loading,  there's ton's of placing to get them at, I personally just used the same one I use on my lightboxes.

Anyway the last piece needed is the javascript, which we loaded as a view since I wanted to include some PHP in it, Here it is

~~~ javascript
 //Form ID is set to "makeqr"
 //The URL Value is IDed as "linkto"
 //The Image to Replace is IDed as "qrimg"

    $("#makeqr").submit(function()
    {
        var dataString = $("#linkto").val();
        var img_rep    = $("#qrimg");
        img_rep.attr('src', 'assets/images/loading.gif');
        $.post(  
                "makeqr/ajax_qr",
                {linkto: dataString },  
                function(data){
                   var img = data['img'];
                   img_rep.attr('src',img);
                },  
                "json"  
               );          
        return false;  //stop the actual form post !important!
    });
~~~


If you want to see a Demo, I will post a link to it shortly,  currently my working copy is under my Auth Controller so you have to login to view it but if enough people ask I'll post a demo for you,  I hope this was a informative article on using jQuery, Bonfire, CodeIgniter and AJAX all in one simple tut.


## Image Preloading with jQuery


Recently, I had to make some modifications to a site that was designed and built back in 2004 when you required images to make things look nice,
I noticed that there were way to many images being loaded due to hover effects, rounded corners, etc, etc, etc. Which caused the site to load
slowly. I personally am of the school that if the web site does not open within 5 seconds, I'm already looking for another website. So to help
with this problem I went ahead and wrote a small image preloader with jQuery. Since this website was built along time ago, I did not remember
the id's or classes of the images that were being loaded so I created a reusable jQuery function that is reusable to load images as I found them.
Save the script below as jquery.preloadImages.js
And remember to include it before you call the function.


~~~ javascript

/**
 *
 *	Preloads Images using jQuery, Can load unlimited amount of images to Cache.
 *
 * Usage is as followed
 *
 *  jQuery.preloadImages("image1.jpg", "image2.jpg",…,"image7.jpg");
 *
 *	Since this function accepts unlimited arguments you can use it again and again no matter how many images you need to preload.
 *
 *	
 * Copyright (c) 2011 Shawn Crigger (http://s-vizion.com)
 * Licensed under the MIT License (LICENSE.txt). 
 *	Created by Shawn Crigger <http://shawnc.org>
 *
 **/
(function ($)
 {
  var cachedimages = [];
  $.preloadImages = function()
  {
    var i   = 0;
    var num = arguments.length;
    for ( i = num; i--;)          
    {
      var thisImage = document.createElement('img');
      thisImage.src=arguments[i];
      cachedimages.push(thisImage);
    }
  }
})(jQuery)

~~~

Basicly, to call this function just copy paste the following code and save the above file.

~~~ javascript
<script type="text/javascript">
 
  $(window).ready(function()
  {
    jQuery.preloadImages("image1.jpg", "image2.jpg","image7.jpg");
  }
 
</script>
~~~

## jQuery Basics - Simple Image Roll Overs


I've been writing JavaScript for along time, I've used many different JavaScript libraries and even wrote code before libraries became popular. Anyway, after going from Prototype to using the YUI Library to jQuery, I'm hooked on it. With YUI! you can not create non-obtrusive code, everything has to be complicated. With jQuery everything is simple, the API Documents are excellent and it's basically a write less and do more library.

Ok, enough about how great this library is, I've decided to write a few articles on using jQuery to do basic things every web page requires. In this chapter, I will teach you how to make image rollovers very simple by just locating a class and renaming the Image Source to the Hover Source and then Reversing the Process.
Let's start out with the code and then I'll try to expain it afterwards.

~~~ javascript

/**
 *
 * Basic Image Roll Over 
 *	
 * Copyright (c) 2011 Shawn Crigger (http://s-vizion.com)
 * Licensed under the MIT License (LICENSE.txt). 
 * Created by Shawn Crigger <shawn@s-vizion.com>
 *
 **/
 
  $("img.rollover").hover(function()
    {
      this.src = this.src.replace(".jpg","_ov.jpg");
    },
    function()
    { this.src = this.src.replace("_ov.jpg",".jpg"); }
  );

~~~

Now let me try to explain what happens here.
$("img.rollover") Will Select All Images that Have the Class "rollover"
Next we set a Event Handler for the Hover property of the Element we just Selected and Create 2 Functions for it.
The First Function Replaces the ".jpg" of the Image Source Tag with "_ov.jpg"
The Second Function Reverses the First Function.
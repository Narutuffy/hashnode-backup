## A Simple Jquery Lightbox for displaying anything



This is a simple light box script I wrote that will take the target URL and open it
with Jquery's Native Dialog Window features. To use this script you will need to have
jQuery 1.4 or above and the jQuery UI Scripts and CSS Files.

You can download them at jqueryui.com.

To use this script, simply add the class ".sbox" to your a href and it will pull them
link into the lightbox. If javascript is off, it will open will target blank.

Please use the title attribute on your a href so that the lightbox can select a title

You will also need to find a loading gif. There's lot's of places to get them at so
I'm sure you will be able to find one and figure out the css fairly easy.

~~~ javascript
/**
 *
 *	Functions for the Dialog Windows
 * Copyright (c) 2011 Shawn Crigger (http://s-vizion.com)
 * Licensed under the MIT License (LICENSE.txt). 
 *	Created by Shawn Crigger <shawn@s-vizion.com>@ EWD 1-15-11
 *
 **/
$(function() {
    var b = $('<img src="http://yoursitename.com/images/loading.gif" alt="loading" class="loading">');
    if ($("a.sbox").length != 0) {
        $("a.sbox").each(function() {
            $(this).css("display", "visible");
            var d = $("<div></div>").append(b.clone());
            var a = $(this).one("click", function() {
                d.load(a.attr("href")).dialog({
                    title: a.attr("title"),
                    width: 400,
                    height: 400,
                    modal: true,
                    position: top,
                    draggable: false,
                    resizable: false,
                    buttons: {
                        Close: function() {
                            $(this).dialog("close");
                        }
                    }
                });
                a.click(function() {
                    d.dialog("open");
                    return false;
                });
                return false;
            });
        });
    }
});
~~~
I Hope this script help's somebody out there. If anyone needs help understanding it
then please ask and I will write a more detailed post about what each section does.

This script will pull in Images, PHP Files, HTML, pretty much anything. I use it one
all my projects and ask that you please keep the header comment with the script.
For those that wanted a simple demo, I forgot to write one for this site, but take a look at

[Demo](http://rarespeciesfund.org/)
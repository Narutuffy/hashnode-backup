## jQuery Character Counter that works with Twitter Bootstrap 2.0 Form markup


With Twitter's Bootstrap Framework being the most popular framework to come out in awhile, I find myself using parts of it in a lot of web sites and apps I build these days. I needed a simple character counter that would let the user know how many character's they had left, I found one already created that with some minor changes worked excellent. All credits go to the original author Alen Grakalic, his plug-in fit the bill perfectly and very few changes were needed to make this work.
Let's start with a simple Bootstrap styled form input, then move on to the jQuery plug-in and how to use it.
```html

  <div class="control-group">
    <label class="control-label" for="meta_title">SEO Page Title</label>
    <div class="controls">
      <input id="meta_title" class="span6" type="text" name="meta_title" maxlength="200" value="<?php echo set_value('meta_title', isset($articles['meta_title']) ? $articles['meta_title'] : ''); ?>"  />
			<p class="help-inline"><span class="counter"></span>
			<p class="help-block"><strong>Note: Please try to keep this under 72 characters long</strong><br /> This is the title that will be show in your browser's title bar and also in your Search Engine results.</p>
    </div>
  </div>
```

Ok this came straight out of one of my CRUD pages for a article system. Anyway, let's get to the point shall we.
First I added a Inline Help block to store the Current Character count-down and the Help block to explain why I'm counting character's.
Next we need some Java-Script to make this happen, so you can either grab the plugin from his site or just copy mine since I already made the changes.

``` javascript

/*
 * 	Character Count Plugin - jQuery plugin
 * 	Dynamic character count for text areas and input fields
 *	 written by Alen Grakalic
 *	 http://cssglobe.com/
 *
 *
 * Updated to work with Twitter Bootstrap 2.0 styles by Shawn Crigger <@svizion>
 * http://blog.s-vizion.com
 * Mar-13-2011
 *
 *	Copyright (c) 2009 Alen Grakalic (http://cssglobe.com)
 *	Dual licensed under the MIT (MIT-LICENSE.txt)
 *	and GPL (GPL-LICENSE.txt) licenses.
 *
 *	Built for jQuery library
 *	http://jquery.com
 *
 *
 */

(function($) {

	$.fn.charCount = function(options){

		// default configuration properties
		var defaults = {
			allowed: 140,
			warning: 25,
			css: 'help-inline',
			counterElement: 'span',
			cssWarning: '',
			cssExceeded: 'error',
			counterText: ''
		};

		var options = $.extend(defaults, options);

		function calculate(obj){
			var count = $(obj).val().length;
			var available = options.allowed - count;
			if(available <= options.warning && available >= 0){
//Since the error/warning field is actually in the first div, we jump 2 parent classes up to find it.
				$(obj).next().parent().parent().addClass(options.cssWarning);
			} else {
				$(obj).next().parent().parent().removeClass(options.cssWarning);
			}
			if(available < 0){
				$(obj).next().parent().parent().addClass(options.cssExceeded);
			} else {
				$(obj).next().parent().parent().removeClass(options.cssExceeded);
			}
			$(obj).next().html(options.counterText + available);
		};

		this.each(function() {
			$(this).after('<'+ options.counterElement +' class="' + options.css + '">'+ options.counterText +'</'+ options.counterElement +'>');
			calculate(this);
			$(this).keyup(function(){calculate(this)});
			$(this).change(function(){calculate(this)});
		});

	};

})(jQuery);

```

Hopefully you can spot out the changes, there's really only a couple changes needed. Now to use just add include the js file and add the following call to activate it.

```javascript
	$("#meta_title").charCount({
	    allowed: 72,
	    warning: 68,
	    counterText: 'Characters left: '
	});
```

And there you have it a properly styled Character counter using jQuery and Twitter Bootstrap 2.0 CSS.
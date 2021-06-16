## How to check if plugin is loaded in jQuery NEW


This is a short simple quick tip,  I re-use a lot of my code, ( it's called going green ) anyway sometimes I forget to remove some common jQuery plug-in call's that I use a lot like Lightboxes, Choosen Selects, jEditable, etc, etc.  So I find just adding a quick check to make sure the method exists before calling it is very helpful.

One way of checking if a method exists is like this

Visualize your output and data while you code to check that your code works as planned.

![Promo notebook](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623830736693/QKrfW5Z7R.png)


    
~~~ javascript
  if (typeof $.fn.pluginname != 'undefined')
~~~
    
Or the short-hand version

~~~ javascript
  if($.fn.pluginname)
~~~

Just replace "pluginname" with whatever your calling like below

~~~ javascript
  if($.fn.pluginname)
~~~

~~~ javascript
    // Verify there is a method named "chosen", then verify we need to use it.
    if ($(".chzn-select").length > 0 && typeof $.fn.chosen !== 'undefined')
    {
    	$(".chzn-select").chosen();
    }    
~~~
    

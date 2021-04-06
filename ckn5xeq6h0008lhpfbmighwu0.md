## Adding the Yahoo YUI! Treemenu Module to ModX CMS System



~~~ javascript

  ///--------------------------------------------------------------------------
  //  Installing a Yahoo YUI! Treemenu into ModX CMS Systems
  //  by **ShawnC** of http://www.shawnc.org
  //--------------------------------------------------------------------------

~~~

    
## Introduction



 Along time ago, I wrote a method of having a Java-script Tree menu in ModX CMS using a script from Dynamic Drive.  This modification involved modifying core ModX CMS php
files and It just wasn't a great method of going about this.   Now I have found a much better method of doing this using the excellent Yahoo YUI! Library.  This Library
is very small in Size and is one of the best Java-Script Library's released at this time.  Its modular nature is the best thing going for it honestly.  Well since I wrote a
Plug in for vBulletin a few weeks ago to use this System, I decided to go ahead and port the modification over to ModX as it is my favorite frame work for a CMS.  So Let's
Get Started on how to Do It!   At the end of this Post, you will find a Download Link to the Files needed to Install this modification in your System.



## The Basics


  Ok this module will basically, offer visitors with java script a Excellent Opening and Closing Tree menu to Navigate your Site very Rapidly.  Visitors without Java-script
will not get to see the module, but I have made it so that Search Engine Spiders will still get a excellent Un-Ordered List of your Pages, so for SEO this is a Good
Modification allowing Spiders to have 1 Click access to any page from any page!



## Let's Get Started!


  Ok, Go Ahead and Download the Modification Package At the bottom of this post and Unzip it into a Directory you can find.  Also Open up a FTP Program and Navigate to your
sites ModX Root Folder ( the one that has the Assets, Manager, Index files/folders in it ).  Go ahead and Upload the UPLOAD directory to this location and click overwrite all
if it asks you for that.

Ok So Now All the Java-Script, CSS and Image Files are Uploaded on your Server in the Assets Directory



## Editing Your Head Section of the Template


  Ok in the Header Section of your Page Templates you want to View this Modification on Add the Following Lines inside of your  )

NOTE : One Tip I Noticed, I Find it wise to include your Sites URL in the links such as this, It will help solve any errors later on the installation 


~~~ html

  <link media="screen" href="http://www.you.com/assets/yui/assets/tree.css" type="text/css" rel="stylesheet"></link> 

~~~

Above was just a example of what you should change, Below you need to copy/paste into your Template Above the Statement,  We need to add our Javascript Function to make the Magic Happen It Looks like This
What this does, is Take your existing UnOrder List and turn it into a JavaScript Tree.  It also Hides the Container Div until After the Tree has been rendered so
that it looks better.


    
    
~~~ javascript

    var ultree;
    (function() {
    
        function treeInit() {
            ultree = new YAHOO.widget.TreeView("treeDiv2");
    
           ultree.setExpandAnim(YAHOO.widget.TVAnim.FADE_IN);
           ultree.setCollapseAnim(YAHOO.widget.TVAnim.FADE_OUT);      
           ultree.readList();
    
           ultree.subscribe("expand", function(node) {
               });
    
           ultree.subscribe("collapse", function(node) {
               });
    
           ultree.subscribe("labelClick", function(node) {
               });
    
    
            ultree.draw();
            YAHOO.util.Dom.get("NavDiv").style.visibility = 'visible';
        }
    
        YAHOO.util.Event.addListener(window, "load", treeInit);
    
    })();

~~~


## Conclusion


	Well I hope this makes sense to some people on the usage of the Yahoo YUI! Treeview module.   If you have any problems using this code please comment at my blog
and you will recive support much faster then anywhere else as I check it once a day whereas I check other forums on a maybe once a week basis.


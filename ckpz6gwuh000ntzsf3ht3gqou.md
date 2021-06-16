## How to Add a Expandable Tree Menu to ModX CMS


This is a older document i wrote last winter.  I reposted it here just incase people were looking for it still.  I will also post the update with the yahoo yui tree menu very soon.



# Expandable Tree Menu for MODx




* * *

Well i just started playing with modx a week ago or so and plan on
using it for my personal Web Development/Freelance Site. Ive always
liked a tree menu for my site nav menu and figured that it would be
easy to add to modx with just a minor change to the wayfinder php file.
Anyway i used the **Dynamic Drive simple tree menu** to do this.  

  
-- [http://www.dynamicdrive.com/dynamicindex1/navigate1.htm](http://www.dynamicdrive.com/dynamicindex1/navigate1.htm)  

  
Visit the above link and download the Javascript file, the css file and the 3
images. Include the java and css file as you would in the example they
give you, then do the following to display the menu.  
   
Using this menu system requires a ID to be added to the root UL which i
discovered wayfinder could not do so i modifyed wayfinder to do this. 
Please note i used wayfinder 1.0   

### Step #1

* * *

-> Open Manager->Resources->Snippets->Wayfinder  

  

Add this line under the other lines that look like it  

    
~~~ php   
	css[rootID] = isset($rootID)? $rootID: ;
?>
~~~


### Step #2

* * *

-> In your text editor open the wayfinder.php file   
( located in **assets/snippets/wayfinder/wayfinder.inc.php** )  

  
Search for this line ( around line # 189 )  


Now directly under it add  

~~~ php

		css[rootID] . ";              
	}
?>
~~~    

### Step #3

Now your call to Wayfinder will look like this  

~~~ javascript
      <script type="text/javascript">
      ddtreemenu.createTree("treemenu", true, 5)
      </script>
~~~

And there you have it a working tree menu.  

  

You can see a sample on my unfinished site at [www.earthbeans.org](/)  

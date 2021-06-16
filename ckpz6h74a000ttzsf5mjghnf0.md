## CodeIgniter Bonfire Quick Tips Permissions


Let's go over the basic idea, first.  Let's say you have a Pages Module that handles your Content,  I use One Module for all my Pages personally and some pages you need to be logged in to view.  Now since I've already written about using permission's this is just a short fast tip.

So basically the idea is we want to add the ability to flag some pages as Login Required pages, not all of them, just some of them.  Well that's actually easier then chasing unicorns while taking LSD trust me on that, dam unicorns evil little bastard they are,  one of these days the world will be unicorn free.

Now,  What I personally did to make this easy was simple,  First I added a simple TINYINT checkbox to my database for my pages,  that's up to you to figure out,  I named mine private you can name yours whatever.  But let's just go with a TINYINT(1) called "private" for the sake of sanity.

Inside of your remap, or whatever method your using to fetch, display this page data, all you need to add is

~~~ php
<?php
  //This is your stuff here
  $page = $this->your_model->find($id);

  if (isset($page->private) && $page->private > 0)
  {
    $this->auth->restrict();
  }
?>
~~~


This code is 100% positive fo sure not gonna drop and fit in your pages module or whatever you need,  there's even a purpose bug in there just for you to find and fix.  But There's only one line you need to make this happen.




~~~ php
<?php
  //! Check your database variable that your page is not private.
  if ($page->private > 0)
  {
      //! This is the Key, by using restrict we are forcing the user to
      //! be logged in, if they are no they are re-directed to a login page.
      $this->auth->restrict();
  }
?>
~~~




Now this could be expanded on using your Coding Jedi Powers and Use the Source, or reading the manual, or even reading more of my blog where I explain permissions a bit more.

One last quick little snippet for you

~~~ php
<?php
  // This checks for permissions but does not restrict them.
  $this->auth->has_permission('Unicorns.OurMagic.CodeIsReal');

  // This is a short cut to that long a$$ line good for views.
  has_permission('Use.The.Source.Luke');
?>
~~~

Ok there's your 2 quick tips for tonight.  Now go do something useful like I donnu, use your Jedi Coding Powers for Good and Stop Staring at Goats damit!

Still bloggin like a boss

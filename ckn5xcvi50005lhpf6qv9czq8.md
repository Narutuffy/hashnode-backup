## CodeIgniter Bonfire - Code completion in Komodo and other IDE's


I personally use Komodo as my main IDE of choice, and one thing that annoyed the living hell outta me was whenever I tried to type 

    
~~~ php 
     $this->load->
~~~
        
It got converted into $this->null-> which got very annoying, so anyway after doing a bunch of research I found that if you added the property's used in PHPDoc format to MY_Controller you would get Code Completion and even Call-tips!  So below is how my MY_Controller looks.

{% gist:3158399 %}    
Most of the original properties from this file were found on a gist on github, I'm not sure the original author, but I did add the Bonfire specific ones in it.

By adding that doc block above the main base class, and setting the directory for code completion, bam done fixed, no more $this->null anymore!

Just a quick tip for those of us wanting to improve our workflows.

EDIT : Mar 29, 2012 

I just updated this to include more of Bonfire's base Libraries, Models, Controllers, etc.  I've also switched over to using phpStorm which provides the Best Call-tip's and Debugging help I've ever seen in a IDE. 

EDIT : Apr 20, 2012 

Renamed CI_Language to CI_Lang to match the current version's of CodeIgniter.

EDIT : May 5, 2012

Something I've been doing with my own modules as of late, is adding Class level Docblock's and including my property's that I use in that Class,  I wrote example in the comments below, but basically I just add the Library's I'm gonna use in the Class Level DocBlock that way if I re-use the code somewhere else like another site, or give it to Joe or whatever 

1) I get auto completion auto-magically
2) Whoever Works on this Class later in life can say O wait it's not working because I don't have the foo_model or the bar_lib

It's one thing to write re-usable code, another to document your code but it's the best to have re-usable code that anyone can understand.

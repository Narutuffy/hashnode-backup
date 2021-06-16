## Over-riding CodeIgniter Bonfire Core Modules and Making it easy to update later


One thing that I require a lot in building Bonfire based Websites is to modify mostly views in some of the Core Modules, namely the User Module since that has Public facing View files and not all my Websites are built using Twitter Bootstrap's framework depending on the Front-end Developer I'm working with on the project with.

This is a really simple trick I learned not to long ago but makes upgrading very easy later on which with a constantly changing software like [Bonfire](http://cibonfire.com) then Over-riding my Core modules and keeping track of the changes gets hard namely when you build as many website's as I do in a year.  This is a very very very simple trick and you'll probably have a Facepalm Moment for not thinking of this yourself after seeing what it involves!


Basically modules in Bonfire exist in 2 locations

 - bonfire/application/core_modules
 - bonfire/modules

 Which the Core Bonfire related modules live in core_modules and custom modules belong in the modules directory.  So the simple trick here is just copy your core_module into the modules directory like this.


 	cp -rfv /bonfire/application/core_modules/users /bonfire/modules


Or you can drag and drop it using Finder or whatever you do to copy a folder to a new location.  So now yoo have 2 copys of the same module, you can actually delete the one your over-riding but I perfer to just leave it there myself.

So what have we learned here, the order modules are loaded is first "core_modules", then "modules" so anything in the modules directory will over-ride the "core_modules" directory.  Simple enough?

Hope this helps someone.  Enjoy and Cheers!




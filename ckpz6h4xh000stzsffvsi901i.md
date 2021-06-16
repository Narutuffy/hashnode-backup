## using flickr api explorer to fetch all your photo ids and names


So the switch from clunky old wordpress is moving right along now and bam, I hit a snag.  I decided to host all my photos on FlickR since they offer a decent photo service and there's plenty of easy to use gems so hey why not right?

Well since I exported about 70% of my posts from Wordpress to Markdown and about 65% of those came out fairly decent,
but all my images were now stuck FlickR in weird numbers and I just wanted to say flickr phto but that would way to easy right?  NAH!  Let's do this smart way please,  I know u can find batch uploader and badbam there up there, just
stick them all in a photoset and let's use FlickR's own API for our own greedy time saving reasons.

I remember awhile ago I wrote a acouple gallerys based on FlickR and they had a great API and a API Explorer/Tester whatever the we want is [flickr.photosets.getPhotos](http://www.flickr.com/services/api/explore/flickr.photosets.getPhotos)

This will give you all your photo ids and names and all kinda goodies that I didn't need but, hey hey extra extra
info never hurt no one so lets what need.

O wow, just stick photoset ID inside the box, I'd set the per to like 1000 so u get them and personally I'd just get
a JSON output then from there it's just finding the images and replacing them with the flickr tag .


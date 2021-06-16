## Ubuntu Gutsy Upgrade - Dual Head Monitors Now Working!


Yes,  after much playing with XORG.CONF in Feisty, and giving up,  I have discovered that with Gutsy you can have Dual Head Monitors working in No Time.  All of this information comes from various sites I have seen on Google, So I can not link back to them all as it took 20-30 sites to finally make this work for me.   Anyway Lets Get This Show on The Road!



## xrandr Is your friend



In the Older X Server, your only chance at getting dual monitors was through heavy modifying your XORG.CONF file, which I try'd afew times and never got more then a cloned desktop using my Intel Chipset.
After the upgrade to Gutsy, I figured I would try again for my Extended Monitor Delights and found that X has given us a new command line tool with Intel's new Driver.  Let me explain how I Set mine up



## Always Backup Conf Files



Ok First, Make a Backup of your working xorg.conf file like


    
    
    sudo cp /etc/X11/xorg.conf /etc/X11/xorg_backup.conf
    





## With Gutsy Comes a new Intel Graphics Driver!



Please Note : The following video card information is for my system, make sure you check your video card make and use the proper driver for your system.

Ok this will give us a backup point just incase.  Now Let's reconfigure our XORG server and use the new Intel driver which i noticed supported my 1440x900 resolution natively and worked much better then the old i810 Driver. So Unplug your External Monitor so you just have your Normal Display on and Reconfigure X.  You must use the Intel Driver instead of the i810 driver


    
    
    sudo dpkg-reconfigure xserver-xorg
    



Ok, now that we have a new XORG.CONF file, let's open it and add Virtual Size to the Display area


    
    
    gksudo gedit /etc/X11/xorg.conf
    



Locate the Display Section and Add the Following 
**Please Note : This is my Screen Settings Do Not Copy Paste the Entire thing, You just need the Virtual Size**


    
    
    Section "Screen"
    	Identifier	"Default Screen"
    	Device		"Intel Corporation Mobile 945GM/GMS, 943/940GML Express Integrated Graphics Controller"
    	Monitor		"Generic Monitor"
    	DefaultDepth	24
    	SubSection "Display"
    		Modes		"1440x900"
    #		Virtual         3048 3048    # Just Add this To Your Display
    		Virtual        2048 2048    # Anymore then This will Break Compiz
    	EndSubSection
    EndSection
    






## Let's Find What Monitors are Being Seen



Now in Terminal Type the Following to Verify your Monitors are there


    
    
    xrandr
    



This will output some very useful information kinda like this

    
    
    
    VGA connected 1440x900+0+0 (normal left inverted right) 327mm x 200mm
       1024x768       60.0
       800x600        60.3
       640x480        59.9
    LVDS connected 1440x900+0+0 (normal left inverted right) 367mm x 230mm
       1440x900       59.9*+
       1280x800       60.0
       1280x768       60.0
       1024x768       60.0
       800x600        60.3
       640x480        59.9
    TV disconnected (normal left inverted right)
    



**Take note of what it calls your Monitors,  Your External is Normally TV or VGA and your Laptop LCD is normally LVDS**




## Let's Make It Work



Now to Try To Make it All Work. That should turn on Both Monitors, The only main problem I found is your External will be your Main Monitor and for me I have to Drag the Top Menu bar back to the other 
screen after I do this.  


    
    
    xrandr --output VGA --right-of LVDS --auto
    



If you get a error from this, then you will have to adjust your Virtual Screensize in XORG.Conf,  Personally my Resolutions were so High I had to go above the 2048 Values and it broke Compiz.

**Please note you will have to change the Names of the Monitors from VGA and LVDS to what names you got from running xrandr**




## The Best Part turning off the Extra Display



Now the Best Part of this is to Turn of this External Display all you need to do is Issue the Following Command


    
    
    xrandr --output VGA --off
    




**Please Note :** I have play'd with this all night and finally decided with my current monitor resolutions I could not use this and keep Compiz Running.  With the 2048x2048 Desktop Limit
but it was fun to play with and if you don't want to use Compiz this is a execellent addition to your desktop!

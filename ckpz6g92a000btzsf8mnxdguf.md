## Tips on Building a Hackintosh



I have been building a Hackintosh for the last few months now to use as my design computer,  In the process of building this computer I have run into a lot of issues.  First let me say the folks at 
[tonymacx86.com](http://tonymacx86.com) have been extremely helpful and have made this process fairly simple compared to the old methods of doing this.  Since there are already excellent tutorials on the actual builds,  I am only gonna give some tips on what to do when you break it, and trust me you will break it many times before you have a fully running Mac.  
  

 - First if you do not understand Terminal, then I suggest you learn some terminal commands.  
 - Second I do not support piracy of Software so just buy the $30 Snow Leopard retail disk. If you can't part with $30 bucks you can't afford to build this system anyway.


## Ok, on to the tips I have learned the hard way.

You should have a retail copy of Snow Leopard and a Disk burned with iBoot already made.  These are you main recovery tools.

Tonymac has created a excellent tool called Multibeast, that will make enabling various things like Networking, Sound, Video, etc... As soon as you have a boot-able Snow Leopard install, I suggest you take a external Hard Drive and use it as a Time Machine.    
  


The Time Machine restore does not always work, so any-time you make changes with Multibeast, you need to back up 2 directories of your file system so that if you changes did not work you can restore back to a working copy.

First open terminal and type the following commands.



~~~

    //-------------------------------------
    //! Dangerous Commands! 
    //------------------------------------- 
    sudo su
    cd /
    mkdir Backup
    cd Backup
    cp -R /Extra ./
    cp -R /System/Library/Extensions ./
    
    exit

~~~

Ok that will Back your Extra directory and all your Kext files ( Kernal Extensions )

I suggest working slowly and adding Kext files very slowly,  Work on Sound, reboot, then Networking, reboot, etc, etc.

I also suggest before installing all your applications that you make sure you system is running completely or you will probably be re-installing your applications eventually.

After making changes and rebooting, if you get a Kernel Panic then first try rebooting with the iBoot CD first.  You can always try to use boot flags such as 

  * -x => Safe Mode
  * -f => Clear System Cache Folder
  * -v => Verbose Mode
  * -s => Single User Mode CLI



If you can not reboot with the iBoot CD then you will have to Reboot using the Snow Leopard install CD, Open Disk Utility and Mount your Hard Drive, then Open Terminal and Type these commands to restore your working Kext's 


    
~~~

    sudo su
    
    rm -Rf /Extra/
    rm -Rf /System/Library/Extensions/
     
    cp -R /Backup/Extra/ /Extra/
    cp -R /Backup/Extensions /System/Library/Extensions
    
    exit

~~~
    

After running those commands, close terminal and open Disk Utility again.  

Select the correct hard drive and Check Verify Permissions, then Repair Permissions.  


Once you have completed these steps,   
reboot the system and use the "-f" boot flag to Rebuild the Cache directory.

If all of this went over your head, you might want to just buy a Mac,  building a Custom Mac is not easy, but if you can build one yourself it is very satisfying.  The one thing I've noticed with building your own Custom Mac or Hackintosh is you need to be motivated by the challenge and be very persistent,  I have broken and fixed my Hackintosh ton's of times, and it took me 3 months just to get everything to work properly.  I'm still running Snow Leopard on mine as I didn't really like it's features on my iMac.  Some people might like them, I just didn't really like them myself. 


Just a minor update: It's July 28, 2012 and I have been running this hackintosh non-stop straight koding shiz daily nightly anytime and it's best CPU I've ever owned.  But I learned a lot from building it

 - **Patience** is not a funky virtue, it's a state of zen without it do **NOT** try this.
 - This will piss you off, again and again but keep hacking and the rewards are plenty

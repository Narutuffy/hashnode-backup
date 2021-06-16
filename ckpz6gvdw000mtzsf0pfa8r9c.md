## Ubuntu Gutsy and the Intel Video Chipset


## Gutsy and the new Intel Video Driver



For those of us with Intel Onboard Graphics,  that used Feisty you probably used the "i810"  driver for your Graphic needs and probably also needed 915Resolution to fix your resolution problems.  I have found that with Gutsy and the New X Server, Intel has a nicer new driver that will work much better and even detected my 1440x900 Resolution without using a special program to set it!  First make a backup of your xorg.conf file by typing the following, then reconfigure your XServer 


    

~~~

//-------------------------------------
//! Always Backup your xorg.conf file
//------------------------------------- 
sudo cp /etc/X11/xorg.conf /etc/X11/xorg.conf_backup810

sudo dpkg-reconfigure xserver-xorg

~~~

When it ask's for your driver, try the Intel driver instead of the "i810"  driver that you used to use.  Now restart your X Server with Control-Alt-Delete and if you have problems rebooting then you can always restore the backup from terminal by typing

~~~
 
sudo cp /etc/X11/xorg.conf_backup810 /etc/X11/xorg.conf   

~~~
     
Then reboot and everything will be back to normal!

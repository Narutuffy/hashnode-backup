## Ubuntu Gutsy Upgrade - Part 2 - Fixing Compiz


## After Upgrading to Gutsy

This is the finished Ubuntu Desktop complete with Beryl, partial KDE install and many other fancy eye candy's!

{% flickr_image 929318723 b %}

{% flickr_image 929317555 b %}

After the Upgrade, to Ubuntu Gutsy 7.10 I found the following things wrong, broken or just missing.

Things I have fixed already.
  
  * Emerald Theme Manager Removed
  * Avant Window Navigator Was Removed  
  * Compiz-Fusion Setting Manager Does Not Work with KDE
  * KDE-Core was missing??

Things left for me to Fix.

  
  * Fonts are Out of Whack,  They Appear very Bold and Large  
  * More stuff I havent checked for yet

Ok those were the Main things I noticed missing from my Upgrade to Gutsy,  Compiz worked but I could not add/change it's settings,  AWN was removed due my repository list, Emerald was changed and needed to be removed and KDE-Core I Don't know why/where it went but I reinstalled it. So far I have fixed some of this stuff, and will update this post later when i fix the rest.  Here  is the Steps I Took to Solve my Problems.



## Replacing Emerald

Well to Replace Emerald-Theme Manager I just did a Simple
    
~~~
sudo apt-get install emerald
~~~    
    
And that seemed to work, I just had to reselect the theme I used before and everything was back to normal.



## ReInstalling AWN



As I Like the AWN Mac OSX Looking Dock-Bar for my Task bar, I needed it apparently, when I upgraded I lost the repository for it so I found we just needed
to upgrade our sources.list with the following and get the new Key

Add Below to **/etc/apt/sources.list**


    
~~~    
sudo nano /etc/apt/sources.list
~~~    

OR use Gedit if you like the GUI

~~~    
gksudo gedit /etc/apt/sources.list
~~~    
    



Add the following to the above File


    
~~~    
deb http://download.tuxfamily.org/syzygy42 gutsy avant-window-navigator
deb-src http://download.tuxfamily.org/syzygy42 gutsy avant-window-navigator
~~~    



Now to add the GPC Key to  verify this repository Enter the following in your Terminal


    
~~~    
wget http://download.tuxfamily.org/syzygy42/reacocard.asc
sudo apt-key add reacocard.asc
rm reacocard.asc
sudo apt-get update

sudo apt-get install avant-window-navigator-bzr awn-core-applets-bzr
~~~    



Ok and now you have AWN Back in place and working good.




## KDE-Core is Missing?



Since I installed originally a Ubuntu Desktop with GNOME, then later changed/added KDE as my Window Manager,  after this upgrade it appears my KDE packages are gone.  Anyway, I just did this to get them back. not sure where they went


    
~~~    
sudo apt-get install kde-core
~~~





## Removed Abunch of Software



Through Synaptic I Removed abunch of "Local:Obsolete" Applications that were installed before, such as Compiz, Compiz-Fusion Plugins, KiboDocker, Kooldock, mostly stuff I don't use but I figured removing Compiz and reinstalling might help in making it work properly.

**Please Note : I Removed these Completely with Config Files**



## Fixing Compiz-Fusion Manager



Well apparently, after compiling afew different programs from sources, and having a older install of compiz-fusion before the upgrade.  My Compiz-Fusion-Setting-Manager
would exit with strange error methods.   After much looking into the matter, I found the problem was in fact older library's or packages where still in place and being used. 
Take a Look in your /usr/local/lib path for them 


    
~~~    
cd /usr/local/lib
ls
~~~    



Personally I created a backup directory and moved all these files into it for later sorting but do this


    
~~~    
mkdir temp
mv *.* temp
mv * temp
mv .* temp

ldconfig
~~~    



Afterwards You may have to reboot or restart X server ( Control-Alt-Delete ) and you should be able to run the Compiz-Fusion-Settings-Manager now
if you can't find it in the menu, then press Alt-F2 and type 'ccsm' and it will open.

So that fixed my Compiz-Control-Center and we can now login and play with our settings.  

Please Note : If you are running Beryl you must Exit it  before Compiz-Fuzion will work properly.



## Remove Beryl from Startup


Well now since I still had Beryl installed on my Desktop before the upgrade, apparently I had to stop Beryl and  Restart Compiz-Fusion to make it work.
If you don't kill Beryl from running your Fusion effects will not work properly.



    
~~~    
sudo killall beryl
compiz --replace 
~~~    





## Add Compiz To Startup Programs


Now that Compiz-Fusion  is working perfectly I want it to start automaticly so I added a quick line to make it happen, enter the following code into Terminal


    
~~~    
echo "compiz --replace" > ~/.kde/Autostart/startcompiz.sh 
chmod +x ~/.kde/Autostart/startcompiz.sh
~~~    



NOTE : This is for KDE users only.



## The Compiz Management Icon


Without Beryl I missed my Management Icon, so I found one for Compiz already in a deb package at this [forum post.](http://ubuntuforums.org/showpost.php?p=3163821&postcount=8)

After installing the above Icon, add the following to your startup commands so it start's up automaticly.


~~~    
    
echo "fusion-icon" > ~/.kde/Autostart/startcompiz-icon.sh 
chmod +x ~/.kde/Autostart/startcompiz-icon.sh
    
~~~




Now your compiz effects will be working just as you had set them.  I personally just un-installed Beyl, through synaptic to get rid of it.
'If I need to give a Startup Command for this Just make a comment and I will add it




## Firefox Font Fix



Well After abit of googling to no avail to figure out why my fonts didn't look the same as they do in Feisty, I Did find this post on the [Ubuntu Forums](http://ubuntuforums.org/showpost.php?p=3593395&postcount=20),  which gives a good quick Firefox tip to
Help the Problem in Firefox Atleast.

Basicly, open Firefox,  in the Address Bar Copy Paste the Following Line


    
~~~    
about:config
~~~    



Then in the next line, Copy/Paste this line and Search for it then Change the Following Information


    
~~~    
layout.css.dpi
~~~    



Change this value from -1 to 0 and Restart Firefox.

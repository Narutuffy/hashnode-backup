## BandWidth Explained


# BandWidth Explained






Most hosting companies offer a variety of bandwidth options in their plans. So exactly what is bandwidth as it relates to web hosting? Put simply, bandwidth is the amount of traffic that is allowed to occur between your web site and the rest of the internet. The amount of bandwidth a hosting company can provide is determined by their network connections, both internal to their data center and external to the public internet.






## Network Connectivity






The internet, in the most simple of terms, is a group of millions of computers connected by networks. These connections within the internet can be large or small depending upon the cabling and equipment that is used at a particular internet location. It is the size of each network connection that determines how much bandwidth is available. For example, if you use a DSL connection to connect to the internet, you have 1.54 Mega bits (Mb) of bandwidth. Bandwidth therefore is measured in bits (a single 0 or 1). Bits are grouped in bytes which form words, text, and other information that is transferred between your computer and the internet.
  

If you have a DSL connection to the internet, you have dedicated bandwidth between your computer and your internet provider. But your internet provider may have thousands of DSL connections to their location. All of these connections aggregate at your internet provider, who then has their own dedicated connection to the internet (or multiple connections) which is much larger than your single connection. They must have enough bandwidth to serve your computing needs as well as all of their other customers. So while you have a 1.54Mb connection to your internet provider, your internet provider may have a 255Mb connection to the internet so it can accommodate your needs and up to 166 other users (255/1.54).






## Traffic






A very simple analogy to use to understand bandwidth and traffic is to think of highways and cars. Bandwidth is the number of lanes on the highway and traffic is the number of cars on the highway. If you are the only car on a highway, you can travel very quickly. If you are stuck in the middle of rush hour, you may travel very slowly since all of the lanes are being used.
  

Traffic is simply the number of bits that are transferred on network connections. It is easiest to understand traffic using examples. One Gigabyte is 2 to the 30th power (1,073,741,824) bytes. One gigabyte is equal to 1,024 megabytes. To put this in perspective, it takes one byte to store one character. Imagine 100 file cabinets in a building, each of these cabinets holds 1000 folders. Each folder has 100 papers. Each paper contains 100 characters - A GB is all the characters in the building. An MP3 song is about 4MB, the same song in wav format is about 40MB, a full length movie can be 800MB to 1000MB (1000MB = 1GB).
  

If you were to transfer this MP3 song from a web site to your computer, you would create 4MB of traffic between the web site you are downloading from and your computer. Depending upon the network connection between the web site and the internet, the transfer may occur very quickly, or it could take time if other people are also downloading files at the same time. If, for example, the web site you download from has a 10MB connection to the internet, and you are the only person accessing that web site to download your MP3, your 4MB file will be the only traffic on that web site. However, if three people are all downloading that same MP3 at the same time, 12MB (3 x 4MB) of traffic has been created. Because in this example, the host only has 10MB of bandwidth, someone will have to wait. The network equipment at the hosting company will cycle through each person downloading the file and transfer a small portion at a time so each person's file transfer can take place, but the transfer for everyone downloading the file will be slower. If 100 people all came to the site and downloaded the MP3 at the same time, the transfers would be extremely slow. If the host wanted to decrease the time it took to download files simultaneously, it could increase the bandwidth of their internet connection (at a cost, due to upgrading equipment).






## Hosting Bandwidth






In the example above, we discussed traffic in terms of downloading an MP3 file. However, each time you visit a web site, you are creating traffic, because in order to view that web page on your computer, the web page is first downloaded to your computer (between the web site and you) which is then displayed using your browser software (Internet Explorer, Netscape, etc.) . The page itself is simply a file that creates traffic just like the MP3 file in the example above (however, a web page is usually much smaller than a music file).
  

A web page may be very small or large depending upon the amount of text and the number and quality of images integrated within the web page. For example, the home page for CNN.com is about 200KB (200 Kilobytes = 200,000 bytes = 1,600,000 bits). This is typically large for a web page. In comparison, Yahoo's home page is about 70KB.






## How Much Bandwidth Is Enough?






It depends (don't you hate that answer). But in truth, it does. Since bandwidth is a significant determinant of hosting plan prices, you should take time to determine just how much is right for you. Almost all hosting plans have bandwidth requirements measured in months, so you need to estimate the amount of bandwidth that will be required by your site on a monthly basis
  

If you do not intend to provide file download capability from your site, the formula for calculating bandwidth is fairly straightforward:
  

Average Daily Visitors x Average Page Views x Average Page Size x 31 x Fudge Factor
  

If you intend to allow people to download files from your site, your bandwidth calculation should be:
  

[(Average Daily Visitors x Average Page Views x Average Page Size) +
(Average Daily File Downloads x Average File Size)] x 31 x Fudge Factor
  







### Let us examine each item in the formula:





Average Daily Visitors - The number of people you expect to visit your site, on average, each day. Depending upon how you market your site, this number could be from 1 to 1,000,000.
  

Average Page Views - On average, the number of web pages you expect a person to view. If you have 50 web pages in your web site, an average person may only view 5 of those pages each time they visit.
  

Average Page Size - The average size of your web pages, in Kilobytes (KB). If you have already designed your site, you can calculate this directly.
  

Average Daily File Downloads - The number of downloads you expect to occur on your site. This is a function of the numbers of visitors and how many times a visitor downloads a file, on average, each day.
  

Average File Size - Average file size of files that are downloadable from your site. Similar to your web pages, if you already know which files can be downloaded, you can calculate this directly.
  

Fudge Factor - A number greater than 1. Using 1.5 would be safe, which assumes that your estimate is off by 50%. However, if you were very unsure, you could use 2 or 3 to ensure that your bandwidth requirements are more than met.
  

Usually, hosting plans offer bandwidth in terms of Gigabytes (GB) per month. This is why our formula takes daily averages and multiplies them by 31.






## Summary






Most personal or small business sites will not need more than 1GB of bandwidth per month. If you have a web site that is composed of static web pages and you expect little traffic to your site on a daily basis, go with a low bandwidth plan. If you go over the amount of bandwidth allocated in your plan, your hosting company could charge you over-usage fees, so if you think the traffic to your site will be significant, you may want to go through the calculations above to estimate the amount of bandwidth required in a hosting plan.


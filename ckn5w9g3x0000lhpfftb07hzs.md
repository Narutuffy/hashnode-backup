## Chained Select Boxes with Yahoo YUI!


## First we need to Get the Yahoo YUI Library



The folks at Yahoo, have outdone themselfs with this YUI Lib, it is field tested on actual Yahoo pages so we know it can perform the job.  In this short article I will teach you how to make Chained Select boxes using a city, state database using PHP and Mysql.   At the end of this article, I will provide a Zipped resource package with all the working example we make during this article and a mysql database export.

Lets Start with the Head Section of Things,  and Include the needed YUI! Librarys to do what we need to do.



* * *




## The Head Section

~~~ html
    
    <html>
    	
    <head>
    <style type="text/css">
    body{	background-repeat:no-repeat;	font-family: Trebuchet MS, Lucida Sans Unicode, Arial, sans-serif;	height:100%;	background-color: #FFF;	margin:0px;	padding:0px;}
    select{	width:150px;}
    </style>
    
    <script src="http://yui.yahooapis.com/2.3.1/build/yahoo/yahoo-min.js"></script>
    <script src="http://yui.yahooapis.com/2.3.1/build/event/event-min.js"></script>
    <script src="http://yui.yahooapis.com/2.3.1/build/connection/connection-min.js"></script>
    
~~~
    



Now that Includes alittle bit of CSS just to organize our page display, its not really needed at all.  And it includes the Yahoo, Event and Connection-Manager Javascript files directly from Yahoo Servers.
Now we need to write alittle bit of script to make the Ajax Calls back to our Php File


    
~~~ javascript

    <script type="text/javascript">
    <script type="text/javascript">
    function getcitys(sel,url, containerid)                  // This is the Function Call to Get City Data
    {	
    	var div = document.getElementById(containerid);				 // Very Simpley Put, Grab The Container Element to Write to Later
    	var handleSuccess = function(o){if(o.responseText !== undefined){	div.innerHTML = o.responseText;	}}  // This is the Success Handler, All It Does is Write the Data
    	var handleFailure = function(o){if(o.responseText !== undefined){}}  // This is the Failure Handler,  In this Example it Does Nothing
    	var callback ={ success:handleSuccess, failure: handleFailure, argument: { }};   // This is the Callback, It sends Success and Failure to the Correct Location
    	var countryCode = sel.options[sel.selectedIndex].value;      // Lets Grab the Form Element for CountryCode and Set it to Something
    	if(countryCode.length>0){
    		var sUrl = url+'?ajaxed=true&countryCode='+countryCode;	// This sends the Data to the PHP File to Pick which State it is
       	var request = YAHOO.util.Connect.asyncRequest('GET', sUrl, callback);		// This is the Actual Work of the Function, It Sends and Recives are AJAX Data
    	}
    }	
    
    function getprofs(sel,url, containerid)  // This is the Same as the Above no comments needed hopefuly
    {	
    	var div = document.getElementById(containerid);
    	var handleSuccess = function(o){if(o.responseText !== undefined){	div.innerHTML = o.responseText;	}}
    	var handleFailure = function(o){if(o.responseText !== undefined){}}
    	var callback ={ success:handleSuccess, failure: handleFailure, argument: { }};
    	var countryCode = sel.options[sel.selectedIndex].value;
    	if(countryCode.length>0){
    		var sUrl = url+'?ajaxed=true&stateCode='+countryCode;	
       	var request = YAHOO.util.Connect.asyncRequest('GET', sUrl, callback);		
    	}
    }	
    	
    </script>
    </head>
    
~~~


* * *

## The Body Section

Now let's include the actual Body of the Form and save all this to a file and see what we get to start with.


    
~~~ html

    <body>
    
    <form action="" method="post">
    <table>
    	<tr>
    		<td>State: </td>
    		<td><select onchange="getcitys(this,'test_citys.php','citys')" id="forum_state_name" name="forum_state_name">
    			echo '<option selected="true" value=""> </option>';
    			
    		</select>
    		</td>
    	</tr>
    	<tr>
    				<td>City: </td>
    		<td><div id="citys"></div>
    		</td>
    	</tr>
    	<tr>
    				<td>Profs: </td>
    		<td><div id="profs"></div>
    		</td>
    	</tr>
    
    </table>
    </form>
    
    </body>
    </html>

~~~

* * *




## And the PHP



Alright, so now we have the HTML and Javascript out of the way,  But it doesn't do anything yet?  Guess we need the Server Side PHP Now to make it functional ,  Lets Add all ths PHP at the very top of this file before the 


~~~ php 
    
<?php
//Sorry this didn't get exported correctly!    
    '<option>'.$bar['state'] .'</option>';
    	}
    
    	return $statelist;
    }
    	
    function fetch_citys( $state_id )
    {
    	global $state;
    
    	$sql = 'select state,city FROM mods_citys WHERE ID=\'' . $state_id . '\'';
    	
    	$foo = query_first($sql);
    
      $bar = explode (',' , $foo['city'] );
    
    	for ( $i = 0 ; $i <= count ( $bar ) ; $i++ )
    	{
    		$citylist .= '<option name="forum_city_name" value="' . $bar[$i] . '">'.$bar[$i] .'</option>';
    	}
    	
    	$state = $foo['state'];
    	return $citylist;
    }
    
    function fetch_profs( $city_id )
    {
    	$sql = 'select profs FROM mods_citys WHERE ID=\'' . $city_id . '\'';
    	
    	$foo = query_first($sql);
    
      echo $foo;
      $bar = explode (',' , $foo['profs'] );
    
    	for ( $i = 0 ; $i <= count ( $bar ) ; $i++ )
    	{
    		$proflist .= '<option name="forum_prof_name" value="' . $bar[$i] . '">'.$bar[$i] .'</option>';
    	}
    	
    	return $proflist;
    }
    
    if ( !$_GET['countryCode'] == '' )
    {
    	$state_id = $_GET['countryCode'];
    	
      echo '<select onchange="getprofs(this,\'test_citys.php\',\'profs\')" id="forum_city_name" name="forum_city_name">';
    	echo '<option selected="true" name="stateCode" value=""> </option>';
    	echo fetch_citys( $state_id );
    	echo '</select>';
    	echo '<input type="hidden" name="countryCode" value="' . $state_id . '"></input>';
    }
    
    if ( !$_GET['stateCode'] == '' )
    {
    	$state_id = $_GET['stateCode'];
    
      echo '<select id="forum_prof_name" name="forum_prof_name">';
    	echo '<option selected="true" onchange="document.cityredirect.submit()" name="profcode" value=""> </option>';
    	echo fetch_profs( '1' );
    	echo '</select>';
    	echo '<input type="hidden" name="stateCode" value="' . $state_id . '"></input>';
    }
    
    if ( !$_GET['profcode'] == '' )
    {
    	$state_id = $_GET['countryCode'];
    	$city_id  = $_GET['stateCode'];
    	$prof_id  = $_GET['profcode'];
    
    	// Insert your redirect Code here or whatever you want it do to at the end
    
    }
    
    if ( $_GET['ajaxed'] == '' )
    {
    
?>

~~~


And at the very bottom let's just add the closing }  so goto the very bottom of the file and add this


And one more thing, we need a Database,  So open Phpmyadmin or whatever you use, and Create a new Database, then Import the SQL Dump inside the Zip File to It.

I Know this Didn't Really Explain a whole lot except give you a working Example of how to do it.  Since I wrote the Code today,  Stay tuned as I will Comment the Code when i get a chance to.

Please Note :  This was originally written for VBulletin, you will have to change some of the mysql query commands.  Instead of mysql_read or mysql_write, just use mysql_query

NOTE : This is so old the is no point in downloading it.
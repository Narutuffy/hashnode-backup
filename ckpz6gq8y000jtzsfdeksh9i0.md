## Some MySQL Tricks and Tips that made my life easier


Here's a short list of handy MySQL functions and queries that while may not be needed in everyday development, they sure come in handy on that special project that you need to let's say Pull set of Records where the Date Starts Yesterday, or where you need to add a counter number that you would normally have to do in your programming language of choice.


## Query where a By Date 

This little snippet I found quite useful, as it allows you to pull a set of records from Yesterday or 30 Days ago or 30 Days from Now, whatever you set the Interval too.   I found it quite useful in a Calendar module I use quite a lot for real estate companys and clubs that have events.

~~~ mysql

SELECT stuff, FROM things
WHERE date_starts >= DATE_ADD(NOW(), INTERVAL +1 DAY) // tommorow!
ORDER BY date_starts ASC

~~~

## Add a counter to the query results.

We all have needed it for some reason, I normally would loop through my results and do a $i++ but sometimes I don't want to loop through the results, I just want the counter variable there, so I found this snippet somewhere, wish I knew where so I could credit the correct person.

{% codeblock date_add lang:mysql %}


{% endcodeblock}

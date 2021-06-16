## Intraday price alert


Are you interested in stock trading but don't have time to watch prices all day? Why stare at the screen for hours on end when you can automate this kind of thing. This post talks about a small app I created to continuously and automatically detect when the price of particular companies have risen to specified levels.

How do I use this? Before trading starts I run my automated trading strategy which often gives me a bunch of buy signals for various companies. Therefore when the market opens I have a list of companies to hand that I am interested in buying. Part of my strategy is a rule that only allows to buy a company when it has surpassed it's closing price from the previous day. This rule helps stop me from buying any company that is starting a downward trajectory. 

I created the intraday price alert app to automatically monitor the prices of the companies in which I have taken an interest. To the app I input my list of companies and specify the price for each. During the day, if any of these companies surpasses the specified price a red siren on my desk goes off (it literally does, see below for a video). Or when I'm away from my desk it sends me an email alert. This means I can happily work on something else or go out and do other things. If I get an alert I come back to my desk to place my orders.

Read on to learn more about how I created my intraday price alert app.

# Contents

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
*generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Contents](#contents)
- [Getting the code](#getting-the-code)
- [Why?](#why)
- [Example use case](#example-use-case)
- [Loading the alert list](#loading-the-alert-list)
- [Getting stock quotes from Yahoo](#getting-stock-quotes-from-yahoo)
- [Scheduling the market scanner](#scheduling-the-market-scanner)
- [Siren alert](#siren-alert)
- [Email alert](#email-alert)
- [Trigger the alerts](#trigger-the-alerts)
- [Conclusion](#conclusion)
- [Resources](#resources)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Getting the code

Working example code for this post is available to [Patrons](https://www.patreon.com/thedatawrangler). 

Please [show your support on Patreon](https://www.patreon.com/thedatawrangler) and I'll give you access to the code for this app and help you get it running. I also give Patrons access to my back catalog of code. My code runs on [Node.js](https://nodejs.org/) and it is easy to get it working if you are technically inclined. 

If you aren't technically inclined, don't worry, I'll give you a desktop app that you can just download and run.

# Why?

I don't want to be stuck watching my trading screen all day. That's why I'm interested in investment automation in the first place. So when I have companies to monitor I want a computer program to do the heavy lifting for me. I'll have the computer program call me back when I need to jump into action and place an order.

Delegating work to a computer program, when it's possible, takes human error out of the loop. The computer will follow the same exact rules each and every time and the result will be accurate and flawless. Us humans have some trouble with this kind of thing. We'll make mistakes occasionally because we are tired, distracted or just plain lazy.

In addition a computer can handle a much bigger work load than a human can. I doubt I could ever manually watch, say, 100 companies at a time. That's just too many. My computer on the other hand will happily monitor thousands of companies simultaneously, if that's what I tell it to do.

# Example use case

I've prepared an example use case to demonstrate the app.

On the morning of the 4th of May (May the 4th be with you), prior to market open, I ran my investment automation script. It selected QAN and SVW as companies of interest (among others, but let's keep things simple).

Yesterday's closing price for QAN was $4.36 and for SVW it was $11.26. I input these companies and price levels into my alert list as follows:

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828198454/yLTPtUnyD1.png)

Not long after the market opens, my red siren goes off:

<iframe width="560" height="315" src="https://www.youtube.com/embed/efnsAaDvQOI" frameborder="0" allowfullscreen></iframe>

I also receive the following email alert:

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828198461/drOQla5Ktx.png)

Qantas has very quickly broken through yesterday's close, so I kick into gear and buy it!

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828198457/kzl5mx8dxz.png)

I received no price alert for SVN. By the end of the day it still hasn't broken through yesterday's close.

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828198436/9INcIM-Emq.png)

# Loading the alert list

The app's input is a spreadsheet of companies and prices saved as *alert-list.csv*:

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828198434/6RqINo6Vp.png)

[Data-Forge](http://www.data-forge-js.com/) makes short work of loading and parsing the CSV file:

    var alertList = dataForge.readFileSync('alert-list.csv')
        .parseCSV()
        .where(row => row.Code)
        .parseFloats("Price")
        .bake();

For convenience the alert-list is loaded on demand so that it can be edited while the app is running. If you edit the alert-list it will simply be reloaded on the next *tick* of the market scanner.

# Getting stock quotes from Yahoo

The Yahoo financial API is used to read current stock quotes. This is a great way to obtain free share market data, although it is delayed by 20 minutes so it won't be suitable if you need up-to-the-second live data.

Calling the API is somewhat complicated, but to make it easier I have abstracted it under a `getQuote` function that is used like this:

    getQuote(['MSFT', 'APPL'])
        .then(quotes => {
            console.log(quotes);
        })
        .catch(err => {
            console.error("Failed to get quote!");
            console.error(err && err.stack || err);
        });

This example gets quotes for Microsoft and Apple and prints the output to the console, which in this example looks as follows:

    [                         
        {                       
            "code": "MSFT",     
            "price": 67.92      
        },                      
        {                       
            "code": "AAPL",     
            "price": 144.53     
        }                       
    ]                           

I use [Linq.js](https://www.npmjs.com/package/linq) to convert my list of quotes to a JavaScript hash:

    var quoteMap = Enumerable.from(quotes)
        .toObject(
            quote => quote.code,
            quote => quote.price
        );

This produces a convenient lookup table for stock quotes, I can plug a company code into the hash to retreive the latest price:

    console.log(quoteMap['MSFT']);

I thus get the most recent price quote for that company:

    67.92

# Scheduling the market scanner

I'm using Cron to invoke the repeated scanning of the market.

In this case I'm using the following pattern to *tick* the market scanner every 5 minutes: 

    0 */5 * * * *

Please check out my use of the cron in [the previous post](http://www.the-data-wrangler.com/automated-internet-speed-testing/#scheduledrecording) to learn more about it.

# Siren alert

Now we come to the fun part, triggering the flashing red light!

This is what I'm aiming at:

<iframe width="560" height="315" src="https://www.youtube.com/embed/efnsAaDvQOI" frameborder="0" allowfullscreen></iframe>

I want to trigger my [Siren of Shame](https://sirenofshame.com/) when there is a price alert.

So how can I trigger this from JavaScript? Well I started by [downloading the official cross platform Java app](http://blog.sirenofshame.com/2016/05/announcing-soscmd-10jar.html).

This app is executed from the command line. First up I tested that I could trigger the siren from the command line: 

    > java -jar soscmd-1.0.jar -l 4 5

To use this you must have Java 8 installed and added to your system path. I'm running this on Windows, but I think it should work in a similar way on other platforms.

Now to use this command from JavaScript I use the Node.js [`spawn`](https://nodejs.org/api/child_process.html#child_process_child_process_spawn_command_args_options) function. For ease of use I wrap this up in my own `cmd` function:

    var cmd = function (cmd, args) {
        const p = cp.spawn(cmd, args);

        p.stdout.on('data', (data) => {
            console.log(`stdout: ${data}`);
        });

        p.stderr.on('data', (data) => {
            console.log(`stderr: ${data}`);
        });

        p.on('close', (code) => {
            console.log(`child process exited with code ${code}`);
        });                    
    };

I then use my `cmd` function to run two separate commands, the first initates the Siren's lights and the second its audio:

    var sirenTime = 30;

    cmd('java', [
        '-jar', 
        'soscmd-1.0.jar',
        '-l',
        '4',
        sirenTime
    ]);

    cmd('java', [
        '-jar', 
        'soscmd-1.0.jar',
        '-a',
        '3',
        sirenTime
    ]);

This allows me to trigger my Siren of Shame and use it as an alert.

# Email alert

The red siren is heaps of fun and effective when I'm nearby, but it isn't enough. It only works if I'm at my desk or close by, it doesn't work when I'm out and about. I don't want to be deskbound! That's part of the point of investment automation, I want to be alerted even when I've gone out to a meeting or to lunch.

So the intraday price alert app can also do email alerts which allows me to pick up my price alerts on the go. 

To send emails I use [nodemailer](https://nodemailer.com) with an SMTP transport in combination with [Mailgun](https://www.mailgun.com/). Here's an example that shows roughly what the code looks like:

    var nodemailer = require("nodemailer");

    var transport = nodemailer.createTransport({
        service: "smtp",
        host: 'smtp.mailgun.org',
        secure: true,
        auth: {
            user: '<your-mailgun-username>',
            pass: '<your-mailgun-password>',
        },
    });

    transport.sendMail(
            {
                from: 'intradaypricewatch@myemail.com',
                to: 'me@myemail.com',
                subject: 'Notification!',
                text: 'Price for company XXX has reached YYY',               
            },
            function (err, info) {
                if (err) {
                    console.error('Failed to send email notification.');
                    console.error(err);
                    return;
                }

                console.log('Email notification sent!);
            }
        );

Nodemailer also works easily with Gmail. You can use an existing Gmail account or signup for a new Gmail account just for your notifications.

# Trigger the alerts

Most of the pieces are in place now:

- We can input companies and prices;
- We can get price quotes for stocks;
- The market is scanned on a schedule; and
- We have the ability to trigger alerts.

Now we need to wire it all together and actually trigger the alerts when the prices of the companies surpass the specified levels.

With our alert list and quote map this is easily done. For each row in the alert list we check the alert price against the latest price quote for the company. An alert is triggered when the quoted price is greater than the alert price:

    alertList.forEach(row => {
        if (!quoteMap[row.Code]) {
            console.error("No quote retreived for " + row.Code);
            return;
        }

        var curQuote = quoteMap[row.Code];
        if (curQuote > row.Price) {
            //
            // Trigger the alert here!
            //
            triggerAlert(row.Code, row.Price, curQuote);
        }
    });

# Conclusion

With the intraday price alert app monitoring stock prices you can now move onto other things. Relax and enjoy life rather than be glued to a stock chart. Or spend the time testing your strategies or doing other work.

Investment automation gives you the ability to scale up your trading activities without requiring extra time on your part.

# Resources

Data-Forge: http://www.data-forge-js.com/

Siren of Shame: https://sirenofshame.com/

Node.js: https://nodejs.org/

Linq.js: https://www.npmjs.com/package/linq

Nodemailer: https://nodemailer.com

Mailgun: https://www.mailgun.com/
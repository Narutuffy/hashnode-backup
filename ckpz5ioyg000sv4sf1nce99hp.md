## Acquiring stock market data from Alpha Vantage


As a stock trader I need a ready of supply stock market data for analysis and visualisation. That data is needed for decision making and I often render it to a chart to better understand it. 

The free Yahoo financial API was the place to go for stock market data. However since it's demise a new data API from Alpha Vantage has arisen. It's also free and very easy to use. Alpha Vantage offer both daily and intraday data and I have created an open-source command line app for downloading it.

This posts introduces alpha-vantage-cli. I'll show you how to use this tool to download stock market data. I'll give examples of use both from the command line and also using it as an API from your Node.js JavaScript code.

Here's an example of what the downloaded CSV file looks like:

![Microsoft stock price history](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828299403/pCKo4G0X8.png)

# Contents

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
 *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Contents](#contents)
- [Getting an Alpha Vantage API key](#getting-an-alpha-vantage-api-key)
- [Using the command line app](#using-the-command-line-app)
  - [Node.js installation](#node-js-installation)
  - [Installing the command line app](#installing-the-command-line-app)
  - [Acquiring stock market data from the command line](#acquiring-stock-market-data-from-the-command-line)
- [Using the code module](#using-the-code-module)
  - [Installing the code module](#installing-the-code-module)
  - [Acquiring stock market data from code](#acquiring-stock-market-data-from-code)
- [Getting the source code](#getting-the-source-code)
- [Need support?](#need-support)
- [How does it work?](#how-does-it-work)
- [Conclusion](#conclusion)
- [Resources](#resources)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Getting an Alpha Vantage API key

Alpha Vantage is free, but to use it you must sign up for an API key. Please follow this link to sign up:

[https://www.alphavantage.co/support/#api-key](https://www.alphavantage.co/support/#api-key)

The examples that follow use the 'demo' API key, please be aware that this has very limited usage.

# Using the command line app

You can use alpha-vantage-cli as a command line application to download stock market data to a CSV file.

## Node.js installation

First you need to have Node.js installed. This is quite straight forward, please see the Node.js web site for more details:

[http://nodejs.org/](http://nodejs.org/)

## Installing the command line app

Once you have Node.js installed you can install the tool via npm by running the following command: 

    > npm install -g alpha-vantage-cli

This installs the tool globally so that you can run it from any directory.

You can check that it installed correctly using the --version argument:

    > alpha-vantage-cli --version

That should display the most recent version of the tool. There is also a --help argument that will help you understand the tool's options:

    > alpha-vantage-cli --help

## Acquiring stock market data from the command line

Minimal usage looks like this:

    > alpha-vantage-cli --type=<data-type> --symbol=<code-for-the-instrument> --api-key=<your-api-key> 

Here's an example that downloads  download daily data for Microsoft:

    > alpha-vantage-cli --type=daily --symbol=MSFT --api-key=demo --out=MSFT-daily.csv

This downloads data to a file named *MSFT-daily.csv*. See the screenshot at the start of this post to see what daily data looks like.

You can also download intraday data like so:

    > alpha-vantage-cli --type=intraday --symbol=MSFT --api-key=demo --out=MSFT-intraday.csv

This downloads to a file named *MSFT-intraday.csv*. 

Intraday data looks similar to daily data:

![Microsoft intraday stock history](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828299406/yaHs75UY0_.png)

Notice the difference in the timestamp between daily and intraday data.

There are various other options including setting the interval for the intraday data. Please use the tool's --help argument for details.

# Using the code module

alpha-vantage-cli can also be imported into a Node.js script to be used from code.

## Installing the code module

To use please install locally in your Node.js project using npm as follows:

    npm install --save alpha-vantage-cli

## Acquiring stock market data from code

Here's an example of use from a JavaScript code file. Don't forget to replace the API key with your own!

    var AlphaVantageAPI = require('alpha-vantage-cli').AlphaVantageAPI;

    var yourApiKey = 'demo';
    var alphaVantageAPI = new AlphaVantageAPI(yourApiKey, 'compact', true);

    alphaVantageAPI.getDailyData('MSFT')
        .then(dailyData => {
            console.log("Daily data:");
            console.log(dailyData);
        })
        .catch(err => {
            console.error(err);
        });


The example code above gets daily data. Getting intraday data is almost the same, just use the `getIntradayData` function instead. alpha-vantage-cli can also be used from TypeScript code, please see the Github page for more examples.

# Getting the source code

Source code for alpha-vantage-cli is available on Github:

[https://github.com/codecapers/alpha-vantage-cli](https://github.com/codecapers/alpha-vantage-cli)


# Need support?

Would you like help getting started with alpha-vantage-cli? Would you prefer to use a desktop app instead of a command line tool? Please become [a patron of The Data Wrangler](https://www.patreon.com/thedatawrangler) and I will be happy to help you.

# How does it work?

alpha-vantage-cli uses [request-promise](https://www.npmjs.com/package/request-promise) to download data from the Alpha Vantage stock history API.

It's fairly easy to download data manually. You can try out yourself by opening the following link in your browser:

[https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol=MSFT&apikey=demo&datatype=csv](https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol=MSFT&apikey=demo&datatype=csv)

That link downloads the recent price history for Microsoft as a CSV file. It is using Alpha Vantage's demo API key. To use it for any other company you'll need to sign up for your [own API key](https://www.alphavantage.co/support/#api-key).

alpha-vantage-cli provides a simple command line tool and API as an interface to the REST API. This allows you to script batch files to download your stock data or if you need something more sophisticated you can call it directly from your Node.js scripts.

# Conclusion

In this post I've introduced alpha-vantage-cli, my open-source command line tool and API for downloading stock data from Alpha Vantage. 

You have learned how to pull down stock data for use in your own analysis and visualisation and to support your stock trading decisions.

# Resources

Alpha Vantage: [https://www.alphavantage.co/](https://www.alphavantage.co/)

alpha-vantage-cli
npm: [https://www.npmjs.com/package/alpha-vantage-cli](https://www.npmjs.com/package/alpha-vantage-cli)
github: [https://github.com/codecapers/alpha-vantage-cli](https://github.com/codecapers/alpha-vantage-cli)
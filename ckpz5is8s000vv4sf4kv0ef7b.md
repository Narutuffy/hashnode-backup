## Graphing stock data with Highstock and Data-Forge


This is a revisit with various improvements to my [original post on *The Data Wrangler*](http://www.the-data-wrangler.com/highstock-data-forge-yahoo/). In this post I show how to visualise stock data using Data-Forge and Highstock. I also demonstrate how to apply a *simple moving average* to the price data.

The original article retreived stock price data from the Yahoo financial API. Unfortunately and without notice Yahoo killed off this API in mid-May 2017. This was most annoying and caused me to search for a new data API. For this new post I've settled on [Alpha Vantage](https://www.alphavantage.co/), an API that is free to use, although you must first register for an API key.

If you would like to register a complaint with Yahoo about termination of their historical data API you can do so [here](https://yahoo.uservoice.com/forums/382977-finance/suggestions/19346998-reintroduce-the-old-yahoo-finance-api).

[The original version of this post on the Data Wrangler](http://www.the-data-wrangler.com/highstock-data-forge-yahoo/)

[Code hosted on GitHub](https://github.com/codecapers/graphing-stock-data-with-highstock-and-data-forge).

The example code for this post demonstrates the following:

*   [Asynchronous](https://en.wikipedia.org/wiki/Ajax_(programming)) loading of [OHLC data](https://en.wikipedia.org/wiki/Open-high-low-close_chart) into a Highstock [candlestick chart](https://en.wikipedia.org/wiki/Candlestick_chart).
*   [Trading volume](https://en.wikipedia.org/wiki/Volume_(finance)) is displayed below the main stock price chart.
*   A [simple moving average (SMA)](https://en.wikipedia.org/wiki/Moving_average#Simple_moving_average) is generated and overlaid on the OHLC chart.
*   Variables can be edited (company and SMA period) and the chart is regenerated.
*   The chart is resized to fit the web page.
*   A simple web server that serves the web page and proxies the share market data API from [Alpha Vantage](https://www.alphavantage.co/).

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
## Contents  
*generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Getting the code](#getting-the-code)
- [The final product](#the-final-product)
- [Highstock](#highstock)
- [Data-Forge](#data-forge)
- [Historical stock data from Alpha Vantage](#historical-stock-data-from-alpha-vantage)
- [Creating the web server](#creating-the-web-server)
- [Creating a proxy REST API](#creating-a-proxy-rest-api)
- [Plugging the data into the chart](#plugging-the-data-into-the-chart)
- [Computing a simple moving average](#computing-a-simple-moving-average)
- [Event handling and chart resize](#event-handling-and-chart-resize)
- [Conclusion](#conclusion)
- [Resources](#resources)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Getting the code

Clone or fork [the GitHub repo](https://github.com/codecapers/graphing-stock-data-with-highstock-and-data-forge). You can also [download an up-to-date zip of the code](https://github.com/codecapers/graphing-stock-data-with-highstock-and-data-forge/archive/dev.zip) from GitHub.

To run the server locally you'll need to install [Node.js](https://nodejs.org/en/). 

With NodeJS installed you can install dependencies as follows:

    cd graphing-stock-data-with-highstock-and-data-forge
    npm install 
    cd client
    bower install
    cd ..

Then use Node.js to run web server script:

    node index.js

Assuming the server started up without error, you can now browse to [http://localhost:8080](http://localhost:8080/) to see it running.

## The final product

In case you don't want to run the code... this screenshot shows what it looks like:

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828311953/Q_iE_H7wBR.png)

## Highstock

Highstock is a pure Javascript, client-side stock charting library that is free for non-commercial use. The company behind Highstock, [Highsoft](https://en.wikipedia.org/wiki/Highsoft), also have a general charting library called [Highcharts](https://en.wikipedia.org/wiki/Highcharts). Highsoft have many commercial customers and their libraries are stable and mature.

For this example code I started with the [Highstock async loading demo](http://www.highcharts.com/stock/demo/lazy-loading). I also incorporated elements of the [candlestick and volume demo](http://www.highcharts.com/stock/demo/candlestick-and-volume).

There are [many other demos](http://www.highcharts.com/stock/demo) of Highstock that give a good overview of its full capabilities. Highstock also has good [docs](http://www.highcharts.com/docs) and an [API](http://api.highcharts.com/highstock) reference. Read these docs for a full understanding of Highstock.

The basic setup for Highstock is quite simple. Use jQuery to get the element that will contain the chart and call the `highcharts` function. Pass in the options to configure the chart and provide data:

    var chartOptions = {
        // ... options that defined the chart ...
    };

    $('#container').highcharts('StockChart', chartOptions);  

The chart options allow us to set the chart type, axis options and initial data.

Multiple data series can be stacked on top of each other. This is how the SMA is overlaid on the OHLC chart. Multiple Y axis' can also be stacked separately on top of each other, as is done in this example with the volume chart below the OHLC/SMA chart.

In the example code I use the chart types: [_candlestick_](https://en.wikipedia.org/wiki/Candlestick_chart), _line_ and _column_. There are [many more chart types available](http://api.highcharts.com/highstock#plotOptions). 

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828311590/9_VxrmvyW.png)

## Data-Forge

This code example for this post uses [Data-Forge](http://data-forge-js.com/): an open-source data wrangling toolkit for JavaScript that is inspired by [Pandas](https://en.wikipedia.org/wiki/Pandas_(software)) and [LINQ](https://en.wikipedia.org/wiki/Language_Integrated_Query). I have used Data-Forge extensively in my own work and to support my [algo trading](https://en.wikipedia.org/wiki/Algorithmic_trading).

In the example code for this post we use Data-Forge in the browser, which can be installed via [bower](http://bower.io/search/?q=data-forge):

    bower install --save data-forge

You can also install for Node.js via [npm](https://www.npmjs.com/package/data-forge):

    npm install --save data-forge

We include the Data-Forge script in our web page as follows:

    <script src="bower_components/data-forge/data-forge.js"></script>

If you are using it in Node.js, require it like this:

    var dataForge = require('data-forge');

Like a [swiss-army knife](https://en.wikipedia.org/wiki/Swiss_Army_knife), Data-Forge does many things, but what does data-forge do for us in this example?

We use Data-Forge to parse the CSV data returned from Alpha Vantage and Data-Forge's `rollingWindow` function is used to compute the simple moving average. We'll look at how this works over the next couple of sections.

## Historical stock data from Alpha Vantage

[Alpha Vantage's](https://www.alphavantage.co) historical data [REST API](https://en.wikipedia.org/wiki/Representational_state_transfer) is used to download stock data. I've only just started using this and it seems like a good source of historical financial data. Alpha Vantage provides some nice [documentation and examples](https://www.alphavantage.co/documentation/) that shows how to use the API.

You can try out Alpha Vantage quickly by opening the following link in your browser:

[https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol=MSFT&apikey=demo&datatype=csv](https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol=MSFT&apikey=demo&datatype=csv)

That link downloads the recent price history for Microsoft as a CSV file. It is using Alpha Vantage's demo API key. To use it for any other company you'll need to sign up for your [own API key](https://www.alphavantage.co/support/#api-key).

Once you have a key you own API key you can plug it into the URL as shown below:

    https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol=MSFT&apikey=<your-api-key-here>&datatype=csv

Just remember to **plugin in your own API key otherwise the API won't work for other companies**.

After downloading the CSV file you can view it in Excel to understand the structure of the data:

![Microsoft stock data CSV viewed in Excel](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828311940/7toVcasCQk.png)

I often prefer to work with CSV which is a more compact format than JSON and convenient to load into Excel, but it you prefer JSON then Alpha Vantage supports that as well: just change the `datatype` parameter to JSON as follows:

    https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol=MSFT&apikey=<your-api-key-here>&datatype=json

Unfortunately you can't use the Alpha Vantage demo API key for json data, if you want to try this out you definitely have to insert your own API key.

JSON data really convenient in one way, you can simply view it on your browser like so:

![Microsoft stock data JSON format viewed in Chrome](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828311594/9SJs8ybphG.png)

## Creating the web server

We need a web server for two reasons.

1. To serve the web page that contains our chart.
2. To provide a proxy REST API to retreive historical stock data.

So why do we need to create a proxy REST API for the stock data? Because of the [CORS restrictions](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) our web app can't directly access the Alpha Vantage REST API. This kind of indirection is also a good idea anyway because it introduces a [layer of abstraction](https://en.wikipedia.org/wiki/Abstraction_layer) between our web app and the data source. This means we can more easily change our data source at some later time without having to refactor or restructure the web app.

We can easily create a static web server using Node.js and [express](https://www.npmjs.com/package/express):

    var path = require('path');
    var express = require('express');

    var app = express();

    // Serve static files.
    var staticPath = path.join(__dirname, 'client');
    console.log(staticPath);
    app.use(express.static(staticPath));

    // Start the server.
    var server = app.listen(process.env.PORT || 3000, function () {

        var host = server.address().address
        var port = server.address().port

        console.log('Example app listening at http://%s:%s', host, port)
    });

To run this we must install express:

    npm install --save express

Then we can run our web server from the command line:

    node index

With the web server running we can view it our browser by navigating to [http://localhost:3000](http://localhost:3000).

## Creating a proxy REST API

The web server in this example acts as a proxy for Alpha Vantage. It retreives the stock data via HTTP GET using [request-promise](https://www.npmjs.com/package/request-promise) and relays the data to the web page running in the browser.

We can install request-promise into our project as follows:

    npm install --save request-promise

We can then add the proxy REST API is to our express web server, something like this:

    var alphaVantageApiKey = "<insert-your-api-key-here>";

    app.get('/stock-history', function (req, res) {

        // This specifies the company for which 
        // to retrieve the stock history
        var symbol = req.query.symbol; .
        if (!symbol) {
            throw new Error("Symbol not specified.");
        }

        var baseUrl = 'https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol=';
        var url = baseUrl  + symbol + '&apikey=' + alphaVantageApiKey + '&datatype=csv';
        request(url) // Send request to Alpha Vantage.
            .then(function (result) {
                res.set('Content-Type', 'text/csv');
                res.send(result).end(); // Send CSV data to the browser.
            })
            .catch(function (e) {
                console.error(e)
                res.sendStatus(500); // Error result.
            }); 
    });

This code creates a *stock-history* API that we can hit from our browser application to get the price history for a particular company. The request is forwarded to Alpha Vantage and the CSV data retreived is returned to the browser.

Now we can restart our web server and point our browser at [http://localhost:3000/stock-history?symbol=MSFT](http://localhost:3000/stock-history?symbol=MSFT). If all is working correctly the browser should download a CSV file with the stock data.

## Plugging the data into the chart

Now let's move from server-side to client-side. With our server running our web page can use the [jQuery `get` function](https://api.jquery.com/jquery.get/) to hit our REST API and request the history for a particular company. You can start with the following code and print the result to the console in Chrome:

    var code = 'MSFT';
    $.get('stock-history?symbol=' + code)
        .then(response => {
            console.log(response);
        })
        .catch(err => {
            console.error(err);
        });


We are using [Data-Forge](http://data-forge-js.com/) in this example, so we use it to parse and index the the CSV data. The resulting `DataFrame` is then cached so that we later rebuild the chart from the same data set whenever needed.

    var requestData = function (code) {
        return $.get('stock-history?symbol=' + code)
            .then(response => {
                curDataFrame = dataForge.fromCSV(response)
                    .where(row => row.timestamp)
                    .parseDates("timestamp")
                    .parseFloats([
                        "open", 
                        "high", 
                        "low", 
                        "close", 
                        "volume"
                    ])
                    .setIndex("timestamp")
                    .reverse() // Data should be going forward
                    .bake()
                    ;

                return curDataFrame;
            });
    };

It's worth noting that the data from Alpha Vantage is in reverse chronological order. To use the data we need it in chronological order, hence the call to the `reverse` function.

To add the data to the chart we must first convert it to the format expected by Highstock. Our data is time series indexed by date. We convert the dates to milliseconds using the [`getTime` function](https://www.w3schools.com/jsref/jsref_gettime.asp). We then give Highstock the OHLC (open, high, low, close) and trading volume values. Check the functions `dataFrameToHighstockOHLC` and `seriesToHighstock` in the example code to see how this conversion works.

With the data converted we pass it to Highstock via the `series` field of `chartOptions`:

        var price = [...];
        var volume = [...];

        var chartOptions =
        {
            // ... other chart options ...

            series: [
                {
                    type: 'candlestick',
                    name: 'Price',
                    data: price,
                },
                {
                    type: 'column',
                    name: 'Volume',
                    data: volume,
                    yAxis: 1,
                }
            ]
        };

Note how the yAxis is configured for the Volume chart so as to be on the second y axis. This is a whole other chart, below the main chart, that we have configured to display the trading volume.

## Computing a simple moving average

A [simple moving average (SMA)](https://en.wikipedia.org/wiki/Moving_average#Simple_moving_average) is computed and overlaid as a line chart over the OHLC chart. This is a basic financial indicator that smooths the frequent fluctuations in the share market to allow the broad trend to be identified. This is simple to achieve building on the Data-Forge `rollingWindow` function:

    var computeSmaSeries = function (series, period) {
        return series.rollingWindow(period)
            .asPairs()
            .select(function (pair) {
                var window = pair[1];
                return [window.getIndex().last(), window.average()];
            })
            .asValues()
            ;
    };

    var dataFrame = ...
    var smaPeriod = 30;
    var close = dataFrame.getSeries('Close');
    var sma = computeSMASeries(close, smaPeriod);
    var dataFrameWithSMA = dataFrame.withSeries('SMA', sma);

    console.log(dataFrameWithSMA.toString());

The SMA function produces a DataForge Series that we can also added to our chart producing the red line shown below:

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828311179/oJ36pvBS2.png)

As you can see the red line follows the stock prices but eliminates the day to day fluctuations, as such it eliminates noise and can be used to understand the direction of the market.

We can reuse the `seriesToHighstock` function again to format this series to display in the Highstock chart.

We then add the new series to our chart as follows:

        var price = [...];
        var volume = [...];
        var sma = [...];

        var chartOptions =
        {
            // ... other chart options ...

            series: [
                {
                    type: 'candlestick',
                    name: 'Price',
                    data: price,
                },
                {
                    type: 'line',
                    name: 'SMA',
                    color: 'red',
                    data: sma,
                    tooltip: {
                        valueDecimals: 2
                    }
                },
                {
                    type: 'column',
                    name: 'Volume',
                    data: volume,
                    yAxis: 1,
                }
            ]
        };


## Event handling and chart resize

The example code uses [jQuery](https://en.wikipedia.org/wiki/JQuery) for event handling. For example, basics like detecting [button clicks](https://api.jquery.com/click/) and [changes](https://api.jquery.com/change/) in input fields. In response to various events the Highstock chart is updated, resized and re-rendered as necessary.

The most interesting event handler is for the window resize event. It would be great if we could handle an event for a particular HTML element (eg the container div for our chart). However this doesn't appear to be possible and we must handle resize for the entire window and then update the chart to match. 

This isn't the most flexible approach but it works when you want your chart to be sized according to the size of the window (or near enough). It is surprisingly difficult to figure out how to do this and it doesn't feel like the most elegant solution, however like so many other decisions in web development it often comes down to _whatever works_.

So we end up with a simple event handler for window [_resize_](https://api.jquery.com/resize/):

    $(window).resize(function() {
        resizeChart();
    });

The `resizeChart` function updates the size of the Highstock chart:

    var resizeChart = function () {
        var chart = $('#container').highcharts();
        chart.setSize($(window).width(), $(window).height()-50);
    };

The initial size of the chart is in `chartOptions` and is based on the window size:

    var chartOptions =
    {
        chart: {
            width: $(window).width(),
            height: $(window).height()-50
        },

        // ... all the other options ...
    };

## Conclusion

In this post I have shown how to visualize stock data with Highstock and Data-Forge with data retrieved from the Alpha Vantage historical stock data REST API. 

You have learned some basics of using Highstock and Data-Forge, including how to compute and visualize a simple moving average.

## Resources

*   Highstock demos: [http://www.highcharts.com/stock/demo](http://www.highcharts.com/stock/demo)
*   Highstock docs: [http://www.highcharts.com/docs](http://www.highcharts.com/docs)
*   Highstock API reference: [http://api.highcharts.com/highstock](http://api.highcharts.com/highstock)
*   Data-Forge: [http://data-forge-js.com/](http://data-forge-js.com/)
*   Alpha Vantage [https://www.alphavantage.co/](https://www.alphavantage.co/)

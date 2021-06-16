## Backtesting trading strategies with JavaScript


Updated: 2020-09-13

JavaScript can be used for many things that you might not have thought of, including quantitative trading - in this blog post I show how to backtest a quantitative and systematic trading strategy with JavaScript.

As you might imagine this can be quite a complex topic, but in this post I'm only going to cover the basics and we are only going to look at a very simple and naive trading strategy. Let me know if this post is useful and then I'll have motivation for advanced topics in future blog posts.

As you read this you will learn:
- How to define rules in JavaScript to implement a trading strategy;
- How backtesting works; and
- How to use [Grademark](https://www.npmjs.com/package/grademark) to simulate trading and analyze the performance of a strategy.

If you want to jump ahead for a sneak peek at the backtest we are creating in this blog post please [look here](https://grademark.github.io/grademark-first-example/). This notebook was created with [Data-Forge Notebook](http://www.data-forge-notebook.com/) and that's how I created the charts in the blog post.

Don't worry if you don't know much about JavaScript... the code snippets included in this post are pretty simple and for the moment it's enough if you can just read them and generally get the gist of what's being done.

For updates and news about Grademark, be sure to [follow me on Twitter](https://twitter.com/ashleydavis75).

## First a disclaimer... 

Before we get into the thick of it, I must offer a disclaimer. **Please don't take anything in this post (or any other work of mine) as any kind of financial advice**. Trading is a very personal endeavor and you need to decide on your own goals and choose the strategy and techniques that are right for you.

> Real trading is risky. You can and will lose money - both winning and losing are a part of it - and you have to be ok with that. 

**That said...** if you are using the backtesting techniques described in this post then you have no risk, that's because **here we are only doing simulation** and we aren't working in any live financial markets so there isn't any real money on the table.

Be aware though, if you decide to graduate to trading real financial markets, then must be confident in your skills and be prepared to lose the money that you invest.

## Video content

This blog post is basically a written version of some videos that I've already created. You can continue to read about it here or you can watch the videos instead. 

[The first video](https://www.youtube.com/watch?v=ziRmuw3KTj8&t=4s) is a primer on trading and covers some of the basics and theory of backtesting using JavaScript.

[The second video](https://www.youtube.com/watch?v=3IoAV56Zbd4&t=3s) is a demonstration using [Data-Forge Notebook](http://www.data-forge-notebook.com/) that shows how to test a simple strategy in JavaScript.

## What is Grademark?

[Grademark](https://www.npmjs.com/package/grademark) is my open-source toolkit for backtesting and quantitative trading. 

It's pretty new and hasn't yet reached version 1, so I'm very keen for people to try it out and give feedback. If you like the sound of this [please star the repo](https://github.com/ashleydavis/grademark).

## Why do trading?

It's obvious right. We want to make money.

![Dollar signs](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828293929/vCjZR0hkE6.png)
[Image source](https://www.wannapik.com/vectors/62459)

Well, actually it’s a bit more sophisticated than that. 

> I want to grow my account over time, benefit from the effects of compounding, but at the same time, I want to keep my drawdown and risk to a tolerable level.

## Why quantitative trading?

When I say quantitative trading, what I mean is systematic trading by the numbers. 

When developing our trading strategy we will start by defining [indicators](https://en.wikipedia.org/wiki/Technical_indicator) that we'll use as trading signals. The only indicator I'm using as an example in this post is a [simple moving average](https://en.wikipedia.org/wiki/Moving_average) derived from [daily close price](https://en.wikipedia.org/wiki/Open-high-low-close_chart). This is illustrated in the following chart:

![A simple moving average of closing price](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828293570/J-Dnc4MHHc.png)

Our trading strategy consists of a system of rules we use to decide, based on our indicators, when we should open and close trades. We also define rules to help us manage the risk. We thus create a systematic strategy, one that can be simulated.

![A trading system](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828293920/U5vRl3P3Gv.png)

Our trading style is a very personal choice and must be aligned with our goals and financial strategy. I prefer systematic quantitative trading strategies for the following reasons:

- It takes the emotion out of trading;
- It can be simulated;
- Performance can be tested; and
- Trading can be automated.

It's the last reason that's most appealing. It's the dream of any systematic trader to be able to go fully algorithmic,  putting our strategy on automatic then retiring to a beach with a cocktail in hand.

![Cocktail on the beach](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828293156/Xfc3OMA9Z3.jpg)
[Image source](https://www.publicdomainpictures.net/en/view-image.php?image=21543&picture=drink-on-beach)

> More realistically though: a systematic trading strategy that works is a great way to supplement your income and build capital. Get it wrong though and you will basically automate losing money. No one wants that. So be careful! 

Fortunately, we can simulate trading to help us understand if our strategy works. That's what this post is all about!

## Why simulate?

There are many reasons to simulate trading, not just to test the performance of your strategy.

You might also want to simulate trading to

- Practice trading and gain confidence/experience in the market, but in a risk-free environment;
- Prepare for trading when you don't yet have the capital for real trading;
- Evaluate alternative strategies;
- Figure out how to manage risks and survive worst-case scenarios;
- Understand a particular market or financial instrument; and importantly
- Prepare yourself for the psychological aspects of trading (handling losses)

As you can see there are many reasons why simulation is useful. I'm sure you can think of more.

## Managing risk

An important aspect of trading (and hence of trading simulation) is learning how to manage risk. 

> When I talk about risk of course I'm talking about the risk of losing money. Losing is a part of the game and it's ok provided your eventual wins outweigh your losses. That's how we can make a profit.

![Risk vs reward](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828293175/PKybZB0_PA.jpg)
[Image source](https://aspenwoolf.co.uk/student-property-investment-risks-rewards/)

Simulation can help us here in two ways. It can help us prepare for the psychological impact of losing. It also shows whether the wins from our strategy can overpower the losses and how well our strategy copes with occasional disastrous market corrections.

## Types of simulation

There are actually two main ways to simulate trading. *Paper trading* and *backtesting*. Paper trading is also known as forward testing.

*Paper trading* happens in realtime, is very realistic and can feel much like real trading. I highly recommend that beginners start with paper trading. It's a great way to start building your experience in the market. I have built software called [Market Wizard](http://market-wizard.com.au/) to do paper trading, so please check it out if paper trading interests you.

Unfortunately, paper trading is very slow. It happens in realtime, which means that if you are simulating a medium- to long-term trading strategy it would take months if not years to yield results.

> Paper trading is easy and realistic. It's a good starting point, but is also very slow.

This is where *backtesting* comes to our rescue. It's called *back*testing because it tests a trading strategy on historical data. It's going *back*ward in time to find out what would have happened if you had executed your strategy in the real market in the past. 

It's more complicated than paper trading and you are going to need some programming skills to do it. Backtesting wins though because it delivers results very quickly. Through backtesting, we can simulate our strategy and analyze the results just as quickly as our computer program can process all the data. Having optimized code and a fast computer will help here, but anyway you look at it - backtesting delivers results very quickly compared to paper trading. 

A short and simple backtest will take minutes. More complex strategies, bigger markets and more data increase the amount of time required. But we are still talking about having a result in hours or days, rather than in weeks, months or even years.

> Backtesting is more complicated, but is vastly quicker than paper trading.

## We need data!

In order to test a strategy on the past history of a market we need some historical data. 

I used to use the free Yahoo financial API, however since it's demise I've started paying for data. Currently, I use data from [Norgate](https://norgatedata.com/), [EOD Historical](https://eodhistoricaldata.com/) and from [IG](https://labs.ig.com/) who are one of my brokers. 

In order to demonstrate backtesting I must pick an instrument to trade. This isn't about choosing a good instrument I just need to choose something for demonstration purposes.

The instrument I've picked for this post is STW, that's an [exchange traded fund](https://en.wikipedia.org/wiki/Exchange-traded_fund) for [the ASX 200 index](https://en.wikipedia.org/wiki/S%26P/ASX_200). Don't think this is the only option though! 

The backtesting technique I'm presenting in this post can be used for any instrument. You can backtest on futures, individual stocks, currencies, crypto or other exchange traded funds. You have a huge amount of choice in this regard, but ranking and portfolio selection is a larger topic that I might discuss in the future.

See below for a 2017 chart of the ASX 200:

![ASX 200 chart](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828294254/X_44zuOXLa.png)
Image source: Yahoo Finance.

In that chart I've highlighted the [long](https://en.wikipedia.org/wiki/Long_(finance)) and [short](https://en.wikipedia.org/wiki/Short_(finance)) trades. Long trades in blue and short trades in purple.  I annotated this chart to indicate the kinds of trades we could get from this instrument. We'd like to have a strategy that can buy and sell at the right times to make a profit. 

In this post though we are only focusing on the long trades. You might think of that as just normal buying and selling and making a profit when the market is trending up. Short selling allows us to also make profit in a downtrending market, but it is more difficult and riskier and I will save it for a another post, to keep things simple the example strategy for this post will be trading *long only*.

> To keep things simple for this demonstration our trading strategy will be trading long only (no short selling).

So what does our data look like? There's any number of ways you might store your data. You might have it spread out through a bunch of different files or structured in a database for efficient access.  For the purposes of this post we'll have our data stored in a [CSV file](https://en.wikipedia.org/wiki/Comma-separated_values). CSV is a simple and common data format. 

You can see below what our example data looks like. This is a CSV data file containing daily prices for STW for a period of three years:

![STW data file](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828294317/el8o7pKYZW.png)

You can find the [example data file](https://raw.githubusercontent.com/Grademark/grademark-first-example/master/data/STW.csv) online in [the GitHub repo that accompanies this post](https://github.com/Grademark/grademark-first-example).

Now that we have some data to use for our backtesting, let's devise a simple trading strategy.

## An example trading system

There are many types of trading strategies I could choose to demonstrate backtesting. Any quantitative strategy that has definitive rules operating on data can be simulated - provided we have the data.

For this blog post I'm going to demonstrate backtesting using a [mean reversion style trading system](https://en.wikipedia.org/wiki/Mean_reversion_(finance)). I picked this strategy because it happens to be what I'm interested in at the moment and I'm currently researching it. This style of trading is based on the statistics phenomenon called [regression toward the mean](https://en.wikipedia.org/wiki/Regression_toward_the_mean).

> Mean reversion strategies are based on the statistics phenomenon *regression to the mean*.

Variables that have been extended away from their average value have a strong statistical tendency to pull back to the average. You can see what this looks like in the following graphic. The red line in the diagram indicates the average or mean value. The black line is the actual value that is oscillating around the average. Sometimes the value is below the average, other times it's above it.

![Regression to the mean](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828293565/iB6fhb9CP.gif)
[Image source](https://asymmetryobservations.com/definitions/countertrend/regression-toward-the-mean/)

You can see that whenever the black line stretches too far below the average it snaps back to the average crossing over it to the upside. We'd like to use this phenomenon in our trading to buy the dips and sell on the bounce back. 

> The essence of our simple mean reversion strategy: when the price falls below the average value we buy, we sell when the price has *regressed to the mean* and bounced back over the average.

How reliable is this strategy? How likely is a dip to bounce back? We can only know by simulating the strategy on historical data. That allows us to understand how well it would have worked in the past. We can then compute performance metrics and use them to predict how this technique might perform in the future.

Please note that the mean reversion trading strategy I present here is rather basic. I'm keeping it simple just to keep this blog post simple, but it's actually a good starting point for any new strategy to start with the simplest thing that you can conceive. We can always add more rules, more filters and more complexity later, but for this post we'll use the simplest mean reversion rules possible.

## Backtesting with JavaScript

Now that we have some data and we understand the concept behind our example trading strategy, let's write some JavaScript code to do our simulation and backtesting.

### Loading and preparing input data

Before backtesting our trading strategy we need to load and prepare historical price data. We can use [Data-Forge](http://data-forge-js.com/) to load the data into a [dataframe](https://github.com/data-forge/data-forge-ts/blob/master/docs/concepts.md):

```javascript
    let inputSeries = dataForge.readFileSync("data/STW.csv") 
      .parseCSV()
      .parseDates("date", "D/MM/YYYY")
      .parseFloats(["open", "high", "low", "close", "volume"])
      // Index so we can soon merge on date.
      .setIndex("date") 
      // Rename "date" to "time" for Grademark.
      .renameSeries({ date: "time" });
```

We now need to compute an indicator to use as a [trading signal](https://www.investopedia.com/terms/t/trade-signal.asp). Our mean reversion strategy calls for a [moving average](https://en.wikipedia.org/wiki/Moving_average). So let's compute that from our input data:

```javascript
    const movingAverage = inputSeries
      .deflate(bar => bar.close) // Extract closing price series.
      .sma(30);                  // 30 day moving average.
```

Grademark expects that we'll have all our input data and indicators combined. So we must now merge our moving average indicator into the input data:

```javascript
    // Integrate moving average indexed on date.
    inputSeries = inputSeries.withSeries("sma", movingAverage) 
      .skip(30); // Skip blank sma entries.
```

Our input data is in the `inputSeries` variable and ready to be used in backtesting.

It's always good to visualize our data after doing such computations to ensure that it looks like what we expect and that we don't have any bugs in our code. I originally put this code together in Data-Forge Notebook which allowed me to very conveniently visualize the input data:

![A simple moving average of closing price](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828293568/wtvDO3FXA4.png)    

### Entering a position

Now that we have some data we can put together the code for our strategy.

The first bit of code is a rule that determines when we should open a trade and take a position.

Our example mean reversion trading strategy is simple, so our code to open a position is also simple:

```javascript
    if (args.bar.close < args.bar.average) { 
        enterPosition();
    }
```

This code opens a trade by calling the `enterPosition` function when the closing price is less than the average price. This function is provided by Grademark. The `args` variable is provided for us by Grademark and gathers together pertinent details of the trading environment such as the latest [bar](https://en.wikipedia.org/wiki/Open-high-low-close_chart) of price action.

### Exiting a position

Closing a trade - exiting our position - is the reverse of entering it:

```javascript
    if (args.bar.close > args.bar.average) { 
        exitPosition();
    }
```

Here we are saying that when the closing price is above the average price, then we exit the position immediately.

> I know what you are thinking: can something so simple and naive actually produce a profit? Stay tuned, very soon we'll find out.

### Stop loss rule

When I'm trading I also like to use a [stop loss](https://www.investopedia.com/articles/stocks/09/use-stop-loss.asp) rule to help limit  losses and manage risk. A stop loss rule is a different kind of exit, one that kicks in to stop us from losing more than a certain amount of money.

In the following stop loss rule I'm setting a maximum loss of 2% per position before exiting:

```javascript
    const stopDistancePct = 2;
    const stopPrice = 
        args.entryPrice - (args.entryPrice * (stopDistancePct/100));
    if (args.bar.close <= stopPrice) {
        exitPosition();
    }
```

Note that we are calling the same `exitPosition` function as we did in our exit rule. A stop loss is just another way of exiting a position.

Using a stop loss is considered by some traders to erode profit potential for a strategy. So why do I like to use this rule? I like to use this rule in real trading as I feel it protects me against the heaviest losses. It buys me some peace of mind. In real trading I have a stop loss rule automatically applied by my broker and this means I can go on holiday and know that my investments are protected. 

In this example, I've included the stop loss right from the start, but normally you might consider this as an incremental addition to the strategy, testing before and after adding it - normally you'd probably like to know how much of difference the stop loss makes to understand if it is worthwhile.

### Simulation loop

The main piece of the puzzle for backtesting is the simulation loop. It walks over our input price data day by day, evaluates our rules and creates a series of trades.

If we wrote the code ourselves it would look something like this:

```javascript
    for (const bar of inputSeries) {
        if (inPosition) { 
            // We currently have an open position, 
            // do we need to exit the position?
            evaluateStopLoss({ bar, position }); // Evaluate stop loss rule.
            evaluateExitRule({ bar, position }); // Evaluate exit rule.
        }
        else {
            // We currently have no open position, 
            // do we need to open a new position?
            evaluateEntryRule({ bar }); // Evaluate entry rule.
        }
    }
```

This code snippet is really just pseudo-code, Grademark actually provides the simulation loop for us. With Grademark we simply call the `backtest` function and pass in the input data and a JavaScript object that represents our trading strategy:

```javascript
    const { backtest } = require('grademark');

    const trades = backtest(strategy, inputSeries);
```

The `strategy` object contains the functions `entryRule`, `exitRule` and `stopLoss` that define our strategy. The complete strategy definition looks like this:

```javascript
const strategy = {
    entryRule: (enterPosition, args) => {
        if (args.bar.close < args.bar.sma) { 
            enterPosition(); // Buy when price is below average.
        }
    },

    exitRule: (exitPosition, args) => {
        if (args.bar.close > args.bar.sma) {
            exitPosition(); // Sell when price is above average.
        }
    },

    stopLoss: args => {
        // Stop out on 2% loss from entry price.
        return args.entryPrice * (2 / 100); 
    }
};
```

### Capturing trades

The process of backtesting generates trades. The result of the `backtest` function captures the important data about each trade including entry and exit dates, entry and exit prices and the amount of profit per trade. Here's an extract from Grademark's TypeScript interface for a trade:

```typescript
export interface ITrade {

    entryTime: Date;

    entryPrice: number;

    exitTime: Date;

    exitPrice: number;

    profit: number;

    profitPct: number;

    // ... More fields ...
}
```

Profit and other metrics are computed and attached to each trade object.

### Analyzing the performance of the strategy

To understand the global performance of our strategy we might use  [Data-Forge](http://data-forge-js.com/) and compute the sum of profit over all the trades:

```javascript
const totalProfit = new DataFrame(trades)
    .deflate(trade => trade.profit)
    .sum();
```

However, Grademark provides an `analyze` function that does this and more:

```javascript
const { analyze } = require('grademark');

const analysis = analyze(10000, trades);
```

The `analyze` function is passed an amount of starting capital and the set of trades. In this example, we are funding our virtual trading with a $10,000 account. The analysis loops over the trades and simulates trading with the requested amount of capital. Profitable trades increase the size of our account, losing trades decrease the size of our account. The result is a performance summary of our trading strategy that includes total profit and other useful metrics:

![Strategy performance](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828293583/KBKFPJrtX4.png)

We now have a basic idea of how good (or bad) our strategy is. We can use this information to work out if the profit (83%) is worthwhile compared to the max risk (2%) or maximum drawdown (-28%). 

We can use this ask questions like *can I tolerate a -28% drawdown?* That's jargon. Think of it this way: *can I handle a 28% loss from the peak.* As traders, a bit of forward planning like this really helps avoid situations where we might panic and pull out of a strategy before it manifests the ancipated profit.

We can use the analysis to determine if changes to a strategy will improve it or make it worse. The analysis also allows a comparison between alternate strategies that we might be considering.

## Visualizing equity and drawdown

After backtesting, it's very common to render an equity curve for our strategy. 

The equity curve shows us the value of our trading account increasing over time:

![Equity curve](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828293586/SJYY2Z5zxV.png)

Another useful chart shows the drawdown of our strategy over time. 

With the drawdown chart we can see the amount of time our strategy spends underwater:

![Drawdown chart](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828293922/Wc03X-TmiT.png)

Such charts are produced by calling the Grademark functions `computeEquityCurve` and `computeDrawdown` and then plotting charts using [tbe Plot library](https://www.npmjs.com/package/plot) or [Data-Forge Notebook](http://www.data-forge-notebook.com/):

```javascript
const equityCurve = computeEquityCurve(10000, trades);
const equityCurvePlot = plot(
    equityCurve, 
    { 
        chartType: "area", 
        y: { min: 9500, label: "Equity $" }, 
        x: { label: "Trade #" }  
    }
);
await equityCurvePlot.renderImage("equity-curve.png");

const drawdown = computeDrawdown(10000, trades);
const drawdownPlot = plot(
    drawdown, 
    { 
        chartType: "area", 
        y: { label: "Equity $" }, 
        x: { label: "Trade #" } 
    }
);
await drawdownPlot.renderImage("drawdown.png");
```

## Conclusion

This post has been a basic overview of simulating and backtesting a simple example (mean reversion) trading strategy using JavaScript and the Grademark API.

This is just a starting point and we still have much more work to do. For instance, we haven't taken into account the effect of fees or slippage. That means the results we are getting are probably too optimistic. 

Also, our analysis is only based on one sequence of trades that would have occurred in the past and so far our use of data science has been simple and the results are quite fragile. We can't expect to have this exact same performance again in the future because we'll never again see this exact same sequence of trades. Fortunately, we have statistical tools we can throw at this problem like [monte-carlo simulation](https://en.wikipedia.org/wiki/Monte_Carlo_method). Monte-carlo simulation will help us have a better statistical understanding of what *range* of performance we can expect in the future from our trading strategy.

In addition, how do we know we are using the best parameters for our indicators? In this example trading strategy we used 30 days as a parameter to our moving average. Why 30 days? Why not some other number? Could a different value produce better (or worse) performance? We can use a process called [walk-forward optimization](https://en.wikipedia.org/wiki/Walk_forward_optimization), a kind of data-mining, to help us determine the performance impact of different parameter values for our trading strategy.

As you can see there are more advanced aspects of trading simulation and quantitative trading I could write about. If you are interested in this sort of thing and want me to continue please reach out and tell me your area of interest.

## Resources

- [Grademark](https://www.npmjs.com/package/grademark)
- [Grademark first example notebook](https://grademark.github.io/grademark-first-example/)
- [Data-Forge](http://data-forge-js.com/)
- [Data-Forge Notebook](http://www.data-forge-notebook.com/)
- [The Data Wrangler on YouTube](https://www.youtube.com/channel/UCOxw0jy384_wFRwspgq7qMQ)

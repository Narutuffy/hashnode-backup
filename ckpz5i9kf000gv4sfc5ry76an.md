## Data-Forge v1 is away!


I have some big news. I've just published Data-Forge version 1 to npm! After over 2.5 years, 1,148 commits and 2 Github repositories it's finally here, and now rewritten in TypeScript no less! This software has been in gestation for sometime.

Data-Forge is my open-source JavaScript data-wrangling and analysis toolkit that was inspired by Pandas and LINQ. I use it for all manner of data-wrangling tasks. I've used it when working on SQL and MongoDB databases, creating reports, backtesting and algo stock trading. I've put it in web apps, server backends, (JavaScript-based) mobile apps. It's my goto tool for slicing and dicing data.

Over the past 5 months or so I've rewritten Data-Forge in TypeScript, although of course you can still use it from JavaScript ES5. It's now better than ever before and still mostly compatible with prior versions. I've improved it's performance, reliability and I've also resolved some long standing design issues.

Data-Forge was born from my frustration in working with data in Python and Pandas. Don't get me wrong though! I won't try to tell you that Python and Pandas are bad in anyway, but it just irked me that I had to rewrite my data handling code in JavaScript so I could then move it to a JavaScript production environment. Besides I just liked JavaScript (well, now TypeScript).

What's next for Data-Forge? There's still more work to be done on Data-Forge itself, but I also have some related projects that I'm itching to get onto. I'm planning to release my extensive library of financial indicators that I use for backtesting stock trading strategies. I'm also planning to build an add-on charting library (on top of D3 of course) that will work from either the browser or from Node.js. This is going to make it so much easier to do ad-hoc exploratory data analysis under Node.js! So please stay tuned.

You can find [Data-Forge on npm](https://www.npmjs.com/package/data-forge).

Please be sure to **star** [the Github repo](https://github.com/data-forge/data-forge-ts)!

I'll soon be writing an introductory blog post on how to get started with Data-Forge, for now though you can take a look at the example in the [readme](https://github.com/data-forge/data-forge-ts), read [the Data-Forge guide](https://github.com/data-forge/data-forge-ts/blob/master/docs/guide.md) or browse [the API docs](https://data-forge.github.io/data-forge-ts/).
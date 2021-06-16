## Data-Forge Plot update


Just a quick update of my progress developing [Data-Forge Plot](http://www.the-data-wrangler.com/introducing-data-forge-plot/). The API has evolved a little bit and so has my thinking around it. I've also started adding examples and unit tests.

## Assigning series to axis'

I've simplified and extended the API. One of the big changes is that you can now set an x axis series to correspond to a y axis series. That has allowed me to replicate [the multi-series C3 scatter plot example](https://c3js.org/samples/chart_scatter.html). You can find a [single-series example with Data-Forge Plot here](https://github.com/data-forge/data-forge-plot/blob/master/examples/c3/example-9.ts) and the [multi-series example here](https://github.com/data-forge/data-forge-plot/blob/master/examples/c3/example-10.ts).

Here are some examples of assigning series to axis' using Data-Forge Plot.  The basic setup looks like this:

    const df = ... the dataframe containing your dataset ...
    const plot = df.plot({}, { 
        x: "ColumnA", // Assign ColumnA to the x axis
        y: "ColumnB"  // and ColumnB to the y axis.
    });

    // Render the image, or whatever else you want.
    plot.renderImage("my-chart.png"); 

Now Data-Forge Plot already had the ability to plot multiple series to the primary Y axis' like this:

    const plot = df.plot({}, {
        x: "ColumnA",
        y: [ "ColumnB", "ColumnC" ]  // Plot multiple series to the Y axis.
    });

You could also plot one or more series on the secondary Y axis as follows:

    const plot = df.plot({}, {
        x: "ColumnA",
        y: "ColumnB",
        y2: "ColumnC"
    });

What's new now is that you can expand the definition of an axis and explicitly assign an x axis series for each y axis series. Here's an example of how to  replicate the [the multi-series C3 scatter plot example](https://c3js.org/samples/chart_scatter.html) with Data-Forge Plot:

    const plot = df.plot({}, {  
        y: [
            {
                series: "versicolor_y", // The Y axis.
                x: "versicolor_x"       // The corresponding X axis.
            },
            {
                series: "setosa_y",     // Another Y axis.
                x: "setosa_x"           // The corresponding X axis.
            }
        ]     
    });

Obviously you can also use any valid combination of these methods of assigning series to axis'.

## Moving towards plugin charting libraries

My big vision for Data-Forge Plot is to have the potential to easily integrate any JavaScript visualization library with Data-Forge. There are so many visualization and charting libraries for JavaScript why shouldn't you have that choice?

To that end I've been prototying a templating mechanism that allows templates for new charting libraries to be added. My intention is to make it easy for others to extend Data-Forge Plot to add support for their favorite charting library. It will also be nice to have a selection of visualization libraries to choose from depending on what you are trying to achieve and which one does the job best for you.

I've prototyped this templating mechanism by adding basic support for the [Flot](http://www.flotcharts.org/) charting library. So Data-Forge Plot now supports both [C3](http://c3js.org/) and Flot. By default when you are plotting a chart it defaults to using the C3 library, but you can now change templates like this:

    const plot = df.plot({ template: "flot" }, { x: "ColumnA", y: "ColumnB" });

There is a still long way to go with this, including having good documentation for template creators, but I'm on my way to having a simple and straightforward template-based extension mechanism for Data-Forge Plot.

## Resources

- [Data-Forge](http://www.data-forge-js.com/)
- [Data-Forge Plot](https://github.com/data-forge/data-forge-plot)
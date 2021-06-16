## Data-Forge Plot update 2


Another quick update on my progress developing [Data-Forge Plot](/introducing-data-forge-plot/) (DFP). [The API has recently evolved](/data-forge-plot-update) but is remains relatively stable, although there are still features I must add to it. I'm sure it will continue to evolve as I integrate new visualization libraries and expand the API. The main thing I have done recently is to extract and restructure the export and capture code, which I talk about in this post.

## Export templates

To support the quick integration of JS charting and visualization libraries into Data-Forge Plot I've been working to create a plugin system for adding new *export templates* to DFP.

Export templates are used for rendering charts to images (the `renderImage` function) and for exporting to the web (`exportWeb`) and Node.js (`exportNodejs`). Existing templates are currently built into DPF, but in the future I also plan to allow external templates, so that users of DFP can integrate their own JS charting libraries.

This is still early work and I'd like to refine it some more, but is a good step forward and has allowed me to clean up the code and start to solidify a standard against which I and other DFP users can create export templates.

So, how does it work?

See the following screenshot shows the export template for the C3 charting library. This is the default template and it is [included with DFP](https://github.com/data-forge/data-forge-plot/tree/master/templates/c3):

![C3 export template screenshot](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828185820/b8uaf4DPH.png)

The directory for the example C3 template includes all the files that are required to generate, export and capture charts using the [C3 charting library](https://c3js.org/). Templates for other libraries will look similar, if not the same as this.

You can see there are sub-directories for `nodejs` and `web`. These are the two main types of export and correspond to the DFP functions `exportNodejs` and `exportWeb`. The implementation of the `renderImage` function also relies on the content in the `web` sub-directory.

An important file to notice is `template.json`. This is the configuration file for each template and specifies files that should or shouldn't be expanded. We can disable expansion for files that don't need it (e.g. static web assets) and that serves to make the export process a bit quicker (non-expanded files are simply copied and not loaded into memory for expansion).

## Export to web and Node.js

Export to web and Node.js works by first serializing the content of the DataFrame and the configuration of the chart (configured using data or fluent API as mentioned in previous posts about DFP). Then the template is expanded. The template sub-directory, either `web` or `nodejs`, is selected depending on whether you have called DFP's `exportWeb` or `exportNodejs` function. 

Each sub-directory contains an `assets` directory. Files under `assets` are expanded in memory during the export using the awesome [Handlebars](https://handlebarsjs.com/) templating library. For example, look at the earlier screenshot of the C3 template and you'll see that the `web` sub-directory contains a complete web visualization with placeholder elements that are filled out using the serialized DataFrame and chart configuration. After the template is expanded in memory it is written to the target directory to complete the export process. You now have a web visualization for your data configured how you wanted it.

The following diagram illustrates Data-Forge Plot's export pipeline:

![DFP export pipeline](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828185823/xXenaY31r2.png)


## Capture to PNG image

Capturing a chart to an image using DFP's `renderImage` relies on the `web` template, so it works in a similar way to the `web` export described in the previous section. A key difference is that the expanded template doesn't need to be written to disk as it is only required to be in memory long enough [to load it into a headless browser (it's headless because we don't need to see it) and then render the chart to an image file](https://css-tricks.com/server-side-visualization-with-nightmare/). 

Because the expanded chart template is being rendered in a headless browser it needs a web server to deliver web assets and data. As the headless browser requests assets they are expanded in memory (never being written to disk) and served via HTTP. 

There's a lot of complexity happening under the hood! But as a user of DFP you don't have to care about that, you just focus on gathering your data, calling `plot` to describe your chart then call `renderImage` to capture your chart to an image file. I'm sure that normally when people use this library they won't consider or be concerned with the level of technical sophistication that's happening here and nor should they, a hallmark of a good API is one that makes the difficulty and complexity disappear completely. That's what I've been aiming for and hopefully that's what I've achieved!

The following diagram illustrates Data-Forge Plot's image capture pipeline:

![DFP capture pipeline](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828185825/nJpKhF8kMB.png)

## New libraries

While building out the new templating system for enhanced export and capture in Data-Forge Plot I've created two new open source libraries. 

The first is [inflate-template](https://github.com/data-forge/inflate-template): a library for expanding a multi-file template. This can be thought of as an extension to the [Handlebars](https://handlebarsjs.com/) library so that it can support an entire directory worth of template files instead of just a single file.

The second is [capture-template](https://github.com/data-forge/capture-template): a library that can expand a web page template, load it into a headless browser (Nightmare/Electron) and then capture it as PNG or PDF file. This is an extension to my earlier work on [server-side visualization under Node.js](https://css-tricks.com/server-side-visualization-with-nightmare/).

I've factored these libraries out of DFP so that I can use them in other work, noteably I'm using them now in [Data-Forge Notebook](http://www.data-forge-notebook.com/) so that JavaScript notebooks can be exported to various formats. I'm just about to roll out new features that allow notebooks to be exported to PNG, PDF and web pages. Coming very soon you will also be able to export the code from a notebook to a single JavaScript file or a complete (and runnable) Node.js project.

## Resources

- [Data-Forge](http://www.data-forge-js.com/)
- [Data-Forge Plot](https://github.com/data-forge/data-forge-plot)
- [Data-Forge Notebook](http://www.data-forge-notebook.com/)
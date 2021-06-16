## Data-Forge Notebook hits version 1!


After part-time development for an entire year I'm very excited and proud to announce the launch of [Data-Forge Notebook](http://www.data-forge-notebook.com/) version 1. 

Thanks so much to all my [early supporters](https://github.com/data-forge-notebook/wiki/wiki/supporters) for helping me achieve this milestone.

## What is Data-Forge Notebook?

[Data-Forge Notebook](http://www.data-forge-notebook.com/) is a notebook-style desktop application for Windows, MacOS and Linux.

It's like Jupyter Notebook, but dedicated to JavaScript/TypeScript and Node.js.

It aims to take Python-style exploratory coding, put it in JavaScript and connect it directly to the extensive JavaScript ecosystem for interactive visualization. 

It's a friendly and interactive environment for coding with zero configuration and contains everything you need to get started. Just install [Data-Forge Notebook](http://www.data-forge-notebook.com/) and start coding.

Visualize your output and data while you code to check that your code works as planned.

![Promo notebook](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828189523/kLr5kAiAi.png)

Because Data-Forge Notebook is a desktop app and not a hosted web application, it puts your privacy found and center. You store all your code and data on your on your local PC and you don't have to submit them to someone's hosted service in the cloud.

## What can you do with it?

[Data-Forge Notebook](http://www.data-forge-notebook.com/) comes with an embedded portable version of Node.js. Any code that you can run under Data-Forge Notebook you can also run under Node.js.

It is an environment for prototyping and exploratory data analysis. You can visualize JavaScript data, tabular data and easily create charts.

![Charts](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828189164/GvARU8TAgM.png)

When working in Data-Forge Notebook you have the world of npm modules at your fingertips, npm modules are automatically installed for you as you code. No more manual installation and configuration of your package file! Using npm modules gives you access to almost unlimited functionality and data formats. 

Data-Forge Notebook supports multiple export formats. You can export your notebooks as PNG, PDF, HTML or markdown. You can also export runable code: either as a single code file or as a complete Node.js project. 

[Click here for an example notebook exported to HTML](https://data-forge-notebook.github.io/data-forge-cheat-sheet/). 

[Click here for an example notebook exported to markdown format](https://gist.github.com/ashleydavis/244b8f7ef91a36b7f8e1b2f0dd90c6f5).

[Data-Forge Notebook](http://www.data-forge-notebook.com/) also supports TypeScript and it even does the TypeScript configuration for you. In addition it automatically creates your build script.  On top of all that it automatically installs npm peer dependencies and TypeScript type definitions.

Don't worry about project setup and configuration. Let Data-Forge Notebook do that for you! When finished export your notebook to runnable code so that you can run it in your production JavaScript environment!

Data-Forge Notebook comes with a heap of example notebooks and cheat sheets to help you get started and understand how to complete various tasks.

![Data-Forge cheat sheet notebook](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828189161/kL_9HPBxES.png)

## How does it work?

[Data-Forge Notebook](http://www.data-forge-notebook.com/) is a cross-platform desktop application built on the [Electron framework](https://electronjs.org/).

It evaluates code on an embedded portable version of Node.js that it runs as a separate process to the Electron application. Evaluating user code in a proper Node.js instance serves two purposes:
- It ensures that code that runs in Data-Forge Notebook will most definitely run in Node.js.
- Having code evaluation separate from the Electron process means that problems in user code can't bring down the notebook editor.

Data-Forge Notebook creates two Electron browser windows, one to host the notebook editor and a hidden window that is used to run background jobs so as not to lock up the UI.

![Data-Forge Notebook structure](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1623828189157/33Q4gJB1f.png)

The main process communicates with the evaluation engine and the hidden window using interprocess communication (IPC).

Data-Forge Notebook is packaged for distribution using [electron-builder](https://github.com/electron-userland/electron-builder).

I'll talk more in a future blog post about how Data-Forge Notebook works and more generally about building desktop apps with Electron. So please stay tuned!


## Onward

Data-Forge Notebook doesn't stop with version 1! I'll continue moving it forward and there's more features coming.

One big feature coming soon is the ability to visualize maps and geo data.

To get a taste for current features please see [the landing page](https://www.data-forge-notebook.com).

To understand where Data-Forge Notebook is heading please see [the road map](http://wiki.data-forge-notebook.com/road-map).

There's so many other features that would be awesome in Data-Forge Notebook so please [get in touch](mailto:ashley@codecapers.com.au) and tell me what you think would be useful. 


## Get it now!

Data-Forge Notebook version 1 is available now at [www.data-forge-notebook.com](https://www.data-forge-notebook.com).


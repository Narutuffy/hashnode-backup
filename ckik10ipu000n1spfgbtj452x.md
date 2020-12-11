## Stream your hapi.js logs to Google Cloud

I have been using Google Cloud Platform for a while now, deployed several Node.js applications using [App Engine custom runtime](https://cloud.google.com/appengine/docs/flexible/). Node and GAE is a relatively new combination and of course there are advantages and disadvantages of going down this path instead of the traditional GCE. Writing about this experience might be a good idea for my next post but this time I want to share with you how to fully use [GCP Logging](https://cloud.google.com/logging/) with your node application. It always bugs me that the out-of-the-box logging solution that Google provides to GAE custom runtime is a simple fluentd agent that reads the app logs as plain text without the ability to define the log pattern. This way developers can’t analyze their logs and figure out what’s going on with their application. Recently, I have started working on a new application and as always I chose my hapi.js stack on top of GAE. The only problem is that I needed a good logging infrastructure to monitor all the data that is being streamed to my application. I tore down the web looking for an elegant solution of how to stream my logs to GCP Logging, without any lead. This is when I decided to take a step forward and build a module which streams [Good](https://github.com/hapijs/good) (the logging module for hapi) logs to GCP Logging. Thanks to the recent refactor made by [Adam Bretz](https://medium.com/u/f785405c3902) in version 7, Good reporter is just a _stream_ object which is very easy to implement. Using the [_gcloud_](https://github.com/GoogleCloudPlatform/gcloud-node) module (GCP api module), I extended the _Stream.Writable_ class to create a stream which maps the incoming Good data into a GCP Log Entry and send it to the cloud. Now all the logs are fully structured in GCP Logging and I can create custom metrics and get insights out of my logs.It is very easy to use this module if you are already familiar with hapi.js (if not, I am encouraging you to give it a try).

*   Install the module _npm install good-gcloud_
*   Add _good-gcloud_ to your Good reporter as the end stream
*   Log as usual using _server.log_ or _request.log_
*   Analyze your logs using GCP Logging

For more detailed instructions about _good-gcloud_ configuration, you are welcome to visit the [github page](http://bit.ly/1Y0tWIS).

Waiting for your feedback and activity in the repository!P.S I created two other useful streams for Good

*   [good-separator](http://bit.ly/21n0zlR) — adds a separator after each object in the stream. Used to add line break after each object when streaming to stdout.
*   [good-requests-filter](http://bit.ly/1VFQbpN) — filters requests logging using regex. Used to filter swagger related requests.

## Using OneSignal in a Vue CLI 3 Application

If you are working on a web application most chances are you would like to add
web push to engage with your users.

OneSignal makes it easy to manage, customize and implement web push and the free
plan might have cover your needs for a long time. This is why we chose OneSignal
for our internal system for the content management of Daily.

The only problem remains is the conflict between the existing service worker of
the PWA module of Vue CLI and OneSignal’s service worker for web push. I must
admit that OneSignal is not very flexible in terms of managing their service
worker and it took a while to solve this conflict.

At first, I just tried to add a simple `importScript('/service-worker.js')`
command to OneSignal’s service worker to import Vue’s service worker. It did
work but the only problem was that it did not update `service-worker.js` file
ever, leaving old files in cache and never showing the up-to-date version of the
app. 😓

After a while and digging into Nuxt solution for the same problem, I came up
with a solution. 😌

Hang tight and follow carefully:

Comment out the following line from `main.js` , as OneSignal automatically
registers the service worker:

    import './registerServiceWorker';

Make sure to initialize OneSignal, in the same file, `main.js` :

    window.OneSignal = window.OneSignal || [];
    window.OneSignal.push(() => {
      window.OneSignal.init({
        appId: process.env.VUE_APP_ONESIGNAL,
        allowLocalhostAsSecureOrigin: process.env.NODE_ENV !== 'production',
      });
    });

Remember to set `VUE_APP_ONESIGNAL` to your OneSignal’s application ID in the
relevant `.env` file.

Add GCM sender properties to your `manifest.json` file:

    "gcm_sender_id": "482941778795",
    "gcm_sender_id_comment": "Do not change the GCM Sender ID"

Now we have to set workbox to ignore OneSignal’s files and not to cache them,
add the following to your `vue.config.js` :

    module.exports = {
      pwa: {
        workboxOptions: {
          exclude: [/OneSignal.*\.js$/],
        },
      },
    };

Obviously, we also have to import OneSignal SDK, they suggest to fetch it from
their CDN so we can simply add the following line to our `index.html` :

    <script src="https://cdn.onesignal.com/sdks/OneSignalSDK.js" async></script>

Now for the important part, we will not use the hard-coded OneSignal service
workers but we will generate them at build time, we have to make to sure that
for each deployment the content of these service workers will change and will
force to check for updates. We can use the build date time as a parameter which
changes on every build. The following code generates two service workers file
which are actually the same as OneSignal requires:

    const path = require('path');
    const { writeFileSync } = require('fs');
    // Provide OneSignalSDKWorker.js and OneSignalSDKUpdaterWorker.js
    const makeSW = (name, scripts) => {
      const workerScript = scripts.map(i => `importScripts('${i}');`).join('\r\n');
      writeFileSync(path.resolve(__dirname, '../dist', name), workerScript, 'utf-8');
    };
    const importScripts = [
      `/service-worker.js?v=${Date.now()}`,
      'https://cdn.onesignal.com/sdks/OneSignalSDKWorker.js',
    ];
    makeSW('OneSignalSDKWorker.js', importScripts);
    makeSW('OneSignalSDKUpdaterWorker.js', importScripts);

Lastly, we have to execute this command on every build so let’s add it to our
build script in `package.json` :

    "build": "vue-cli-service build && node build/onesignal.js",

Now both workbox and OneSignal can live happily ever after and you can engage
your audience with awesome web push.

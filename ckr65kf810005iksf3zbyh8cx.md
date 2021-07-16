## React Native Deep Linking Simplified


Before we go through the **HOW** part of this blog where we will be adding *Deep Linking* in our React Native app, let’s first go through the **WHAT** and **WHY** to better grasp the concept.

A deep link* *is simply an *intent filter system* that allows the user to access a certain activity in an Android app with a URL. Let us suppose that we saw a certain product (for example a shoe) on an e-commerce website, and we want to share it with a friend. A deep link will allow us to share a URL that will enable the receiver to open that exact shoe product page in the app in just one click by taking them directly to that screen. Let’s look at a definition so that it will be clearer:
> *Deep linking consists of using a uniform resource identifier (URI) that links to a specific location within a mobile app rather than simply launching the app. Deferred deep linking allows users to deep link to content even if the app is not already installed.*

We already went through one example in *What* part above but there can be many more use cases where a deep link can come very handy. Think of marketing strategies, referral links, sharing a certain product, etc.

The greatest benefit of mobile deep linking is the ability for marketers and app developers to bring users directly into the specific location within their app with a dedicated link. Just as deep links made the web more usable, mobile deep links do the same for mobile apps.

Let’s see how to create one in React Native. There are just 3 simple steps involved.

## Steps involved:

1. Create a Project

1. Edit AndroidManifest.xml

1. Build the Project

## Step 1: Create a project

Create a React Native project by running this command:

```
react-native init deeplinkdemo
```


## Step 2: Edit AndroidManifest.xml

We have to add `intent-filter` inside `AndroidManifest.xml` to specify the incoming links to launch this particular app.

```
<activity
 android:name=".MainActivity"
 android:label="@string/app_name"
 android:configChanges="keyboard|keyboardHidden|orientation|screenSize"
 android:windowSoftInputMode="adjustResize">
 <intent-filter>
 <action android:name="android.intent.action.MAIN" />
 <category android:name="android.intent.category.LAUNCHER" />
 </intent-filter>
 <!--Copy & Paste code from here-->
 <intent-filter android:label="@string/app_name">
 <action android:name="android.intent.action.VIEW" />
 <category android:name="android.intent.category.DEFAULT" />
 <category android:name="android.intent.category.BROWSABLE" />
 <data android:scheme="app"
 android:host="deeplink" />
 </intent-filter>
 <!--To here-->
 </activity>
```


I hope the comments are clearly specifying what to do. Let’s understand the `intent-filter` a little better.
> *An `intent filter` is an expression in an app's manifest file that specifies the type of intents that the component would like to receive.*

Get a closer look at `&lt;data&gt;` tag inside `&lt;intent-filter&gt;`. There are two properties that we have to care about. Consider `scheme` as a type of incoming link and `host` as the URL.
> *Our Deep Link will look something like this: `app://deeplink`*

Read Google Docs for more info: [developer.android.com/training/app-links/deep-linking](https://developer.android.com/training/app-links/deep-linking)

## Step 3. Build Project

Go to your root directory and run this command:

```
react-native run-android
```


Wait for the project to build and then we will test if our deep link is working correctly.

Make sure your app is in the background and run this command:

```
adb shell am start -W -a android.intent.action.VIEW -d app://deeplink com.deeplinkdemo
```


If your package has a different name then edit command as follows:

```
$ adb shell am start -W -a android.intent.action.VIEW -d <URI> <PACKAGE>
```

> Note: Take a closer look at `app://deeplink`. This is our link added inside `intent-filter` to specify our app.

If your App opened successfully then our deep linking is working as expected. Yay!

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626428546683/BKSVDKWbB.gif)

We used the `app` scheme earlier and how we are going to use the `https` scheme. Edit `&lt;data&gt;` tag inside the `intent-filter` attribute as follows:

```
<data android:scheme="https" android:host="www.deeplinkdemo.com" />
```


Run this command:

```
adb shell am start -W -a android.intent.action.VIEW -d https://www.deeplinkdemo.com com.deeplinkdemo
```


Cheers if your app appears right in front of you.

## Note:

You can use multiple `&lt;data&gt;` tags inside `intent-filter` so something like this is totally okay.

```
<data android:scheme="app" android:host="deeplink" />
<data android:scheme="https" android:host="www.deeplinkdemo.com" />
```

> *The URIs “app://deeplink” and “deeplinkdemo.com” both resolve to this activity.*

You can also create an HTML file with these two links like this and *test*:

```
<html>
<a href="app://deeplink">DeepLink with app scheme</a>
<a href="https://www.deeplinkdemo.com">DeepLink with https scheme</a>
</html>
```


Access the file via localhost or place it on the device. Click the link and this will hopefully launch your app.

This was the first part of the series *React Native Deep Linking Simplified* and in Part 2 we will learn **How to handle incoming links upon app launch and redirect user**.

Do share this article if you find it helpful.
Shad
---

---


I work at [Mutual](https://mutual.club/), a peer to peer lending Fintech in Brazil. We connect borrowers looking for fair rates to lenders looking for above-market returns. Being a very broad product with a diverse amount of users in a large country, our React Native iOS and Android apps are downloaded by all sorts of devices.

The majority of those users, however, are on low-end devices. We can see this using Facebookâ€™s `[device-year-class`](https://github.com/facebookarchive/device-year-class) library which, given a device model, shows in what year it would be considered a high-end device. For example, the most popular phone for our users is the Samsung Galaxy A10, which although was launched in March 2019, would only be considered a high-end phone in 2013. Looking at all userâ€™s devices, 85% of those would only have been high-end in 2015 or before. Because of this, we put a premium on optimizing our app so that even with a limited device, our users can still have a great experience.

![Percentage of devices considered a flagship in each year](https://cdn-images-1.medium.com/max/2000/1*PGAtjAj2pwVlnyjQNSnfIQ.png)*Percentage of devices considered a flagship in each year*

On that note, weâ€™ve more recently taken a hard look at our app size which was 26.8 MB on Android. While that size isnâ€™t completely out of control, it is definitely above our peerâ€™s median, which the Google Play Console reports as 16.3 MB. Size can be a deal-breaker for users who have limited data plans or little to no available disk space and have to choose which apps to keep or uninstall. This is especially important at Mutual since borrowers have to pay their monthly installments through the app. When a borrower uninstalls the app, their chances of paying on time decreases drastically, directly impacting investorsâ€™ returns in our marketplace.

![Mutual (26.8 MB) is much larger than peersâ€™ apps in the same segment](https://cdn-images-1.medium.com/max/2000/1*e0XXZ1aWrScU3ORywM6hiQ.png)*Mutual (26.8 MB) is much larger than peersâ€™ apps in the same segment*

Not only does app size affect uninstall rates, but it has a great impact on the **install** conversion rate also. Thereâ€™s a [great article](https://medium.com/googleplaydev/shrinking-apks-growing-installs-5d3fcba23ce2) by the Google Play team on the subject where they show the importance of shrinking your app.
> For every 6 MB increase to an APKâ€™s size, we see a **decrease in the install conversion rate of 1%**.

They go on to reveal that in developing countries, where low-end devices are the norm, this effect is even greater:
> The removal of 10 MB from an appâ€™s APK size in emerging markets correlates **with an increase in install conversion rate by ~2.5%**.

![Install conversion rate increase per 10MB decrease in APK size by country (Google internal data)](https://cdn-images-1.medium.com/max/2396/1*QP5Kh-1pz2Uo731oII92LQ.png)*Install conversion rate increase per 10MB decrease in APK size by country (Google internal data)*

We were very motivated by the potential improvements to our install conversions and uninstall rates. So we got to work on reducing our app size as much as possible without degrading the user experience. The first step was looking through the [official resources](https://developer.android.com/topic/performance/reduce-apk-size) available for Android developers.

## Android App Bundle

Reading through that page, we are told that the simplest way to reduce app size is to try out the new [Android App Bundle](https://developer.android.com/guide/app-bundle) (AAB) method of distribution. Up until this point, we were distributing our app by compiling a good old [Android Package](https://developer.android.com/guide/components/fundamentals) (APK) file that could be run in most android devices and uploading it to the Google Play Console. An AAB bundle, however, contains only your compiled code and resources. Therefore when you upload it, Google Play itself becomes responsible for generating an optimized APK for each device type, knowing its specifications and CPU architecture.

So with a simple change to our build pipeline, we can get a massive size reduction for no cost? Sounds too good to be true!

After looking through the documentation, all we did was change the React Native Gradle build script to run `bundleRelease` instead of the current `assembleRelease` . Just like that, we have our AAB file. After some further modifications to the [Fastlane](https://fastlane.tools/) configâ€™s `[supply`](https://docs.fastlane.tools/actions/supply/) action to automatically upload directly to the Play Store, we are good to go and our new reduced release shows up on the Google Play Console.

With just this change we cut 9.1 to 12.4 MB of our delivered APK size. Turns out it was really too good **and** true!

![Old APK at 26.8MB compared to the new AAB at 14.4 to 17.7 MB](https://cdn-images-1.medium.com/max/3740/1*dlw3IceAuZ3iKFgWF6AKYA.png)*Old APK at 26.8MB compared to the new AAB at 14.4 to 17.7 MB*

Be careful though: if youâ€™re using React Native with [Hermes](https://hermesengine.dev/), you might have to update your `soloader` dependency as per [this issue](https://github.com/facebook/react-native/issues/26400#issuecomment-539395814) or you risk delivering an app with a critical bug to your users. Thankfully we were able to catch this problem by testing in the alpha release track. But it couldâ€™ve easily slipped through as it doesnâ€™t show up locally or when you build an APK.

## Optimizing assets with the Android Size Analyzer

The next suggestion on the list is the [Android Size Analyzer](https://developer.android.com/topic/performance/reduce-apk-size#size-analyzer). A command-line tool that analyzes an Android app to point out opportunities for size reduction. After running it using the command `size-analyzer check-bundle [BUNDLE].aab`we receive a list of **large assets** and **images we can optimize.** We are also told to **configure [ProGuard](https://developer.android.com/studio/build/shrink-code)**.

![The output of the size-analyzer command](https://cdn-images-1.medium.com/max/2626/1*Nq5Wyh3NVxTwbsh1EgOvnA.png)*The output of the size-analyzer command*

### Proguard

Proguard is a tool to shrink, obfuscate, and optimize your Java bytecode. We have not yet explored this avenue, as weâ€™ve read about possible incompatibilities with other Android libraries. As we are looking for quick and easy size reductions, we chose to leave this optimization for the future.

### Large assets

Running the command again with a `-d` flag, we get a list of each asset ordered by size. As the `size-analyzer` tool doesnâ€™t know our appâ€™s user flow, it lets us decide which ones we can remove or bundle dynamically.

![List of large assets ordered by size](https://cdn-images-1.medium.com/max/6528/1*Shn_tgyrDkqxSWg1w3VQWw.png)*List of large assets ordered by size*

The first and largest item is the React Native JavaScript bundle. It is not currently possible to split and dynamically load this, but we will see how we can shrink this later on. Following further down the list of suggestions, we see many large fonts (TTF) and images (JPG and PNG) assets.

### Unneeded pictures

Our attention is immediately drawn to four huge JPG pictures being used in our internal [Storybook](https://storybook.js.org/) tool. They added an extra 2 MB of trash to our production APK. What an embarrassing blunder! When things like this happen we feel pretty foolish. But in the complex world of Software Engineering, we all make mistakes. I believe in sharing them with our peers so we can all learn from them. Chances are if you donâ€™t track the growing size of your applications you might be making some of these mistakes too.

### Fonts

After quickly getting rid of these large pictures, we kept looking at the rest of the list. Itâ€™s clear to see that there is an abundance of fonts that were being bundled. After talking to the design team they told us that many old components did not strictly follow the typography guidelines. So we identified which components could be removed and which could use a comparable updated font. With this, we cut down our font usage from six to four.

Another thing we noticed is that our font asset sizes are huge! They are clocking in at almost 670 KB each. which means that our four fonts represent a whopping 2.7 MB of our uncompressed bundle size. Thankfully there is a tool called [FontForge](https://fontforge.org/en-US/) which allows us to take a deeper look and modify those font files. Opening these up, we can see that the majority of the asset size can be explained by the expansive Cyrillic script and other unneeded glyphs. We can remove all of these since our app is completely in Portuguese. With this change, we shrunk our font sizes from 670 KB to 70 KB each, a 90% reduction!

![Example of some glyphs being included in our fonts](https://cdn-images-1.medium.com/max/2000/1*GsB8NbzXId-sk_ZcmLOlfA.png)*Example of some glyphs being included in our fonts*

Removing unneeded fonts and optimizing the remaining ones totaled a 3.8 MB decrease, which translates into a cool 2 MB reduction to the final APK size.

![](https://cdn-images-1.medium.com/max/2000/1*t76orb12PhgiX3921op6Ww.png)

![Before and after removing two fonts and optimizing the rest](https://cdn-images-1.medium.com/max/2000/1*3tHPqMpRyZK6V72om3hSMg.png)*Before and after removing two fonts and optimizing the rest*

### Optimizing images

Taking a look at the remaining images some of them are quite large. We ran a couple of them through an image optimizer tool ([tinyjpg](https://tinypng.com/)) and saw large reductions in size. After that, we decided to optimize all 41 JPG and PNG assets used within our app.

![Before and after of one of the images we optimized](https://cdn-images-1.medium.com/max/2688/1*G333vq4XCmnylzx1O8j6jQ.png)*Before and after of one of the images we optimized*

That brought us down from a total of 2.5 MB in image assets to 756 KB, a 70% reduction. However, because the images were not optimized, they were already being compressed in the process of generating the final APK. So, in practice, we only cut 500 KB to the end-user.

After this, we realized that weâ€™ve already depleted all of the easy low hanging fruit optimizations. All further asset optimizations would either take a lot more effort or result in only marginal improvements.

## Optimizing our React Native JavaScript bundle

Now that we are done looking at the native assets, it is time to analyze our JavaScript bundle. This is an especially good thing to optimize for three reasons. Firstly, it reduces the bundle size of our finished APK. Secondly, it also results in a faster app startup as the JS virtual machine needs to parse less code. Finally, and most importantly, it speeds up over the air (OTA) updates that we ship out multiple times a week through [CodePush](https://microsoft.github.io/code-push/).

### Bundle analyzer

To decide how we will reduce the size of our bundle, first, we need to be able to see what is taking up the most space. For this we will count on another great open-source tool: `[react-native-bundle-visualizer`](https://github.com/IjzerenHein/react-native-bundle-visualizer). Running it against our project, we get a visualization of every folder and dependency of our application and their respective sizes.

![Snapshot of the libraries and folders of Mutualâ€™s front end code base with their respective sizes](https://cdn-images-1.medium.com/max/7342/1*qvhkvFCbE9yR49fjMnwQ0w.png)*Snapshot of the libraries and folders of Mutualâ€™s front end code base with their respective sizes*

We can see that the app bundle totals 5.49 MB, with 57.8% of that being from `node_modules` dependencies, 27.5% from the application code, and the rest the tool wasnâ€™t able to map. The bundling process already removes unused code paths, so what we see here is the code that is actually used by our app. Even so, there is always still room for improvement.

The biggest dependency that we have is math.js which as the name implies implements many mathematical operations. We shouldnâ€™t need this dependency since we execute all sensitive calculations in the server and only send the results to the app. Taking a closer look at the front end code, the library was being used for some simple operations. Most likely it was used out of habit by a developer who had also worked on the back end code. We quickly extracted these methods from the library and took it into our codebase, removing the dependency entirely. This brought down the bundle size to 4.64 MB. We got a 15.5% reduction from removing one lib!

As mentioned before, we use Storybook for developing and testing components independently. However, it should only be available in the local and staging environments. No end-user should be able to see it. Because of this, we use an `ENVIRONMENT` variable to control whether we enable this part of the app. While this works for restricting access, the bundler has no way of knowing the value of this variable. Because of this limitation, all of the Storybook code ends up going to our production bundle.

To remedy this, we isolated the import of this section to a single file. Then we created two versions of the file: one that includes Storybook and another one for production that just has a dummy component. To toggle between those when targeting production, we wrote a script that runs before the bundling step that swaps the two files. Through this method we were able to completely remove the Storybook code path from production, eliminating the `node_modules` dependency as well as all internal code configuring each story.

![The updated storybook folder with two versions of the index file](https://cdn-images-1.medium.com/max/2000/1*asesI8TLiQyhcMbhYoOU3Q.png)*The updated storybook folder with two versions of the index file*

With both these changes, we were able to bring our bundle size from 5.49 MB to 4.2 MB. Meaning our users will have faster app startup speeds and update downloads.

![Final bundle totaling 4.2 MB in size](https://cdn-images-1.medium.com/max/4340/1*77bkAU-9XM9fzBMd2LI-iw.png)*Final bundle totaling 4.2 MB in size*

After all these improvements, we uploaded our app to the Play Store again. It was now reporting that our final APK size would be between 10.5 to 13.7 MB. An incredible ~60% reduction from our original 26.8 MB! This means we could potentially increase our install conversion rates by 3.75%, as indicated by the Google Play teamâ€™s article.

![Comparison between our initial APK and the final AAB version with all improvements](https://cdn-images-1.medium.com/max/3688/1*uCxcMK-ctgpB4JatQwSJOw.png)*Comparison between our initial APK and the final AAB version with all improvements*

## Closing thoughts

As business-oriented Software Engineers, we know that sometimes the best decision for the company is to accumulate technical debt to evolve the product faster. This is especially true in early-stage startups like Mutual that are trying to find their product-market fit. But without monitoring this debt, you could make some big blunders, like bundling 2 MB of testing pictures and using a huge library unnecessarily. Donâ€™t let technical debt get out of control and blow up in your face!

It is also common to tunnel vision and let pass by quick and easy opportunities to optimize what you already have. So a good idea is to periodically take a step back. Ascertain that youâ€™re not missing out on quick improvements to the size, speed, or any aspect of your application. It took us only two days to analyze, plan out, and execute all of the above improvements which brought the size of our app down by up to 60%. Itâ€™s hard to think of anything that would bring so many tangible results for so little effort.

*A special thank you to my colleagues at Mutual for proofreading and creating an all-around great environment for all of us to grow and improve every day.*
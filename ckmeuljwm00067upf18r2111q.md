## Introducing Audio Blogs on Hashnode. Now listen to articles automatically!

Hey everyone!

I am super excited to introduce **Audio Blogs** on Hashnode. Last week I came across [this guide](https://publications.iamroyakash.com/set-up-an-audio-version-of-your-blog-articles-works-automatically), which explains how to set up an audio version of your articles. We loved the concept, and got really excited. Although it's a great idea, it requires our authors to do quite a bit of work. We asked ourselves, can we make it simpler? Can Hashnode support audio blogs automatically?

After that, I had a chat with our dev team. Everyone in our team was excited and wanted to ship this fast! We discussed a little bit and figured out that this can actually be built within a week. We went ahead, and started our research. Finally, we decided that we can use two AWS services to build this.

*   Amazon Polly to generate audio
*   Amazon S3 to store the audio files

So, we worked on it and built the solution within a week. Shout out to [Vamsi](https://hashnode.com/@vamsirao), one of our amazing engineers, who did most of the work 🙌. We beta tested with some of our team members, worked on improving the audio, and optimized it further. Finally, it's ready for you to try out.

Why We Built This
-----------------

We all love to read, but as developers we are always busy! It's often difficult to find time to read. Around 30% of content on Hashnode is about general programming, culture and life around programming. So, converting text into audio makes sense. You can have hundreds of tabs open, and still be able to listen to your favorite content on Hashnode. You can listen to the interesting articles from amazing Hashnode writers, while you cook, run errands, or go for a walk. By doing this, you can definitely save more time everyday, and dedicate that to something else.

How To Enable
-------------

This feature is experimental, but you can unlock it for your blog today, if you are a [Hashnode Ambassador](https://hashnode.com/ambassador). Just visit the "Advanced" tab of your blog dashboard and enable Audio Blog. We'll start converting all your existing articles into audio format, and show an audio player on your articles. All your future articles will also be converted to audio automatically.

![Screenshot 2021-02-23 at 12.53.49 PM.png](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1616070236922/3XFqRVKAK.png)

We have restricted it to only **[Ambassadors](https://hashnode.com/ambassador)**, because we want to understand the usage and cost first, before rolling it out to everyone. This will also allow us to improve, and optimize the offering before general availability.

* * *

As always, I would love to know what you think in the comments below. We are aiming to release it for everyone in the next couple of weeks. Before that, it'll be really helpful to get some early feedback, so that we can improve further. 🙌
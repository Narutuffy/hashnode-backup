## A conversation with Athan Reines


This is the transcript of a conversation I had recently with Athan Reines, co-creator of the JavaScript [stdlib](https://stdlib.io/) library. We talked about why Athan created the library, why he needs to do scientific computing in JavaScript and the difficulties he has encountered. We also discussed the state of data science in the JavaScript ecosystem and how far off we are from equalling Python and what it would take for us to catch up.

Enjoy...

**Athan:**
Hello?

**Ash:**
Hi Athan, this is Ashley.

**Athan:**
Hey, how are you doing?

**Ash:**
Good mate. Hey thanks for agreeing to talk today about your JavaScript library stdlib

**Ash:**
I'm working on my own data transformation toolkit, that's [Data-Forge](http://www.data-forge-js.com/) and I've also written a book called [Data Wrangling with JavaScript](http://bit.ly/2t2cJu2). So I've been very interested in the JavaScript ecosystem for data transformation, analysis and visualization. But somehow your standard library sort of slipped past me until just a couple of weeks ago, when I noticed through a blog post I found. I thought I'd have a chat to you because I've only just learned about your library and I'm interested to know more about it, where it's come from, where it's going to and that sort of thing. I’m also interested in your background as well understanding how you contribute to the library.

**Athan:**
Okay, where would you like me to begin?

**Ash:**
Well, maybe you could just start by telling me a bit about what you do for a living and your background. 

**Athan:**
I work fulltime on open source, where most of my time is spent on stdlib (pronounced “standard lib”). I've been working on stdlib for a little over two years. Most, if not all of my work focuses on how to provide scientific computing facilities to web environments and Node.js. This entails providing native bindings, porting numerical algorithms to JavaScript and creating idiomatic tools and libraries tailored to the JavaScript language for use in scientific and numerical computing. 

**Ash:**
What an awesome job. It sounds like we are coming at it from similar perspectives. I used to use Python and Pandas to do my data analysis, but I was working with JavaScript and I really wanted to be able to do that same kind of thing but in JavaScript. That’s what spurred me on to do my work and eventually create my open-source library [Data-Forge](http://www.data-forge-js.com/) and my book [Data Wrangling with JavaScript](http://bit.ly/2t2cJu2). 

How do you fund yourself if you're working on open source full time - are you sponsored or something like that, how does it work?

**Athan:**
I primarily live off savings. I receive some funding through [Patreon](https://www.patreon.com/athan/), which is a form of crowdfunding. In terms of contributors, Phillip, who is the main other contributor, is a graduate student at Carnegie Mellon University. He's funded through grants. I'll also occasionally perform freelance work. Fortunately, I worked for a couple of successful startups in Silicon Valley, so I have enough financial cushion to be able to do this. 

**Ash:**
So, did you actually start the stdlib library? Are you one of founding members?

**Athan:**
Yes, that's correct. The longer story is that, when I moved back to the US from the UK after completing my PhD and working for startup in London, I worked for a startup in San Francisco called NodePrime. The company had nothing to do with Node.js, instead providing data infrastructure analytics. As part of our product, we were pulling off metrics from datacenter servers and analyzing those metrics with various analytic facilities. As we were a Node.js “shop”, so to speak, I needed to be able to do some kind of analytics on that data, so I started writing JavaScript libraries in order to be able to do analytics and transformations. 

That led to some open source work, which I worked on for about 2 years and, through that work, Phillip, who is the main other collaborator on stdlib, began working with me. We wrote well over 1,000 NPM modules that did small little things, such as special functions or Matlab like functionality. Over time our approach was not particularly sustainable. At the time, small modules were the “craze”, and the “everything is its own repository” approach just wasn't scalable for us, as we were investing a lot more time writing tooling to help manage all these packages than actually working on core library functionality. Eventually, we consolidated everything into a mono-repo which was the genesis for stdlib. We've been working on stdlib since March of 2016.

**Ash:**
Do you have any kind of like relationship with JavaScript committee or with the Node.js foundation? I’m wondering if there is anything in the works there? I guess I’m asking: will stdlib be something official?

**Athan:**
No, there is nothing official. From our standpoint, no reason exists for official incorporation into either Node.js or JavaScript. There are several reasons for avoiding official incorporation. For the most part, these reasons are detailed in the [FAQ](https://github.com/stdlib-js/stdlib/blob/develop/FAQ.md) found in the repo. So, I'll just briefly highlight some of the reasons. First, some of the concerns of the standards bodies are not our concerns. When the standard committee has thought about math at the standards level, they haven't necessarily prioritised accuracy or precision or having robust algorithms. Instead, they have wanted flexibility in choosing performance over accuracy. If you're doing something like game design or similar, you don't necessarily care that you have 16 decimals of precision. What you do care about is that your code runs relatively fast and that accuracy is good enough for things like trajectories of flying projectiles and angry birds. Forgoing accuracy is not okay, however if you want to be able to do any kind of reproducible scientific research. 

From our standpoint, we believe it is very important for us to write implementations that are robust and have high accuracy (as compared to other languages or other implementations). Ensuring that results are comparable across languages means that you can go from one language to the other and get the same thing that you would expect, which is important for scientific reproducibility. Currently, such reproducibility is not possible in JavaScript. Next, because there's no codification at the standards level for these implementations, you can go from one browser to the next and you'll get different results, which, again, is not okay with regard to scientific reproducibility. 

For us, it's very important that you have one standardised implementation that you can run your algorithms against. Now, with regard to Node.js, the reason that, we don't think that this should be included in Node.js is that we believe that node should keep its core small. That ensures flexibility and allows people to choose exactly what libraries they want and need and how they want to choose them. We don't necessarily think that bringing in all stdlib, which may have [BLAS](https://en.wikipedia.org/wiki/Basic_Linear_Algebra_Subprograms) bindings, GPU bindings, and additional things that, for your typical HTTP application, you don't care about and not important to many Node.js use cases.

The reality is that most people don't need to have all these bindings for the basic networking applications they are going to deploy in the cloud. A good chunk of stdlib functionality would simply not be of their concern. So, for us, we think that a separation here between stdlib and Node.js is perfectly fine. 

In a similar vein, consider Python, which does have a larger standard library but you don't have NumPy or SciPy included in Python, right? In Python, you seem to have a similar separation of concerns, where people should be able to bring these things in as they need them. So, while the people at the standards level and within the Node.js community have been supportive, we have no plans for any kind of official inclusion or official support from either of those two. 

**Ash:**
Okay. It's interesting that you mentioned the accuracy issues a moment ago. I've heard various criticisms against data analysis with JavaScript but nothing that particularly swayed me. But then I heard someone talking about floating-point accuracy and the fact that the only type of number we've got in JavaScript is double. Do you have any special way of working around the imprecision of very large floating-point numbers? Is that something that's on your radar as a concern?

**Athan:**
No, the same problems that plague JavaScript, e.g., that 0.1 plus 0.2 doesn't exactly equal 0.3, plagues Python, R, Julia, etc. Some of the other environments, such as Julia, have built-in support for BigFloat or BigInt and that's great. But historically, technical computing environments haven't necessarily had that for scientific computing. In terms of future support, I think there will be some things coming down the pipeline in JavaScript that will alleviate some of these concerns. 

One of them is the BigInt proposal, which is now at stage 3 and currently available in Chrome and Node.js version 10. So, there are some things for arbitrarily sized integers at least. In terms of arbitrarily sized floating point numbers, if you're going to do any kind of thing like that in JavaScript it's going to be really slow right now, so I don't really see the point. You're just going to be doing essentially manual mathematics, using strings or bit arrays, both of which are not going to be performant. 

So, for us, we think there's a lot more low hanging fruit that needs to be addressed in terms of basic data structures or basic algorithms and basic floating-point implementations before even thinking about those other, more specialised, use cases, such as BigInt and BigFloat. 

**Ash:**
Okay. Just for a second, let's assume that I'm not into JavaScript and I'm going to ask a stupid question. When you were getting started with all this why didn't you just do it in Python?

**Athan:**
Ah, well. 

**Ash:**
Other people have asked you this question?

**Athan:**
Yes, other people have asked me this question. As I mentioned, when I was at NodePrime, we didn't really have the luxury of using Python. If you're part of a small team and you are a full-stack JavaScript shop, you don't necessarily have the bandwidth to have a polyglot architecture. 

Sometimes, it's just easier to implement these things directly in JavaScript. So, that was one of the reasons. We had other reasons. For example, we needed to be able to do various data transformations. Had we used Python for data transformation and analysis, we would have incurred a performance penalty as we’d be required to shell out. Or, if we were performing analysis in a web browser, we’d have to make an HTTP request over the network to do basic data transformation, which is relatively slow and wasteful. If you can do these transformations for a data visualisation or a dashboard directly in the browser itself, you can push some of the computational load out to the “edge”, so to speak. 

Many times, when people are working on their local machines, your web application is not necessarily utilising all the local resources that it can. For example, nowadays, people have more than one core, so, if you're savvy about it, you can utilise that in browser-based applications to kind of parallelize some of your compute. 

Or, you can take advantage of the fact that IndexedDB in web browsers can access up to half of a person’s hard drive, so you can store data locally and do downsampling and upsampling of data as need be, within web browsers. 

So, for us, it just made sense to develop JavaScript computational libraries if we want to minimise the resource usage on our servers and push some of that computation out to the client. So, there were advantages for us at the time from a business/product standpoint. And another reason is that JavaScript is a fast scripting language. There are no technical reasons why you can't do scientific and numerical computing in JavaScript. 

The only reason you don't do so now is because of the lack of comparable libraries. That's it. Furthermore, it's not like we were doing anything super fancy at the time, and we found it straightforward to implement scientific computing functionality from scratch and not waste too much time. We also found that we were able to do it in an idiomatic way for JavaScript, using it's asynchronous nature, etc, that you might not be able to do as easily in a Python or Julia, etc. Additionally, we've done performance benchmarks up and down the board, and what we can say is that, for a dynamically typed language, JavaScript is faster than R or Python.

Admittedly, the language that's superior in terms of performance is Julia, but Julia is still niche, occupying a space within the technical computing ecosystem and is not something that people are choosing to run web servers out in production. In short, if you are using JavaScript, including Node.js, if you care about performance, you're going to want to do these things in JavaScript. 

**Ash:**
I think it has a lot to do with practicality doesn’t it? I mean, if you're already working in JavaScript then you just need to be able to do stuff in JavaScript. I see a lot of people interested in it and working on this sort of thing, so it's definitely getting more popular. How far off do you think we are from JavaScript possibly rivalling Python for data analysis. Do you have any thoughts on that?

**Athan:**
We're far off. There are certain things where we're not far off, but there are other things where we are. For example, if you need to compute trigonometric functions or to calculate probability distributions etc, you can do all of this now in JavaScript. The real issue is how do you create performant, native bindings to hardware optimized libraries and other existing native libraries. Creating such bindings is something that we actively work on. In short, the big gap at the moment is creating something comparable to a native library tailored to JavaScript (and Node.js). Addressing this gap is difficult because you need substantial technical expertise.

**Ash:**
That'd be a native plug-in, is that right?

**Athan:**
Yes, using Node.js terminology, native bindings take the form of “native add-ons”. We include such bindings in stdlib, but these bindings are still very much a work in progress. 

Once you have core data structures and bindings, you can start adding additional functionality such as Apache Arrow or Pandas DataFrames, which enable higher level abstractions and more advanced use cases. But we're just not there, given the amount of work that goes into actually building these things. 

The “dirty secret” behind Python, MATLAB, R, etc is that they all have bindings to, e.g., [BLAS](https://en.wikipedia.org/wiki/Basic_Linear_Algebra_Subprograms) and [LAPACK](https://en.wikipedia.org/wiki/LAPACK) and then expose various higher level APIs which invoke lower level functionality. In essence, scientific computing environments re-use a lot of these old Fortran and C/C++ libraries. Nevertheless, creating such bindings and writing such wrappers involves considerable work. And it's not just a matter of creating a file that calls lower level libraries. If you're going to do these things in an idiomatic way and need to take into account how, for example, the web works, then you need to do a little bit of reinventing the wheel and re-implementing functionality from scratch. 

But it's not just enough to create just a single binding to some large native library. If, for example, you want to bundle functionality exposed in the native library in your web application, you wouldn't be able to do that right now. The reason being that these things are either written in Fortran, or they're in C/C++, or they're calling down to something that's hardware optimised. Even with WebAssembly, you're not going to be able to run any hardware optimised code in a web browser. In the case of BLAS and LAPACK, you're going to need vanilla reference implementations, and, if you want to create small bundles (or even binaries in the case of Node.js), you need to create individual packages and modules. In which case, you need to break down these large libraries into individualised components, so that they can be consumed in a very ad-hoc fashion. And, it's not going to be the case that you're just going to be able to go, okay, well, what I'll do is I'll install NumPy and I'll simply hook into their existing native implementations and bindings. This is not going to work because Python does things in a Pythonic way, with special consideration to Python types, syntaxes, and APIs. And even if someone did manage to port NumPy, SciPy, and other libraries to, e.g., WebAssembly, doing so will introduce considerable bloat because of how those libraries are implemented (e.g., considerable glue code mapping native constructs to Python abstractions).

To summarize, if you want to be able to break these libraries down into individual components, you're not going to be able to. You're just going to end up shipping a huge binary, leading to increased network costs, and users being annoyed because your application takes too long to load. In short, there are many considerations that you have to take into account to do this well, that takes into account the nature of the web, and that takes into account the nature of how people build modern applications. It's a different kind of programming model and a different kind of design model than what authors of numerical and scientific computing libraries are typically used to. 

**Ash:**
What are we missing though? Under Node.js we can already build cross platform, native plug-ins right? Are we missing some sort of a communication protocol?

**Athan:**
No, no, no. It's literally, it's literally creating the actual bindings. It's creating the bindings, it's creating the representations, it's compiling to web assembly to make sure that these things are implemented in the correct way, when you think about web assembly package to modules. It's testing, it's examples. They are labour intensive, so it's a matter of getting people on board to contribute to actually doing these things. And we've started on our end, but you know like take BLAS for example. You have, I don't know, it's around 80 functions. So, that takes time. If you think about LAPACK, there is around 1,000 functions and so that’s just a lot. You end up doing a lot of work and it's hard to necessarily automate that, because there is no really robust way of going from Fortran to C, and then from C to some kind of idiomatic JavaScript implementation. It's just not there, so it's just a matter of just time and effort and getting people to actually contribute and do these things. 

**Ash:**
So, it sounds like the JavaScript ecosystem needs a bit more maturity. It needs to grow and solidify and people will come along and hopefully put some of these things in place?

**Athan:**
Yeah, yeah, that's definitely the case. It's a bit of a chicken and egg type situation. In order for people to do amazing things, like cool demos to attract attention, there needs to be libraries. But in order for there to be libraries, people need to be doing cool things and want to contribute. 

So, there is a little bit of a dearth right now of kind of technical expertise in order to be able to do these things well. Like, you'll see people on npm publish some kind of one off module to do whatever. But it's typically a terrible implementation. It's just not accurate, it's not robust, or they are not using stable algorithms or whatever. This can be, this is from all the way down, all the way up. 

So, if you look on npm now and you find something to compute the mean over an array, my guess is you'll find an algorithm which is not numerically stable. People will just do the textbook solution, compute the sum and then divide by the number of elements. Which is not numerically stable, so people need to have some kind of knowledge and technical expertise in order to realise that actually there are more, you can't just rely on the naïve implementation. And this is across the board. You need people that recognise what needs to be done, from a computer science and numerical computing point of view. And then have, an interest in doing that in JavaScript. For example, if you look at the numpy guys, those guys were academic. Again, look at IPython, Fernando Pres - academia. If you look at the Wes McKinney stuff, he was working at a company where they were doing like financial stuff, right? There was already technical expertise and they were implementing stuff from that point of view. 

Right now you don't have that expertise within the JavaScript community. The people doing interesting things in data science and JavaScript are like zero, right, so you have to have those people kind of come in and realise or want to take advantage of the platform. 

**Ash:**
We need more data scientist working JavaScript?

**Athan:**
Yip.

**Ash:**
Do you see that happening? Do you see more qualified data scientists coming into our arena?

**Athan:**
Yeah, I think, yes, yes, yes but it's slow right. There is people that are interested in doing machine learning and these kinds of things, and you see like efforts like deep learn, which is now Tensorflow, so you are seeing corporate buy in, into the power of the web. 

But still, it's not necessarily data scientists who are using these libraries, it's people like front end developers who want to do things like image tracking. It's not, you're not going to go to a company right down the street in San Francisco and say hey, are you doing stuff in JavaScript? 

You'll get laughed at, right, because people have all these preconceptions. So, it's a bit hard. I mean there are people, obviously the data visualisation community is huge and web technologies are used in companies across the board. You go to Uber, they have a huge data visualisation team, really smart people. They're all using web technologies, whether it's d3 or whatever the stuff that Nicholas Belmonte is doing. 

So, it's slow. You get some academic buy in, you get some people that are doing for example, like Stinsello that is trying to do a web based spreadsheet program which, has a little more power than your traditional spreadsheet. You have people that recognise this stuff and recognise the power of the web. But you don't have a critical mass of people at the present moment that recognise the power of the web platform or a willingness to invest resources in it. 

**Ash:**
I'm also very interested in machine learning in JavaScript, but that's probably a whole other conversation. 

**Athan:**
Yip. 

**Ash:**
So, where do you see the stdlib library going in the future? Where does it fit? Is the audience data scientists or is it broader than that?

**Athan:**
Yeah, I think it's broader than that. Our perspective is that we came at this from a scientific computing background. My PhD is in physics, I did a lot of time series machine learning, analysis etc. So, that's our bent and our bias. 

But we also recognise that it's not just that, it's a bit more encompassing, how we think about the project. We want to create a particular set of libraries that has consistent documentation, that has consistent benchmarks, examples etc. And this goes for everything from low level utilities to your lodash, ramda, etc. And then to doing more hard core NLP, machine learning etc. 

We see those things as not necessarily being disparate things that oh, I'm only going to use this library just for, because it has ND-arrays. We think that these things actually work together, in the sense that if you're going to be doing any kind of data analysis, you're going to need CSV parsers or you're probably going to want to use streaming libraries, or you're going to need CLI utilities to pluck various things out of JSON data structures. 

You're going to need to be able to do some functional operations on these data structures, whatever it might be. So, we see it as everything is pretty much fair game to us. As long as we see that, that it fits within the narrative of providing a standard library that people can use for whatever application that may be. So, yeah our bent is toward the scientific computing community, but we don't necessarily exclude ourselves from implementing just basic utilities for data transformation or whatever. 

**Ash:**
Well, I think that's pretty much everything I wanted to ask. One last thing though. Your library has been around a couple of years now and I hadn’t even noticed it until recently. I'm thinking that I need better ways to keep up to date on these sort of things. How do you keep up to date? Are there any particular resources you use to keep in touch with new developments in the JavaScript community?

**Athan:**
I guess there are a few people that I follow on Twitter and things like that. I keep pretty close tabs on like my Github activity feed and I know key people within the community that were doing at some point or another, scientific computing work. 

So, well I find through those kind of weak links, resources and then if there are repositories I just follow them. So, we have a huge laundry list internally of various projects that have been started either many, many years ago or new ones that people were working on. What I would say is that yeah, certain things like this will probably fall through the cracks. I don't know how much it's worth it, how much time really needs to be invested in this, because there’s a lot of noise. There are so many crappy implementations out there, so from our standpoint were hadn't really done anything for like a year and a half to really promote the project. 

We'd go and we'd talk at conferences and wherever else, and we'd plug it that way. But it wasn't, at least from our standpoint, we were trying to create as much low level functionality and we weren't looking for broad adoption. Because we recognise that we need a certain kind of critical mass in order for the library to actually show it's potential. 

So, it's only been relatively recently that we've thought about actually pushing and promoting this project within the broader community. And that's since we've kind of kept a low profile, but there are other people in the community that are doing interesting things. Obviously like all the Tensorflow guys. Ryan Dahl, obviously was doing propel, but then that got shut down because Google scooped him. And then there's the kind of canonical ones like Math.js, but they're not really doing anything innovative. 

You know, to be quite honest there is not that many interesting projects happening, there is just not. People will do one off things and it's all typically featured around deep learning at the moment. So, if you're interested in that, there's like Keras-js, there's the Tokyo ML people. There's Synaptic, which is now Neptactic and the deep learn JS that is now Tensaflow. That's where a lot of the eye candy and activity is. Not a lot of activity is happening on doing actual, good, solid computer science. 

**Ash:**
Do you think there is anyway to coordinate the community to bring these things together and get stuff moving in the right direction?

**Athan:**
Yeah, maybe. I think it's quite difficult. It's a little bit like finding a needle in the haystack, because you need the people. Obviously there are a lot of well intentioned people, but they don't necessarily have the knowledge or the wherewithal to recognise what's a good implementation from a bad implementation. 

They'll go and they'll find something on Stack Overflow and that will be it. They won't actually go and look at the academic literature and figure out okay, this is how people actually really implement these things. Also with open source work there is often ego involved. So, they are more often than not, if they're going to devote their time, they are going to devote their time to something that they do. Of course there are a lot of people that contribute to open source. They do one off drive by, kind of contributions. But they are not necessarily investing a lot of time. 

The people that are investing time are more likely to want to do their own projects and have their own way of doing it and they're not really willing to contribute to other projects. So, it's a bit hard to recruit people, to get people to kind of consolidate around a particular project. What I think will have to happen is that a project, whatever it is, maybe that's Tensorflow, maybe that's something else, will just have to win and people and that's the one that we're all going to jump on. 

I don't know if there is anything that can be done at the present moment to say like hey, let's create this committee that's now going to architect this thing. I think it's way too soon to be able to even start thinking about that. My guess is we won't see any kind of consolidation, maybe for another year, two years from now. Where people go okay, this is legitimate. 

There are these cool demos that people are doing, like machine learning. Some people have written some books on it, they've shown the potential and they're saying okay now, now is the time, this is the most promising one and now we're going to pull our eggs in the basket. At the present moment, there is no project out there that is like that. There is just not. 

So, I think it's too soon at the present moment, too nascent of an area with JavaScript, within this ecosystem, within the web space to even think about coordinating efforts or whatever else. Essentially, if people find an interesting project and they are interested, they should just contact the authors and then little bit by little bit, hopefully you get more people contributing and you get, increased momentum within the field. But, I'm not certain that much can be done at the present moment, besides people just tinkering and small evolutionary things, that hopefully one of them will kind of take off and get exponential growth. 

**Ash:**
Cool, we might wrap it up now. Is there anything else you wanted to say about standard library?

**Athan:**
No not really. Our current road map is working on low level numeric stuff. So, this is low level implementation, BLAS bindings, LAPACK bindings etc. So, while the fundamental building blocks are needed to create libraries for machine learning, deep learning etc. That's probably going to be the next few months of work. There are other things, like in data visualisation and such, that we might work on and focus on but it's really the fundamental building blocks that people need to build these larger, more performing libraries. 

But it's difficult. I think one of the biggest things that needs to happen in order for the community to start recognising it's potential is you need some kind of corporate buy-in. There needs to be some kind of like funding into this space. Otherwise it's not sustainable, it's not sustainable for people like me to be able to live off my savings and create these things in their garage kind of thing. There needs to be some kind of model for supporting initiatives in this space, in order for them to actually grow. Because the scope of the problem is so big, that it's not something you just knock off in a weekend. 

It's something that takes from months to years. We've been working on this stuff now for two years. And to create something like a numpy or a boost or whatever, it takes a lot of effort. And there needs to be some kind of way for supporting those efforts. Whether that's corporate sponsorship or grant funding etc. 

**Ash:**
Maybe that's where a committee or some sort of foundation could come in useful? Maybe for evaluating these projects? Looking at the ones with the most potential and potentially allocating funding to those ones. 

**Athan:**
Yeah, but those bodies are really few and far between. Even look at the JS foundation. They provide mentorship, they don't actually provide any kind of financial backing. You have things like the Moore Foundation, but or NumFOCUS, but those things are typically for more established projects. And there is nothing currently super well established, or in widespread usage. So, this is difficult. There is this gap right now. You don't have anything like seed or angel stage funding for open source work. And there is not a lot of interest yet in academia for sponsoring this type of thing. 

People in academia will just say, well we do everything in R. Why do we need to worry about doing anything in JavaScript. So, it's a bit hard. It's a bit difficult and I don't know if there's much interest there even is at the kind of like upper echelon of even having a committee to begin with. So we're in this tough space, where people have to knuckle down and really, really believe in the potential of the web platform. And then put in the time and effort to make it happen. 

**Ash:**
Cool. 

**Athan:**
So, I think that wraps up. I don't know if you have any other questions or anything?

**Ash:**
No, I think that's about it. Yeah, thanks for taking the time for a chat. Great work so far on your stdblib library, it's looking really impressive. And I'm keen to watch where it goes now. And hopefully help transform the JavaScript ecosystem into a contender.

## Resources

- [stdlib](https://stdlib.io/)
- [stdlib FAQ](https://github.com/stdlib-js/stdlib/blob/develop/FAQ.md)
## Paint Your Github Profile with Serverless

I'm often asked things like "What should I make?" or "Where do the ideas come from?". I've [covered how I generate ideas before](https://dev.to/jh3y/playfulness-in-code-supercharge-your-learning-by-having-fun-39hf). The gist being, write down all your ideas, great or small.

This works great for demos. But what about when you want to learn something a little more applied? Like putting together a project or trying out more tools.

One thing I advocate is building tools. Tools that you want to use. Tools that solve a problem for you. That's right, make for yourself.

This has many benefits:

*   You're invested in the idea.
*   You get to learn many things to solve your problem.
*   You have something to show potential employers/clients that's different.

That last point could be particularly useful. Interesting side projects make for good talking points. I can't tell you how many times I've had comments because of my [Github profile](https://github.com/jh3y). Because the hiring staff check it out and see an image painted in the contributions graph.

Today, we're going to walk through a project I made last year. "[Vincent van Git](https://github.com/jh3y/vincent-van-git)" gives you a way to paint your Github contributions graph. I want to cover the "What?", the "Why?", and the "How?".

> [Check out this tweet!](https://twitter.com/jh3yy/status/1324811198857125890?ref_src=twsrc%5Etfw)

* * *

What?
-----

As mentioned above, "Vincent van Git" helps you paint your Github contributions graph. It's a web app that generates a shell script for you to run on your machine. The result is that you populate your graph with commits that paint a picture. Over time (around 3 months), that picture will move and you'll need to recreate it.

![Github profile for jh3y with painting](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1615808238395/2Q-oedG0X.png)

Why?
----

This part's split into two, "Why make it?" and "Why make it?" ha.

First. Before making "Vincent", I'd always used the package "[gitfiti](https://github.com/gelstudios/gitfiti)". It's a command-line tool for applying graffiti to your contributions graph. It uses Python and you draw images with Arrays.

    KITTY = [
      [0,0,0,4,0,0,0,0,4,0,0,0],
      [0,0,4,2,4,4,4,4,2,4,0,0],
      [0,0,4,2,2,2,2,2,2,4,0,0],
      [2,2,4,2,4,2,2,4,2,4,2,2],
      [0,0,4,2,2,3,3,2,2,4,0,0],
      [2,2,4,2,2,2,2,2,2,4,2,2],
      [0,0,0,3,4,4,4,4,3,0,0,0],
    ]

If you squint hard enough, you'll see the kitty. But, the fact it's a non-visual tool for a visual result made it tricky for me to use. It's a great tool, don't get me wrong. But, I always wanted a visual way to make my creations.

Now, I could've created a front end to generate that Array. And then used it with gitfiti. But, why stop there? Why not have a go at creating my own version from scratch?

This leads us to the second "Why?". Because there's an opportunity to learn a variety of different tools here. There's also the opportunity to try new things out. And this goes back to the point we made in the introduction. With side projects that aren't the norm, you get to solve problems that aren't the norm. And that will help you develop your skills as a problem solver.

Before diving into the things learned and how. Here are some of the things I got to try out more.

*   [`react-hook-form`](https://react-hook-form.com/)
*   [`luxon`](https://moment.github.io/luxon/)
*   [`cheerio`](https://cheerio.js.org/)
*   [`electron-store`](https://github.com/sindresorhus/electron-store)
*   [`electron-dl`](https://github.com/sindresorhus/electron-dl)
*   [`tone.js`](https://tonejs.github.io/)

They aren't likely to pop up in a tutorial CRUD app. That's not to say we shouldn't follow those tutorials when we're starting out. But, when we start looking for "What's next?", there are advantages to being adventurous.

![Spider Man pointing at Spider Man meme with the words "Todo App by X" and "Todo App by Y"](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1615808240675/fUIKolWql.jpeg)

How?
----

It's time for "How?". I'm going to break this part down into different sections. I won't dig in too deep but I will go over how certain things are possible. The talking points so to speak.

### Electron

I had it in my head I wanted to create an `electron` app for "Vincent". A desktop app I could fire up, draw something, and hit "Submit". It didn't pan out that way but that's how it started.

And this was a key part of the project. I had chosen to use `electron` because I wanted to make a React app that could use Node on the user's machine. That would provide a way to invoke "git" from within `electron`.

I hadn't played with this idea much before but it was a chance to get familiar with the [ipcRenderer](https://www.electronjs.org/docs/api/ipc-renderer). That's a way you can communicate between the `renderer` and the `main` process. That means you can hit a button in React world and fire a function in Node world.

I put together [this repo](https://github.com/jh3y/electron-parcel-react-starter) that shows how this is possible. On OSX, if you press a message button in the front end, it uses `say` on the command line to read out the message.

### Front End

I had a good idea of what I wanted here. We needed a grid that resembled the Github contributions graph. A user can use their pointer to paint the grid. Each cell can either be transparent or one of four shades of green. Here's what the final grid looks like.

Check out [this pen](https://codepen.io/jh3y/pen/ExNdwEy) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

The tricky part with these types of interaction and React is that we don't want to update the state on every paint. That would cause lots of rerendering. Instead, we can use refs to keep track of what's going on.

Making something different challenges us to use the tools we use in a different way. Something like Vincent is great for working with DOM manipulation and React. I've done this for other projects too like ["PxL"](https://pxl.netlify.app).

This part of the project was all about generating the Array we mentioned earlier. We're giving the user a way to generate the Array of digits from 0 to 4 without having to type it out.

### Web Scraping with Serverless

Now, what makes "Vincent" possible is empty commits. The way it works is that we generate hundreds of empty commits and commit them to a repository of your choice. And those empty commits show up in the contribution graph.

How do you get the four different greens? Well, this depends on the amounts of commits. For example, if we say your max commits per year is 100. Then to get the 4 levels, we can use 400, 300, 200, and 100 commits per day. That will generate the four shades of green.

The main thing we need is the max number of commits for the username. To grab that we make some checks and then scrape the activity page on Github. In "Vincent", we ask for a user name, branch name, and repository name. "Vincent" checks that they exist and that they're empty before scraping for commits.

We're making about 4 or 5 requests here. This is where serverless comes in handy. We can put them requests into a [Netlify function](https://www.netlify.com/products/functions/) and then the front end only needs to make one request.

This is the important part of that function. Here we make a request for the "contributions" page. And then we use `cheerio` to scrape for the highest amount of commits over the last year.

    const getCommitMultiplier = async (username) => {
      // Grab the page HTML
      const PAGE = await (
        await fetch(`https://github.com/users/${username}/contributions`)
      ).text()
      // Use Cheerio to parse the highest commit count for a day
      const $ = cheerio.load(PAGE)
      // Instantiate an Array
      const COUNTS = []
      // Grab all the commit days from the HTML
      const COMMIT_DAYS = $('[data-count]')
      // Loop over the commit days and grab the "data-count" attribute
      // Push it into the Array
      COMMIT_DAYS.each((DAY) => {
        COUNTS.push(parseInt(COMMIT_DAYS[DAY].attribs['data-count'], 10))
      })
      // console.info(`Largest amount of commits for a day is ${Math.max(...COUNTS)}`)
      return Math.max(...COUNTS)
    }

You could create a local version of this too and parse the response. Try making that request with your own username.

### Generating a Shell Script

Next up we need a shell script to push all these generated empty commits. This part is about creating a big string in a loop. For every commit, we are assigning a date and many commits based on the draw level.

The first part requires the use of `luxon` ([We don't need `moment.js` anymore](https://momentjs.com/docs/#/-project-status/)) to match dates to commits. There is a little Math around the dates that was a little tricky on the first couple of tries. But once it's sussed, your good!

    const processCommits = async (commits, multiplier, onCommit, dispatch) => {
      const TODAY = DateTime.local()
      const START_DAY = TODAY.minus({ days: commits.length - 1 })
      let total = 0
      let genArr = []
      for (let c = 0; c < commits.length; c++) {
        const LEVEL = commits[c]
        const NUMBER_COMMITS = LEVEL * multiplier
        total += NUMBER_COMMITS
        genArr.push(NUMBER_COMMITS)
      }
      // Dispatch a message.
      dispatch({
        type: ACTIONS.TOASTING,
        toast: {
          type: TOASTS.INFO,
          message: MESSAGES.TOTAL(total),
          life: 4000,
        },
      })
      // Loop through the commits matching up the dates and creating empty commits
      for (let d = 0; d < genArr.length; d++) {
        // Git commit structure
        // git commit --allow-empty --date "Mon Oct 12 23:17:02 2020 +0100" -m "Vincent paints again"
        const COMMITS = genArr[d]
        if (COMMITS > 0) {
          const COMMIT_DAY = START_DAY.plus({ days: d })
          for (let c = 0; c < COMMITS; c++) {
            onCommit(COMMIT_DAY.toISO({ includeOffset: true }))
          }
        }
      }
    }

Once we have all the commit data ready it's time to generate that script. It's a long string based on the commit dates, the username, branch, etc.

    const generateShellScript = async (
      commits,
      username,
      multiplier,
      repository,
      branch,
      repoPath,
      dispatch
    ) => {
      let SCRIPT = `mkdir ${repoPath}
    cd ${repoPath}
    git init
    `
      await processCommits(
        commits,
        multiplier,
        (date) => {
          SCRIPT += `git commit --allow-empty --date "${date})}" -m "Vincent paints again"\n`
        },
        dispatch
      )
      SCRIPT += `git remote add origin https://github.com/${username}/${repository}.git\n`
      SCRIPT += `git push -u origin ${branch}\n`
      SCRIPT += `cd ../\n`
      SCRIPT += `rm -rf ${repoPath}\n`
      return SCRIPT
    }

### Ditching Electron

> "Wait. I thought you wanted to use electron?" – Reader

I did.

I got quite far with it. But, I hit some blockers, and that's OK. The issues were around pushing the commits via Node. It would take a long time and sometimes run out of buffer. The other issue was that I couldn't communicate this to the front end in a clean way.

This is why I started generating the shell scripts. And I'd started digging in with `electron-dl` and `electron-store` when it hit me. "This belongs on the web".

I'd only read up on how to package a desktop app for different platforms and it looked OK. But, from testing and feedback, there were some issues already with Windows.

There was also the factor of usability. This isn't a tool you use every day. And the web is more accessible than downloading and installing an app, etc.

I decided to ditch electron at this point. And this is where React is great. Because I'd created various building blocks for the front end, it was painless to port those into a web app.

Was it a waste of time? No!

> [Check out this tweet!](https://twitter.com/jh3yy/status/1322184887936450563?ref_src=twsrc%5Etfw)

Because I didn't use electron for the final product, doesn't mean it was a waste of time to try. In fact, I learned a lot about `electron` in a short space of time which was neat.

### UI Fun

At this stage, I had a working proof of concept 🙌

Now I could have some fun with it and put together all the conveniences for users. A form to configure, the ability to save and load drawings, animations, etc.

These are the things that stood out for me.

#### Configuration

I needed forms for configuration. Somewhere for a user to put their username, branch, and repository information. But, I also wanted to create a sliding drawer effect.

For form handling, I could've reached for `formik` or created the form handling myself. But instead, I thought I'd give `react-hook-form` a try and it was great. It was another opportunity to try something different. Here's how the sliding drawer looks.

Check out [this pen](https://codepen.io/jh3y/pen/VwmEMeP) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

The other benefit to building things like this is that you can look for patterns to refactor. This drawer became a reusable component. I reuse it for an "info" drawer on the right side in the app.

#### Audio

I like to add a little whimsy to my projects. It's something people associate with me. Sound was a must and I hooked up some button clicks and actions to audio with a quick custom hook.

    import { useRef } from 'react'
    
    const useSound = (path) => {
      const soundRef = useRef(new Audio(path))
      const play = () => {
        soundRef.current.currentTime = 0
        soundRef.current.play()
      }
      const pause = () => soundRef.current.pause()
      const stop = () => {
        soundRef.current.pause()
        soundRef.current.currentTime = 0
      }
      return {
        play,
        stop,
        pause,
      }
    }
    
    export default useSound

But, the real joy would be audio when painting the grid. I wanted to try out Tone.js some more after seeing it on ["Learn with Jason"](https://www.learnwithjason.dev/get-weird-with-audio-on-the-web). And this seemed like a great opportunity. Different levels play different notes. Erasing plays a dull note.

Check out [this pen](https://codepen.io/jh3y/pen/BaQqRXE) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

#### Toasts

The app needed some little toast components to let the user know what's happening. For example, confirming a save or telling the user that the commits are being generated.

I could've reached for off-the-shelf ones. But, I couldn't remember making any myself in open source. This felt like a good opportunity to do that. With a little React and GreenSock, I had a nice Toasts component. The neat thing about creating a Toast component is that it makes you think more about components. You need to use the state to trigger creation. But, you don't tie state to the Toasts. It's worth checking the code on that one.

Check out [this pen](https://codepen.io/jh3y/pen/ZEBqXVb) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

#### Animation

I love to put some animation somewhere. And with this being my own project I can put as much as I like in.

What better than a loading animation when the shell script gets generated? Playing on the project name and writing code, I settled on this.

Check out [this pen](https://codepen.io/jh3y/pen/eYzvQzq) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

Some audio and 8-bit style music tops it off!

#### Zip Files

If you try and download a shell script for users, you're prompted with a security warning. It's not something I've needed to do before and this was new to me.

The audience on live stream suggested trying out `jszip`. And this solved a problem in a neat way. Using `jszip` I could bundle a `README` and the shell script for the user and have them download a single zip file. This way the user has instructions to run the file too.

    const FILE = new zip()
    FILE.file('vincent-van-git.sh', SCRIPT)
    FILE.file('README.md', README)
    const ZIP_FILE = await FILE.generateAsync({ type: 'blob' })
    downloadFile(ZIP_FILE, 'vincent-van-git.zip')

This was convenient and another opportunity to try something new that I wouldn't have.

That's It!
----------

I deployed it, made a quick video, and shared it! All the code is open source. And you can use [the app](https://vincent-van-git.netlify.app) to paint commits to your Github profile with serverless. I learned a bunch from creating "[Vincent van Git](https://vincent-van-git.netlify.app)". And it solves a problem for me. There were techniques for me to try and opportunities to try out different packages.

What's the actionable advice here?

Make for yourself. That's the actionable advice here. Make something that you will find useful. Make a tool or something you're interested in. It could solve a particular problem for yourself. It will likely solve a problem for others too. And it gives you an outlet to learn and try new things.

Make for yourself.
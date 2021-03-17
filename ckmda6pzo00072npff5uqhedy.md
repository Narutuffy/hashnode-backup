## Pug in 5 Minutes

What is it?
-----------

A template engine for node and browser environments.

It uses an indent sensitive syntax allowing you to write clean markup with less code 😎

For those in camp **TL;DR**, scroll down for demos! 😎

Jumping in 👟
-------------

An element follows this structure

    input#check.checkbox(checked="true")

*   Text at the start of a line with no special character prefix is treated as a tag. If no tag is defined, `pug` defaults to `div`
*   Define classes prefixed with `.`
*   Define an id prefixed with `#`
*   Define attributes optionally comma-separated within the brackets

    <input class="checkbox" id="check" checked="true" />

If we wanted a `div` with the class `flower`, we only need

    .flower

You can write comments with `//`(included in output) and `//-`(not included in output).

Nesting content
---------------

To nest an element, indent it!

    .parent
      .child
        .grandchild

    <div class="parent">
        <div class="child">
            <div class="grandchild"></div>
        </div>
    </div>

Think of the keystroke savings! 🙏

If you need to include plain text within an element, suffix with `.` 👍

    script.
      if (isAwesome(pug))
        return "Hell yeah!"

Inheritance via extends and blocks
----------------------------------

Pug supports template inheritance via `extends` and `blocks`. The common example is a layout extension.

    //- layout.pug
    html
      head
        title Awesome site
      body
        block content

    //- home.pug
    extends layout.pug
    block content
      h1 Welcome to my awesome site!

Giving us

    <html>
      <head>
        <title>Awesome site</title>
      </head>
      <body>
        <h1>Welcome to an awesome site!</h1>
      </body>
    </html>

Includes
--------

To stop our pug files from growing out of control, we can split them into separate files and `include` them.

Consider a layout where we "include" a menu section of markup.

    //- layout.pug
    html
      head
        title Some awesome site!
      body
        include menu.pug
        main
          block content

    //- menu.pug
    nav
      ul
        li
          a(href="/") Home
        li
          a(href="/about") About

    <html>
      <head>
        <title>Some awesome site!</title>
      </head>
      <body>
        <nav>
          <ul>
            <li><a href="/">Home</a></li>
            <li><a href="/about">About</a></li>
          </ul>
        </nav>
        <main></main>
      </body>
    </html>

Inline code 🤓
--------------

You can use valid JavaScript within Pug templates. There are various ways to do this.

*   `Unbuffered` - Code prefixed with `-` is not included in the output
*   `Buffered` - Code prefixed with `=` is evaluated and output is included

    - const random = Math.floor(Math.random() * 10)
    .number= `Random number is ${random}`

    <div class="number">Random number is 4</div>

This opens up a bunch of possibilities we'll explore in our example! 😎

Interpolation
-------------

Need to interpolate a variable? There are two ways. You could use Pugs interpolation operator `#{}`. But, if you're using inline code, you could also use unbuffered code 😎

    - const name = 'Geoff'
    .greeting Hey #{name}!
    .greeting= `Hey ${name}!`

    <div class="greeting">Hey Geoff!</div>

Conditionals
------------

Pug provides conditional operators that feel familiar to those we use elsewhere. Alternatively, we could use `Unbuffered` code to achieve the same result 😎

    - const balance = 100
    if balance >= 50
      span Nice!
    else if balance >= 0
      span Cool
    else
      span Uh oh!

    <span>Nice!</span>

Iteration
---------

Two main operators for iteration in Pug are `each` and `while`.

    ul.week
      each day in ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat']
        li.week__day= day.toUpperCase()

    <ul class="week">
      <li class="week__day">SUN</li>
      <li class="week__day">MON</li>
      <li class="week__day">TUE</li>
      <li class="week__day">WED</li>
      <li class="week__day">THU</li>
      <li class="week__day">FRI</li>
      <li class="week__day">SAT</li>
    </ul>

    - let b = 0
    while b < 5
      .balloon
      - b++

    <div class="balloon"></div>
    <div class="balloon"></div>
    <div class="balloon"></div>
    <div class="balloon"></div>
    <div class="balloon"></div>

As with conditionals, we could use `Unbuffered` code to achieve the same results 👍

Mixins
------

Mixins are a powerful feature of Pug. They're reusable blocks of Pug that can either be static, accept params, or take blocks.

We invoke a mixin with `+`.

When we find repeating patterns in our markup, it could be time for a mixin!

Here's a static mixin.

    mixin socials
      li
        a(href='https://dev.to/jh3y') Check out some articles!
      li
        a(href='https://codepen.io/jh3y') Check out some code!
    
    footer
      ul
        +socials

    <footer>
      <ul>
        <li><a href="https://dev.to/jh3y">Check out some articles!</a></li>
        <li><a href="https://codepen.io/jh3y">Check out some code!</a></li>
      </ul>
    </footer>

That's cool but mixins that accept params will be more useful 💪

    mixin card(name, avatar = 'https://placehold.it/400x400')
      .card
        img.card__image(src= avatar)
        h2.card__title= name
    
    +card('Geoff', 'https://some-image.com/geoff.png')
    +card('Jack')

    <div class="card">
      <img class="card__image" src="https://some-image.com/geoff.png" />
      <h2 class="card__title">Geoff</h2>
    </div>
    <div class="card">
      <img class="card__image" src="https://placehold.it/400x400" />
      <h2 class="card__title">Jack</h2>
    </div>

Notice how we can also provide default values for those params! 🤓

If you want a `mixin` but need different nested markup for certain cases, then a mixin block will work.

    mixin card(name, avatar = 'https://placehold.it/400x400')
      .card
        img.card__image(src= avatar)
        h2.card__title= name
        if block
          block
    +card('Stu', 'https://stu.com/avatar.png')
      .card__badge User of the month!

    <div class="card">
      <img class="card__image" src="https://stu.com/avatar.png" />
      <h2 class="card__title">Stu</h2>
      <div class="card__badge">User of the month!</div>
    </div>

Hot tip! 🔥
-----------

You can use JavaScript template literals for inline styles to generate dynamic demos 😎

An example - Randomly generated flowers 🌼
------------------------------------------

Let's put some techniques into practice. Here's a styled up flower.

Check out [this pen](https://codepen.io/jh3y/pen/wvKxzdQ) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

Quite a bit of pug there 👎

    .flower
      .flower__petal.flower__petal--0
        div
        div
        div
        div
      .flower__petal.flower__petal--1
        div
        div
        div
        div
      .flower__petal.flower__petal--2
        div
        div
        div
        div
      .flower__petal.flower__petal--3
        div
        div
        div
        div
      .flower__core

Let's refactor that! 😎

    mixin flower
      .flower
        - let p = 0
        while (p < 4)
          .flower__petal(class=`flower__petal--${p}`)
            - let s = 0
            while (s < 4)
              div
              - s++
          - p++
        .flower__core
    +flower

Check out [this pen](https://codepen.io/jh3y/pen/GRpBjyG) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

That's great! But we could take it further. Let's generate random inline CSS variables for our flower. We could define its position with a generated inline `--x` and `--y` 😎

Check out [this pen](https://codepen.io/jh3y/pen/RwWBGyy) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

Example markup generated with a random `--x` and `--y` 👍

    <div class="flower" style="--x: 1; --y: 85;">...</div>

Once we start leveraging `Pug` and `CSS` together like this, it opens up a bunch of possibilities. Check this out!

Check out [this pen](https://codepen.io/jh3y/pen/XWmBjOp) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

We utilize a `while` loop and generate random characteristics to be passed into each flower element 🤓

    - let f = 0
    while f < 50
      - const x = randomInRange(0, 100)
      - const y = randomInRange(0, 100)
      - const hue = randomInRange(0, 360)
      - const size = randomInRange(10, 50)
      - const rotation = randomInRange(0, 360)
      - const delay = f * 0.1
      +flower(x, y, hue, size, rotation, delay)
      - f++

That's it!
----------

In 5 minutes you now know enough `Pug` to be well on your way to speeding up your markup generation.

You can also leverage some of `Pug`s awesome features to speed things up, mitigate errors, and randomly generate demos! 🔥

Have fun!

All the demos in this article are available in this [CodePen collection](https://codepen.io/collection/XEoWvj).
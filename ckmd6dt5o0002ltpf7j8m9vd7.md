## Guide to BEM

I use BEM where I can and when it’s appropriate. That last piece is important. BEM is **not** appropriate in **every** scenario. I’m not preaching that everyone should go out and BEM, SMACSS etc. all their code. Classname standards are important though and and deserve time and consideration. Good classnames are going to make things a lot easier for developers new to a code base.

What is BEM?
------------

    .btn {} //block
    .btn__txt {} //block element
    .btn--lg {} //block modifier
    .btn__txt--red {} //block element with modifier

    <button class="btn btn--lg">
      <span class="btn__txt"></span>
    </button>

**BEM** stands for _block, element, modifier_. What are these three components?

*   **block** — Represents a high level component that may comprise of many smaller elements. Rendered in isolation or with others.
*   **element** —Aan element is a building block of a block denoted by two underscores. It has a dependency on it’s block. In some cases a mutual dependency if an element provides important context for a block.
*   **modifier** —Express different aesthetic effects and states of elements and blocks. Denoted by two hyphens.

An analogy? A sports team(please suggest better).

The team is the block.

*   A player is an element.
*   A players’ position could be the modifier.

    .team {}
    .team__player {}
    .team__player--forward {}
    .team__player--defender {}

* * *

BEM aids with readability for not only your CSS, but your markup too. If I was new to a repo and saw the following;

    <button class="btn btn--lg">
      <span class="btn__txt btn__txt--red">push me!</span>
    </button>

We might assume from reading the markup that I have a large variation of a button with red text saying “push me!".

It’s likely there are variations for small and other text colors. If not, it is clear how we would go about introducing them. The element hierarchy/dependency of a block is intuitive.

The features of BEM promote flat specificity. They allow us to tackle complex specificity, in some cases eradicating it altogether.

    .btn .txt {} => .btn__txt {}

There may be scenarios where you can’t quite BEM it. You’re unable to escape levelled specificity or something doesn’t quite sit right. Don’t give up at the first attempt. The power of BEM is that it makes you consider your markup and CSS. One blocks rule should not interfere with nor override another. It may be that you need to introduce a new element to your block or think about adding a modifier to a sub-block. In my honest opinion, try your best to avoid sub-elements.

BEM can get quite opinionated when not so simple use cases arise. That’s one of the pitfalls with BEM. There is no strict right or wrong. Knowing when it’s necessary and how to use it is BEMs trickiest aspect.

At the very least, attempting to use BEM or any classing standard for that matter is a step in the right direction. It can improve your code leaps and bounds if you’ve got into a bit of a state with your CSS and markup.

For a high level intro, that’s pretty much all you need to know to get started with BEM.

* * *

Working observations
--------------------

It’s when you start working with BEM and tackling real projects that questions regarding BEM usage arise.

### Many modifiers, much ugliness

What if you have several modifiers for a block or element? You don’t want to end up with several large bloated classnames. If the modifiers are small then do you need to write a big extra class for them? How about the following modifiers?

    .btn.small {}
    .btn.medium {}
    .btn.large {}

Is this right or is this wrong? It’s only a little modifier and we save having to write long classes in our markup.

We should not be doing this. The classes “small", “medium", and “large" offer no context. They could get altered or have side effects if we say wrote CSS for those classes elsewhere.

Say we had the following CSS;

    .small { background-color: red; }

Now our small layout panels become red and we don’t want that. Our class isn’t specific enough in our markup, we now have red panels 😢

    .btn.small { background-color: green; }

We could add a specific `background-color` declaration to our nested classes but now it’s getting messy 😕

This is why taking time to determine our elements and modifiers is important. ⏲

Consider this;

    .btn--small { height: 10px; }

This means instead of our previous markup which could be;

    <button class="btn small"></button>

We now have;

    <div class=”btn btn--small”></div>

This is nice with small convenient examples like “**.btn**". This get’s a little bloated when we have larger block names 😅

Consider;

    <div class="super-block small red"></div>
    /* BEMifying to */
    <div class="super-block super-block--small super-block--red"></div>

We are only using two modifiers and our classes are becoming larger. They don’t have to though. We can change them by defining appropriate abbreviations. How about the following?

    <div class=”sb sb--sm sb--red”></div>

That now has a smaller footprint than our original markup! And still retains meaning if we understand the abbreviations 👍

### Preprocessors and &

I write very little `CSS` without using a preprocessor. Personally, I’m a fan of `Stylus`. When using `BEM`, preprocessors can be pretty useful 💪

We would not want the following code to be compiled. This goes against `BEM` and causes unnecessary bloat.

    .btn {
      .btn__txt {}
    }

This code, would actually go against our BEM ideals producing

    .btn .btn__txt {}

Luckily, the ampersand operator comes to the rescue!

    .btn {
      &__txt {}
    }

If you are writing vanilla CSS, or non-nested \[_insert flavor of choice_\] syntax, you’re unlikely to fall into this trap whilst trying to keep your specificity as flat as possible.

    /* VANILLA CSS */
    .btn {}
    .btn--red {}
    .btn__txt {}
    /* NESTED */
    .btn {
      &--red {}
      &__txt {}
    }

### BEM Preprocessor Mixins? 🤓

Most preprocessors have some form of mixin functionality. Leveraging this can really help with visually understanding any `BEM` in your code. Consider the following `Stylus` code;

    // RED MODIFIER BLOCK
    red = {
      color: red;
    }
    // TXT ELEMENT BLOCK
    txt = {
      font-size: 10px;
    }
    // ELEMENT MIXIN
    el(elName)
      define(‘content’, lookup(elName))
      &__{elName}
        {content}
        {block}
    // MODIFIER MIXIN
    mod(modName)
      define(‘content’, lookup(modName))
      &--{modName}
        {content}
        {block}
    /**
     * OUR ACTUAL CODE
    */
    .btn {
      padding: 5px;
      el('txt');
      mod('red');
      +mod('green') { // BLOCK MIXIN
        color: green;
      }
    }

This would output;

    .btn {
      padding: 5px;
    }
    .btn__txt {
      font-size: 10px;
    }
    .btn--red {
      color: #f00;
    }
    .btn--green {
      color: #008000;
    }

Not everyone will be using Stylus. But the same concept can work with the preprocessor of your choice. That is of course, feature . It’s an interesting approach.

### Nested elements

How do I tackle nested elements?

    <nav class="nv">
      <li class="nv__item">
        <a class="nv__item__link">LINK</a>
      </li>
    </nav>

Not quite.

You’re looking to avoid scenarios like this ideally. In this case, items and links should both be their own elements.

    <nav class="nv">
      <li class="nv__item">
        <a class="nv__link">LINK</a>
      </li>
    </nav>

If you needed to be specific about a link within an item. You may look to introduce a modifier to tackle this.

    <nav class="nv">
      <li class="nv__item">
        <a class="nv__link nv__link--header">LINK</a>
      </li>
    </nav>

You may decide that a link is not even an element of nav and you want to just introduce modifiers for links within the nav.

    <nav class="nv">
      <li class="nv__item">
        <a class="link--nv">LINK</a>
      </li>
    </nav>

That’s one of those fun/tricky parts of `BEM`, emphasising there is no 100% right or wrong way when working with `BEM`. It’s a convention to help you out!

That’s it!
----------

A quick look at BEM. Might not be appropriate in every scenario but can aid in making your source user friendly.
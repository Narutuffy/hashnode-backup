## Why GitHub Copilot is not a Threat to your Job

> TL;DR: If you are a good software designer Copilot will not help you very much.

What is GitHub Copilot?
=======================

GitHub Copilot is an AI pair programmer.

It was trained with a huge coding database of common small routines.

It also can recognize bad comments and create imperative code from them.

[https://maximilianocontieri.com/code-smell-05-comment-abusers](https://maximilianocontieri.com/code-smell-05-comment-abusers)

GitHub copilot is a text transformer similar to GPT-3.

It was developed by the same company: OpenAI.

[https://maximilianocontieri.com/ive-recently-learned-about-gpt3-this-is-my-journey](https://maximilianocontieri.com/ive-recently-learned-about-gpt3-this-is-my-journey)

How does it work?
=================

The OpenAI Codex engine powers GitHub Copilot.

It was trained with a lot of source code and also natural language.

To use it, we must apply to their [waiting list](https://copilot.github.com/). The approval process is fast.

We add it as a Visual Studio Code Extension that interacts in real-time with GitHub.

Benefits(?)
===========

Autofill
--------

Copilot can predict anemic structures once we describe their accidental data.

[https://maximilianocontieri.com/code-smell-70-anemic-model-generators](https://maximilianocontieri.com/code-smell-70-anemic-model-generators)

They are suitable for implementative and anemic code generation.

[https://maximilianocontieri.com/code-smell-01-anemic-models](https://maximilianocontieri.com/code-smell-01-anemic-models)

Code wizards are a present problem. Copilot is a brand new one.

[https://maximilianocontieri.com/lazyness-ii-code-wizards](https://maximilianocontieri.com/lazyness-ii-code-wizards)

Bad comments to code
--------------------

It converts bad comments (those that should never be present in our code) to straightforward algorithms.

We can assume that the training set was filled with bad implementative commented code. We shouldn't rely much on the algorithm's declarative.

Structural tests
----------------

CodePilot can generate tests on setters. These tests are coupled to implementation and fragile.

[https://maximilianocontieri.com/code-smell-52-fragile-tests](https://maximilianocontieri.com/code-smell-52-fragile-tests)

They test our getters, so they don't add much value to validating our system's behavior.

[https://maximilianocontieri.com/code-smell-68-getters](https://maximilianocontieri.com/code-smell-68-getters)

More insights [here](https://goldedem.hashnode.dev/github-co-pilot-in-a-nutshell).

Should we worry about it?
=========================

Not now.

If you read the benefits above, most of the Copilot code belongs to the [code smell area](https://maximilianocontieri.com/how-to-find-the-stinky-parts-of-your-code).

Very soon, transformers like Copilot will replace lazy and implementative programmers.

[https://maximilianocontieri.com/most-programmers-are-losing-our-jobs-very-soon](https://maximilianocontieri.com/most-programmers-are-losing-our-jobs-very-soon)

What should be doing right now?
===============================

We need to be cleverer than it.

We need to create great behavioral models far from implementative structural data.

[https://maximilianocontieri.com/the-one-and-only-software-design-principle](https://maximilianocontieri.com/the-one-and-only-software-design-principle)

The problem copilot is solving right now tackles software main mistakes. Thinking of programming as just dealing with data instead of behavior.

[https://maximilianocontieri.com/what-is-wrong-with-software](https://maximilianocontieri.com/what-is-wrong-with-software)

Once we decide to grow up and build serious software instead of dealing with strings, dates and arrays, we will push our jobs a few years away from this fancy robot.

Please do write me a line below with your thoughts on this.
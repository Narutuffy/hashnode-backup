## How to align Align and Justify your flex item in the first try


The idea is to choose a topic that you were having difficulty with and you think others might find it a little difficult too. Take a look at this tweet by [@erinfranmc](https://twitter.com/erinfranmc)

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626262028335/6Thm44fIo.png)

Hmm, I used to have problems with that too and I figured out the way. Sounds like a valid topic, to begin with.

## What’s up with the confusion

So, I’m going to tell you how I learned these two properties and will try to make sure that after reading this blog post you too will be able to get the right alignment on your very first try. Let’s start.

## The Concept of Axis

This is the core concept that we are going to wrap our head around. ***The Axis.***

There are generally two axis in particular:

**Main Axis** is the horizontal line around the View or say Screen (let’s take the Main Axis =&gt; Horizontal as default for now).

**Cross Axis** will be perpendicular to Main Axis and is a vertical line around the View or say Screen in our case (let’s take Cross Axis =&gt; Vertical as default for now).

Let’s see how it looks:-

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626262030823/_Yr67b9vR.jpeg)

We will visualize our view sliding on these two lines.

## The Default Case:

We will be using a combination of ***flexDirection***, ***alignItems***, and ***justifyContent*** to achieve the right layout.

Here’s what the React docs say:
> Adding flexDirection to a component’s style determines the Main Axis of its layout. Should the children be organized horizontally (row) or vertically (column)?
> The default flexDirection is ‘column’ i.e. when you don’t assign any property to a view.

For remembering purpose, we are going to consider ***flexDirection: ‘row’*** first (horizontally) and will see how ***justifyContent*** and ***alignItems*** behaves under this.
> Remember, these things are documented already but the goal here is to not get confused.

## 1. flexDirection: ‘row’:-

By giving this property, we are making sure that children are going to be assigned **HORIZONTALLY**.

That’s what I’m about to tell that How I cleared the confusion and figured it out.

Please try to remember the order in which the properties are listed and follow along.

### 1. Justify Content:-

This is the first thing we are going to learn. The power of ***justifyContent*** is:-

Let’s see how it looks…

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626262033762/KooKmd0AE.jpeg)

See how the ball appears to slide horizontally i.e., on *Main Axis*

Simple, right? Now let’s move to the next thing.

### 2. Align Items:-

You can remember it as the opposite of ***justifyContent*** . In ***justifyContent*** we were moving View along ***Main Axis.***

So, the power of ***alignItems*** will be:-
> Cross Axis is always perpendicular to the Main Axis. Try to remember the position of Main Axis only and the Cross will already come to its 90 degrees.

Let’s see how it looks…

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626262036630/qYHnOksTq.jpeg)

See how the ball appears to slide vertically i.e., on *Cross Axis*

Simple, right? Before moving onto *flexDirection: ‘column’* I want you to recall the order in which we learned things. Believe me, that matters.

* flexDirection: ‘row’ (horizontal).

* justifyContent (move View along Main Axis which is horizonal).

* alignItems (move View along Cross Axis).

## 2. flexDirection: ‘column’:-

Remember how we learned the order in the previous section? We are going to do the same thing here and if you got the previous section right then this will go smoothly too.
> Assume flexDirection: ‘column’ as vice-versa of flexDirection: ‘row’ property and apply everything you learned so far.
> Main Axis and Cross Axis change position upon changing the flex direction. Other properties (justifyContent and alignItems) will still work same.

**1. Justify Content:-**

Remember the power of **justifyContent**? It lets you move your view along***Main Axis.***

That thing still works but now the *Main Axis* is *Vertical* and sliding the View will look like this:-

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626262039497/Dr_nVqVVM.jpeg)

**1. Align Items:-**

Align Items was moving view along ***Cross Axis*** before so it should still behave same, right?

Yes, it’s the same as before but now ***Cross Axis*** is *Horizontal* and sliding the View will look like this:-

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626262042365/dn_aCDL2F.jpeg)

See how simple and clear it is.

Now, when we know the basics, let’s cover the other two flex Directions which are not commonly used.

## 1. flexDirection: ‘row-reverse’:-

Works almost similar to ***flexDirection: ‘row’*** and the axis is still the same. You just have to reverse the order of flex-start and flex-end.

Remember, the basic idea here is:-
> Reverse the order of start and end.

As the name suggests, it’s reverse of row.

Let’s see how it works.

**1. Justify Content:-**

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626262046456/1xC-zFtMt.jpeg)

**2. Align Items:-**

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626262049332/F5I-OGCIC.jpeg)

## 1. flexDirection: ‘column-reverse’:-

Nothing surprising here. Just remember the basic idea:
> Reverse the order of start and end.

**1. Justify Content:-**

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626262052416/cack0FmFu.jpeg)

**2. Align Items:-**

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626262055275/uDqF32PaL.jpeg)

## Conclusion:-

* Just remember the concept of *Main Axis* and *Cross Axis*.

* Under *flexDirection: ‘row’*, *Main Axis* is Horizontal and the *Cross Axis* is Vertical i.e., perpendicular to *Main Axis*.

* *Justify Content* slides the view on *Main Axis*, always.

* *Align Items* slides the view on *Cross Axis*, always.

* *flexDirection: ‘column’* reverses the direction of *Main Axis* and *Cross Axis*.

* *Justify Content* and *Align Items* still works same as before.

* The basic idea of *reverse* flex direction is that you have to reverse the order of Start and End and everything else works as usual.

Remembering things in order helped me clear the confusion and I was able to get the right alignment at the very first try. Although inserting USB correctly in the first try is still a problem for and maybe some things can’t really be helped.

Hope that was helpful.

Shad

*Originally published at [http://iamshadmirza.hashnode.d](https://gist.github.com/c62d0bfca369967d225ad1f04fa9e814)ev.*
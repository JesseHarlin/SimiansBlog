title: "How to Split a List in Javascript"
date: 2012-03-18 12:00:00
tags: Javascript, jQuery
---

Occasionally someone taps my should to ask a quick javascript question. I figure that if someone is asking, its probably worth writing a quick blog post about. This post is geared to someone that is more or less newer to frontend stuff, and is looking for a few tips to help them out.

The question was: I have a list of 12 items, in a ul. I'd like to split it in half, and make two seperate lists. How can I do this?

Actually, this seemingly straightforward question is a good chance to talk about a few things.

Foremost:  the code is pretty straightforward if you do this in jQuery. First, here is the jQuery way:


{% jsfiddle the_Simian/ySL4U %}

<!-- more -->

So far, so good. Notice the first statement is simply to target a desired ul. the next bit uses an interesting jQuery pseudoselector: gt. This of course stands for 'greater than' and takes an index. since our list is 12 items, and index of 5 is exactly halfway in, since its 0 indexed. Its counterpart is lt, which is 'less than'.

Now, at this point, I always suggest pulling something off the DOM, working 'in memory' and then doing a single DOM resinsertion. Get in the practice of thinking like this. Working directly on the DOM is slow-sauce, and you don't want to go there. So, we use .remove(). Notice the selection itself is cached in our variable $last6lis. I should add that I personally always preface a jQuery variable with $. This is a way of keeping your jQuery DOM pieces separated from plain old javascript 'data'.

Then we make a 'ul' tag in memory, pop our list items in that, and push it back onto the DOM, directly after our targetd item.

And all is right in the world.

Only... we can do just a tiny bit more.

first of all, while the lt and gt selectors are certainly great, the `.splice()` function is technically faster, and in my opinion more clear. Recall that jQuery selectors, are really lists of jQuery objects. Actually, if you can internalize that with jQuery its 'always' a list - that makes life easier.

So lets try a quick experiment: first lets ditch the :gt selector and replace it with `splice()`. That means we need indexes 6-11, so we'd write something like:

`$ul.find('li').splice(6,11)` instead of `$ul.find('li:gt(5)')`

That's cool and all, but if you drop that directly in the fiddle.. it totally doesnt work! Why?

Simple answer: remember earlier that jQuery selectors always return a list of jQuery objects! what does splice return?  Just a primitive array.  The fix: Just wrap the result of the splice in a new jQuery function, and you're back in action, rockin' the suburbs.

`$($ul.find('li').splice(6,11))`  .. like this.

Well, this is cool! now we can even change these numbers around, and select different segments of the initial list. Now, what if we wanted to refactor and make something that would always split our list in half, regardless of length? The answer is much more clear now, you could do something a bit like this:

{% jsfiddle the_Simian/GgXXL %}

Neat! now no matter what the length our li's grow to be, we can always bisect them! Anytime you can do a small amount of work to make your code not require as much updating, that is handy.  Of course taking this to the next level, would be to pull this code out and put it in a function, or a plugin and begin to make it truly reuseable, but for now, for a quick 'document ready' script - this is fine.




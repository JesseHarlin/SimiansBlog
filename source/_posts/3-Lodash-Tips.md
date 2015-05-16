title: "3 Lodash Tips"
date: 2015-05-15 20:19:53
tags:
---

###Three Things that will help you use lodash more effectively.

{% img /images/lodash/lodash.png "lodash.js" %}

If you don't already know what lodash it, you're missing out on possibly the best utility library available for javascript. Its useable in both the client and server, and can really inprove the speed and readability of your code. If you're already in on how great lodash is, here are three tips I want to share that are useful to me, working with the library every day.


<!-- more -->

## Using `chain()`, with named functions is, in fact, totally off the chain.

This one is simple. Just about everytning I do in lodash starts with the following code

```js
var kennyLogginsAlbums = [
  { year: 1977, name: 'Celebrate Me Home', sales: 'Platinum'},
  { year: 1978, name: 'Nightwatch', sales: 'Platinum'},
  { year: 1979, name: 'Keep the Fire', sales: 'Platinum'},
  { year: 1982, name: 'High Adventure', sales: 'Gold'},
  { year: 1985, name: 'Vox Humana', sales: 'Gold'}
]; //.. Danger Zone

_
  .chain(kennyLogginsAlbums)
  .value();
```

So far so good. Now for any transformations that need to occur on the data, I am affored a much cleaner, useable interface for handling the data.


## Favor functional verbs, like `map`, `filter` and `reduce


##Don't reach for each.

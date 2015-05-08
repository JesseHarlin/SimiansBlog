title: "Using Inquirer.js"
date: 2015-05-06 20:38:08
tags: [inquirer.js, javascript, commnad line, Slush.js, Yeoman]
---

#Inquirer.js

If you've used Yeoman, or Slush.js you probably noticed that they both make use of a really nice tool for getting user input from the command line.

What is Inquirer? This is what they say:

{% blockquote %}
We strive at providing easily embeddable and beautiful command line interface for Node.js
{% endblockquote %}

I'd say they achieve their goal quite nicely. Its probably  the simplest, but most powerful way I've seen thusfar to gather user input from the command line.

```js
inquirer(questions, callback);
```

That's really it! You ask questions..then do something! The callback takes an argument, and that arment is a lost of all the answers the user provided.

###Answers

Because this works the same way, no matter how you make questions, it makes sense to discuss the answers first. Consider this..

```js
inquirer(questions, function(answers){

console.log(answers); // you would see an array of answers!

});
```

What would each answer look like? Each is a simple object.

```js




```


###Questions

There's two primary ways you can make questions , declaratively or imperatively. You can provide a static list of json objects that represent questions, or you can build the questions functionally useing the underlying reactive interface.



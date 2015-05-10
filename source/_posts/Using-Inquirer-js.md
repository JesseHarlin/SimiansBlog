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

 console.log(answers); //an object containing the user response.

});
```

What would the answer look like? Each is a property on a simple object. The keys are entirely determined by the names assigned to the question, and the values are what the user input.

```js

{
  nameOfQuestion1: "userAnswer1"
  nameofQuestion2: "userAnswer2"
}


```

Now, how do we ask questions?


###Questions

There's two primary ways you can make questions , declaratively or imperatively. You can provide a static list of json objects that represent questions, or you can build the questions functionally useing the underlying reactive interface.

The absoultely most common and simple use will be just making the questions declaratively, as a simple JSON array. Depending on the type of question, you might need to add more properties, but you'll always want  at minimum the `name` of the question, and the `message` the user will see.

```js
var questions = [{
  name: 'firstName',
  message: 'What is your first name?'
}]

```

You can add more properties, though to do a bit more..

```js

var questions = [{
  name: 'firstName',
  message: 'What is your first name?'
}, {
  name: 'operatingSystem',
  message: 'What is your operating System?',
  default: 'Windows'
}, {
  name: 'favoriteColor',
  message: 'What is your favorite rainbow Color?',
  type: 'list'
  choices: ['Red', 'Orange', 'Yellow', 'Green', 'Blue', 'Indigo', 'Violet'],
  filter: function (str){
    return str.toLowerCase();
  }
}]

```

Here we see that the `questions` list has a few tricks up its sleeve. Firstly, you can always provide a default answer. That's handy. Secondly you can provide a filter function that will run the response through some sort of function, to get the answer in a format you'd prefer. For the favorite rainbow color question, "Green" will return as "green", and so on.

The next part is of special signifigance... the `type property`. This determines the type of prompt the user will see. There are

* *list* - A simple list, where the user can use the cursor to pick a selection
* *rawlist* - like list, but they type in the number for the answer
* *expand* - This takes a limited set of answers that map an answer to a character
* *checkbox* - like list, but you can have more than one answer
* *confirm* - Its a confirmation, as in Y/n style.
* *input* - Literally the default questions. Asks a question, gives you the user's exact response. Nothing fancy.
* *password* - like input but the answer looks all like `*******`.

Really, when it gets down to it, you either present a list for the user to pick from, or take a free response. All of these types achieve one of these goals, and if its a list, you'll need to give them coices on the `choices` property.

Of course you can also do some fancy stuff, like put a divider bar in the coices, too. It looks like this:

```js
{
  name: 'favoriteColor',
  message: 'What is your favorite rainbow Color?',
  type: 'list'
  choices: [
    'Red', 'Orange', 'Yellow',
    new inquirer.Separator(),   //separating the orangy things from the bluish things
    'Green', 'Blue', 'Indigo', 'Violet'
  ],
  filter: function (str){
    return str.toLowerCase();
  }

```

That's the easiest way to ask questions, certainly. You can do it also, "functionally" by using the underlaying ractive library. Heres a simple 1:1 example...

```js

//rx is in inquirer.. so you'll need that.
var Rx = require('inquirer/node_modules/rx');

var declarativeQuestions = [{
  name: 'firstName',
  message: 'What is your first name?'
}];

var rxQuestions = Rx.Observable.create(function( obs ) {
  obs.onNext({
    name: 'firstName',
    message: 'What is your first name?'
  });
})

```





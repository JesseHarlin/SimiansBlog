title: "Using Inquirer.js"
date: 2015-05-06 20:38:08
tags: [inquirer.js, javascript, commnad line, Slush.js, Yeoman]
---

#Inquirer.js

If you've used Yeoman, or Slush.js you probably noticed that they both make use of a really nice tool for getting user input from the command line.


{% img /images/inquirer/inquirer_small.png "inquirer.js" %}


What is Inquirer.js?

<!-- more -->

This is what they say:

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

One of the things you'll want to do is validate user input. That's easy, there's a `validate` property that's optional;

```js
var questions = [{
  name: 'firstName',
  message: 'What is your first name?',
  validate: function(str){
    return str !== 'Yoda'; //Yoda had *better not* use this application!!
  }
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

Here we see that the `questions` list has a few tricks up its sleeve. Firstly, you can always provide a default answer. That's handy.

Secondly you can provide a filter function that will run the response through some sort of function, to get the answer in a format you'd prefer. For the favorite rainbow color question, "Green" will return as "green", and so on. This is especially handy. This combined with `validate` means you can really get exactly the answer you want.

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

So what. Why would you ever, ever leave the compfort of the declarative way of making the questions? I can think of one reason offhand; a major reason, actually. Async situations. Say you wanted to make an ajax call, and then use the response to somehow compose a question? That's a bit harder to do the other way (though not impossible). Just an option if you want it. Honestly I never use it.

# Branching Out a Little

However, I do use one feature I haven't mentioned yet, `when`. This is how you can, very very easily introduce branching to your questions. Its just a simple function that when it returns truthy, the question gets included. Its default truthy of course so the question gets asked. Its really, if anythign a way to turn *off* questions you *don't* want.

```js
var questions = [{
  name: 'pizzaOrTaco',
  message: 'You want to eat pizza or taco?',
  type: 'list',
  choices: ['Pizza', 'Taco']
}, {
  name: 'pizzaIngredients',
  message: 'OK, so what goes on your pizza?',
  type: 'checkbox',
  choices: ['peperonni', 'extra cheese', 'sausage', 'mushroom', 'sugar cubes'],
  when: function(answers){
    return answers.pizzaOrTaco === 'Pizza';
  }
}, {
  name: 'tacoIngredients',
  message: 'Pick out the things that go in your taco.',
  type: 'checkbox',
  choices: ['chicken', 'rice', 'beef', 'pico', 'hawt sauce', 'zazzle'],
  when: function(answers){
    return answers.pizzaOrTaco === 'Taco';
  }
}]
```

Depending on whether or not you are eating a pizza or a taco, you'll pick differnt ingredients.

#Get a Handle On It

One of the most practical features of inquirer is the ability to hook into the lifecycle of the questions. This is handy if you want to persom actions in between quetions.

```js

inquirer
  .prompt(questions)
  .process
  .subscribe(onAnswer, onError, onComplete);

```

These functions fire exactly when you think they do. After each question, the question handler fires and takes the ever-increasing object that is the answers. The complete one works the same way. Actually, its really just the same as if you had supplied a callback to the main function.

I use this if I want to do something after each question, mainly. Otherwise, its not really necessary in my opinion.

#Lets Get Fancy

So what good is this post if we don't do something a little interesting? So far, everything I've mentioned is basically also covered, fairly well in the documentation. Lets say you want to ask the user an endless series of questions? How would you even do this? The answer is a simple recursive function!

A quick referesher - when you make a function that is recursive, you need to have a base case. This is how you will ultimately break out of a function. Without this, you go ferver and ever. Here's what I mean:

{% jsfiddle uojs9yt5 %}

Look at this very simple recursive function. The base case is how we will eventually break out. Be careful with recursion, its easy to accidentally go forever.

So in an inquirer prompt, what will *our* base case be? Easy! It is simply when the user no longer provides input.


```js

var question = [{
  name: 'tacoIngredient',
  message: 'Whats another tasty thing to add to your taco?',
  default : ''
}]

var tacoIngredients = [];

function finalAction(){

  console.log('Make a taco with: =>' + tacoIngredients)
}

function askOrPerformFinalAction(answer) {

  ingredientsList.push(answers.tacoIngredient);

  if (!answers.tacoIngredient) {
    finalAction(tacoIngredients);
    return;
  }

  return inquirer.prompt(question, askOrPerformFinalAction);
}

inquirer.prompt(question, askOrPerformFinalAction);

```

So this here is a situation where we will ask over and over, until the enduser hits enter on a blank line. Each anser is fed into an array, which could ostensibly be used by the programmer to 'do something' later on.

Combined with a little functional programming , the ability to branch via the `when` property and the various prompt types, you can use this technique to ask the enduser nearly anything, in any way! Since the answers are just simple objects, the possibilities are limitless.







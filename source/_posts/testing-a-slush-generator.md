title: "Testing a Slush Generator"
date: 2015-04-04 22:43:43
tags: slush, javascript, TDD, testing
---


One thing I'v enoticed while looking at the generators for both yeoman and sluch, is that overall, they tend to be undertested. I think this is for two reasons. Firstly, I think many folks consider this kind of dev tool to not be, generally speaking, differnt from production code. Its a tool to assist a dev work, and not necessarily subject to the same level of testing rigor. I think anothe reason is that testing file IO is fundamentally much more challengin then a typical unit test.

Thankfully, since we are testing gulp output, theres a handy module that does make things easier called [`mock-gulp-dest`.](https://github.com/slushjs/mock-gulp-dest) If you look at the source of `mock-gulp-dest`, you'll notice that it uses `through2` to essentially handle the mocking of the file IO. Of course you don't want to *actually* write files to the disk and assert their existance. It might be tempting to use one of the `fs` assertion tools and actually put the files somethere, but that's pretty messy in the long run, and not pragmatic for running tests in multiple environments.

Firstly, its handy to make a fixture to mock the inquirer output. 

```js
'use strict';
var inquirer = require('inquirer');

function mockPrompt(answers) {
  
  function assignAnswer(prompt) {
    if (!(prompt.name in answers)) {
      answers[prompt.name] = prompt.default;
    }
  }
  function inquirerPrompt(prompts, done) {
    [].concat(prompts).forEach(assignAnswer);
    done(answers);
  }
  inquirer.prompt = inquirerPrompt;
}

module.exports = mockPrompt;
```


This is the contents of an inquirer fixture that handles that well. Once you have that, just include it in your test files where you are going to mock the prompts. You'll also want to include the actual slushfile you're testing. In this case, imagine there is just one main slushfile to test:

```js
var mockPrompt = require('./inquirer-prompt-fixture');
require('../slushfile');
```


To my taste, I am partial to mocha for a test runner and chai's expect syntax for assertion. YOu can use whatever you want, but this is what I use.





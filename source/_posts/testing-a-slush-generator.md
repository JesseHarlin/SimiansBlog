title: "Testing a Slush Generator"
date: 2015-04-04 22:43:43
tags: slush, javascript, TDD, testing, scaffolding, generator
---

One thing I've noticed while looking at the generators for both yeoman and slush, is that overall, they tend to be undertested. I think this is for two reasons. Firstly, I think many folks consider this kind of dev tool to not be, generally speaking, differnt from production code. Its a tool to assist a dev work, and not necessarily subject to the same level of testing rigor. I think anothe reason is that testing file IO is fundamentally much more challengin then a typical unit test.

<!-- more -->

Thankfully, since we are testing gulp output, theres a handy module that does make things easier called [`mock-gulp-dest`.](https://github.com/slushjs/mock-gulp-dest) If you look at the source of `mock-gulp-dest`, you'll notice that it uses `through2` to essentially handle the mocking of the file IO. Of course you don't want to *actually* write files to the disk and assert their existance. It might be tempting to use one of the `fs` assertion tools and actually put the files somethere, but that's pretty messy in the long run, and not pragmatic for running tests in multiple environments. Rather, `mock-gulp-dest` actually stubbs the `dest` method with one that doesn't actually write, and then reverts it when complete. That said, this is ultimately a sophisticated function stub.

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


Here is an example fo a file performing two simple tests:

```js
'use strict';
var chai = require('chai'), //chai for assertion
  gulp = require('gulp'), 
  mockGulpDest = require('mock-gulp-dest')(gulp), //here is our dest stub
  expect = chai.expect; //I use expect, but I dont expect(you).to.also()

//this is to mock inquirer
var mockPrompt = require('./inquirer-prompt-fixture');

//and the thing we are testing
require('../slushfile');

describe('your awesome module!!!', function () {

  describe('taskname', function () {
    
    //before each of these mocks, I need to provide mock data for what might have been responses
    beforeEach(function () {
      mockPrompt({
        appName: 'test-app',
        userName: 'the-simian',
        authorName: 'Fancypants Harlin',
        authorEmail: 'derp@derp.derp',
        appDescription: 'some description',
        moveon: true
      });
    });
    
    it('should make a readme', function (done) {

      function assertDirectories() {
        mockGulpDest.assertDestContains('README.md');
        done();
      }
      
      //here's the tricky part....
      gulp
        .start('default')
        .once('task_stop', assertDirectories);
    });

    it('should make a package.json', function (done) {

      function assertDirectories() {
        mockGulpDest.assertDestContains('package.json');
        done();
      }

      gulp
        .start('default')
        .once('task_stop', assertDirectories);
    });

  });
});
```

Now for a description of the tricky part. The file writing is essentially an async process. You'll need to use a testing framework that can elegantly handle an async assertion with a callback. In this case, its idiomatic in `mocha` to use done in this manner. You can use `gulp.start('yourtaskname')` to kick off a stask, but the very important part is that you attach the assertion to the handler of `task_stop`. Be wary, you might see `on('finish')` , `on('stop')` or other variants in other folk's code. If you use `on`, you might get some errors in your runner, such as

```sh
Error: done() called multiple times
```

Or if you attach to a differnt event,  your runner will fail because it times out, because the asserting function never gets called at all (and hence the `done` function never is called).

To test, I cam calling my testing method from gulp. Its pretty straightforward in the gulpfile.


```js

var gulp = require('gulp');
var istanbul = require('gulp-istanbul');
var mocha = require('gulp-mocha');

function test(cb) {

  var mochaOpts = {
    reporter: 'nyan' //cool-mode
  };

  function runner() {
    gulp
      .src(['./test/*.js'])
      .pipe(mocha(mochaOpts))
      .pipe(istanbul.writeReports())
      .on('end', cb);
  }

  gulp
    .src(['./slushfile.js'])
    .pipe(istanbul())
    .pipe(istanbul.hookRequire())
    .on('finish', runner);
}

gulp.task('test', test)
```

So to actually run the test, I call

```js
gulp test

```


I always use the nyan cat reporter, because it delights me. 

{% img /images/slush-howto/slide-cat.gif "How I feel testing code" %}

No judging here. 
Watch that kitty surf that poptart!!!


```sh
[22:37:41] Starting 'test'...
[22:37:41] Starting 'default'...
[22:37:41] Finished 'default' after 16 ms
readme
[22:37:41] Starting 'default'...
[22:37:41] Finished 'default' after 5.07 ms
package~|_( ^ .^)
 2   -_-_,------,
 0   -_-_|   /\_/\
 0   -_-^|__( ^ .^)
     -_-  ""  ""

  2 passing (36ms)

-------------------------|-----------|-----------|-----------|-----------|
File                     |   % Stmts |% Branches |   % Funcs |   % Lines |
-------------------------|-----------|-----------|-----------|-----------|
   your-module-name\     |     80.65 |     40.74 |        80 |     80.65 |
      slushfile.js       |     80.65 |     40.74 |        80 |     80.65 |
-------------------------|-----------|-----------|-----------|-----------|
All files                |     80.65 |     40.74 |        80 |     80.65 |
-------------------------|-----------|-----------|-----------|-----------|


=============================== Coverage summary ===============================
Statements   : 80.65% ( 25/31 )
Branches     : 40.74% ( 11/27 )
Functions    : 80% ( 4/5 )
Lines        : 80.65% ( 25/31 )
================================================================================
[22:37:42] Finished 'test' after 734 ms
```

Oh nice! We got two passing tests! Could use a few more though :)



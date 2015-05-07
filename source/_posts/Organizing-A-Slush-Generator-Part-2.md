title: "Organizing A Slush Generator Part 2 : Testing And creating Default Answers"
date: 2015-04-24 00:10:42
tags: [Slush, Mocking, Reqire, Testing]
---


Part of the reason for having an improved project organization is to define better units of work for testing. In the last section, we arranged our files in such a way that we wanted to make it easier to have each file do a singluar task each.

Because We have 4 totally separate files with separate jobs-each test file will also look pretty different.

<!-- more -->



## Default answers

Probably the most challenging file to test is the default answers file. This is a necessary file, but it involves using a lot of node globals and private variables. This seems to make it untestable at a glance, but its actually possible with a module called `rewire`. Rewire is a drop in replacement for require, that for all practical purposes acts fully identical - except you have a way to override private variables in the module. Normally, this is a no-no, but not in this case. We need to mock process, and this is how it can be done.


```js
var rewire = require('rewire');

var defaultTransforms = rewire('./../../slush/default/answers');
```

So we use `rewire`, not `require`.


One of the things you might need to do for your default answers is get information about things like, the name of the file you're starting the project in, or information out of the user's gitconfig. We need to be able to branch, depending on whether or not the user is using windows or not, this affects these sort of things.

Here is mocking both the win32 defaults and nonwin32 defaults, with chai's expect for assertion.

```js
  describe('default-answers', function () {

    function mockCwd() {
      return '/example/of/something';
    }

    var mockEnv = {
      USERNAME: 'Jesse_Harlin',
      HOME: '/Users/YOURMOM',
      USERPROFILE: 'C:\Users\Jesse_Harlin'
    };


    describe('process is win 32', function () {
      defaultTransforms.__set__({
        process: {
          env: mockEnv,
          cwd: mockCwd,
          platform: 'win32'
        }
      });

      var defaults = defaultTransforms(),
        out = {
          appName: 'something',
          userName: 'Jesse_Harlin',
          authorName: '',
          authorEmail: ''
        };

      expect(defaults)
        .to
        .eql(out);
    });

    describe('process is other than win 32', function () {
      defaultTransforms.__set__({
        process: {
          env: mockEnv,
          cwd: mockCwd,
          platform: 'notWin32'
        }
      });

      var defaults = defaultTransforms(),
        out = {
          appName: 'something',
          userName: 'YOURMOM',
          authorName: '',
          authorEmail: ''
        };

      expect(defaults)
        .to
        .eql(out);
    });

  });
  ```

We are essentially testing the first major branch of the default answers. In order to do this, we need to basically give differnt data for the default answers, by mocking it. We need to test the platform, in this case by mocking the `process` module. Now we can branch for when its windows...or not.

In our answers, we can now write code about this:

```js
  if (process.platform === 'win32') {
    homeDir = process.env.USERPROFILE;
    osUserName = process.env.USERNAME || path.basename(homeDir).toLowerCase();
  } else {
    homeDir = process.env.HOME || process.env.HOMEPATH;
    osUserName = homeDir && homeDir.split('/').pop() || 'root';
  }
  ```
  Now, whenever we want to get information based on environment, we have a way to check and mock them.
  
  Another thing we will problably want to handle is getting information from the user's github `.gitconfig` file. This is also tricky. A first thought would be to use `fs` and some `fs` mocking utilities to simulate reading the file. One way that works well is to make a fixure in the test directory that simulates a User directory, and the `.gitconfig`'s relative location. Fixures are a great way to get things done without adding way too much complexity.
  
  If you haven't ever opened one up, a `.gitconfig` is a ini file, that looks sort of like this:
  
```sh
[user]
  name = fart Blaster
  email = turdburgular@gmail.com
```
When we actually make the module, we'll use `iniparser`, a simple module to read this and make sense of it... if it actually exists. That's the thing we need to test. The other branch in our, soon-to-be-default answers file is whether or not there is even a `.gitconfig`.


You can make a fixture in your test firectory like this

```sh
+-- fixtures
   +-- Jesse_Harlin
      +-- .gitconfig
   +-- Not_Jesse_Harlin
      +-- .gitkeep
```

I used my name, because.. no reason. Its just mock data. You can put the mock data in the Ini file. Now, in our test, there will be a `.gitconfig` file that gets loaded, assuming we mock the same userName in our fixture.
  
Lets see what then, the expecations for this might be. If there is a gitconfig in our User Directory, get the username and email. Otherwise..dont!

```js
    describe('.gitconfig is present in the user directory', function () {
      function mockCwd() {
        return '/example/of/something';
      }

      var mockEnv = {
        USERNAME: 'Jesse_Harlin',
        HOME: 'test/default/fixtures/Jesse_Harlin',
        USERPROFILE: 'test/default/fixtures/Jesse_Harlin'
      };

      defaultTransforms.__set__({
        process: {
          env: mockEnv,
          cwd: mockCwd,
          platform: 'win32'
        }
      });

      var defaults = defaultTransforms(),
        out = {
          appName: 'something',
          userName: 'Jesse_Harlin',
          authorName: 'fart Blaster',
          authorEmail: 'turdburgular@gmail.com'
        };

      expect(defaults)
        .to
        .eql(out);
    });
    
    describe('.gitconfig is not present in the user directory', function () {
            function mockCwd() {
        return '/example/of/something';
      }

      var mockEnv = {
        USERNAME: 'Jesse_Harlin',
        HOME: 'test/default/fixtures/Not_Jesse_Harlin',
        USERPROFILE: 'test/default/fixtures/Not_Jesse_Harlin'
      };

      defaultTransforms.__set__({
        process: {
          env: mockEnv,
          cwd: mockCwd,
          platform: 'win32'
        }
      });

      var defaults = defaultTransforms(),
        out = {
          appName: 'something',
          userName: 'Jesse_Harlin',
          authorName: '',
          authorEmail: ''
        };

      expect(defaults)
        .to
        .eql(out);

    });
    
  ```
  
  So if there's a .`gitconfig`, I expect it to be parsed and there will be some nice, shiny defaults - and if not? there wont. Now we can implement the code that will make these pass.. the act of actually parsing the file.
    
```js
  iniparser = require('iniparser');

  user = fs.existsSync(configFile) ?
    iniparser.parseSync(configFile).user :
    {};

//and in the return object...


  return {
    appName: workingDirName,
    userName: osUserName || formatName(user.name || ''),
    authorName: user.name || '', //<=== these are covereed now
    authorEmail: user.email || '' //<===
  };
```

When you're all done your answers file will be something like this:
    
```js
'use strict';


var path = require('path'),
  iniparser = require('iniparser'),
  fs = require('fs');

function makeDefaults() {

  var workingDirName = path.basename(process.cwd()),
    homeDir,
    osUserName,
    configFile,
    user = {};

  if (process.platform === 'win32') {
    homeDir = process.env.USERPROFILE || '';
    osUserName = process.env.USERNAME || path.basename(homeDir);
  } else {
    homeDir = process.env.HOME || process.env.HOMEPATH || '';
    osUserName = homeDir && homeDir.split('/').pop() || 'root';
  }

  configFile = path.join(path.resolve(homeDir), '.gitconfig');

  if (fs.existsSync(configFile)) {
    user = iniparser.parseSync(configFile).user || {};
  }

  return {
    appName: workingDirName,
    userName: osUserName || '',
    authorName: user.name || '',
    authorEmail: user.email || ''
  };
}

module.exports = makeDefaults;
```
  
If we find, for our default tasks, as we need more properties, we can add them, and make sure the right default answers object is being asserted against in our tests.





title: "Organizing A Slush Generator Part 2 : Testing"
date: 2015-04-24 00:10:42
tags: Slush, Mocking, Reqire, Testing
---


Part of the reason of an improced organization is a better units of work for testing. In the last section, we arranged our files in such a way that we wanted to make it easier to have each file do a singluar task each.

Because We have 4 totally separate files with separate jobs-each test file will also look pretty different.

<!-- more -->



## Default answers

Probably the most challenging file to test is the default answers file. This is a necessary file, but it involves using a lot of node globals and private variables. This seems to make it untestable at a glance, but its actualyl doable with a module called `rewire`. REqire is a drop in replacement for require, that for all practical purposes acts fully identical - except you have a way to override private variables in the module. Normally, this is a no-no, but not in this case. We need to mock process, and this is how it can be done.


```js
var rewire = require('rewire');

var defaultTransforms = rewire('./../../slush/default/answers');
```

So we use `rewire`, not `require`.


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

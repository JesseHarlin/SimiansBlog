title: "organizing a slush generator"
date: 2015-04-05 21:56:34
tags: [slush, scaffolding, project organization]
---


If you're making a scaffold, or even a gulpfile of any degree of size or signifigance, you'll quickly learn that placing every task you make in a single file can lead to a big mess after a while. One of the most attractive things about slush, is that it uses gulp, and one of the most attractive things about gulp is that it is extremely organizeable, due to its modular task building system.

When you first get started, or run the `slush generator-generator`, the first thing you will notice is that a lot of folks put everything in the main slush file. I don't really recommend this. Its easier, at least to first make a separate `folder` called "slush".

What we want is to separate the tasks by folder, and then per task, separate each one of these by function. If there is a complex task, consisting of smaller parts, we cna break those down by file in the folder. When you're doing tests, its much easier to have your test directory match that of your slush directory. Assuming each file is single responsibility, and each task has a folder, you will then also have one test file and folder to match the source.

Lets factor out a default task. Consider some code, that is like what is spit out by the default `generator-generator`

```js
/*
 * slush-test
 * https://github.com/the-simian/slush-test
 *
 * Copyright (c) 2015, Jesse Harlin
 * Licensed under the MIT license.
 */

'use strict';

var gulp = require('gulp'),
    install = require('gulp-install'),
    template = require('gulp-template'),
    rename = require('gulp-rename'),
    _ = require('underscore.string'),
    inquirer = require('inquirer');

function format(string) {
    var username = string.toLowerCase();
    return username.replace(/\s/g, '');
}

var defaults = (function () {
    var workingDirName = path.basename(process.cwd()),
      homeDir, osUserName, configFile, user;

    if (process.platform === 'win32') {
        homeDir = process.env.USERPROFILE;
        osUserName = process.env.USERNAME || path.basename(homeDir).toLowerCase();
    }
    else {
        homeDir = process.env.HOME || process.env.HOMEPATH;
        osUserName = homeDir && homeDir.split('/').pop() || 'root';
    }

    configFile = path.join(homeDir, '.gitconfig');
    user = {};

    if (require('fs').existsSync(configFile)) {
        user = require('iniparser').parseSync(configFile).user;
    }

    return {
        appName: workingDirName,
        userName: osUserName || format(user.name || ''),
        authorName: user.name || '',
        authorEmail: user.email || ''
    };
})();

gulp.task('default', function (done) {
    var prompts = [{
        name: 'appName',
        message: 'What is the name of your project?',
        default: defaults.appName
    }, {
        name: 'appDescription',
        message: 'What is the description?'
    }, {
        name: 'appVersion',
        message: 'What is the version of your project?',
        default: '0.1.0'
    }, {
        name: 'authorName',
        message: 'What is the author name?',
        default: defaults.authorName
    }, {
        name: 'authorEmail',
        message: 'What is the author email?',
        default: defaults.authorEmail
    }, {
        name: 'userName',
        message: 'What is the github username?',
        default: defaults.userName
    }, {
        type: 'confirm',
        name: 'moveon',
        message: 'Continue?'
    }];
    //Ask
    inquirer.prompt(prompts,
        function (answers) {
            if (!answers.moveon) {
                return done();
            }
            answers.appNameSlug = _.slugify(answers.appName);
            gulp.src(__dirname + '/templates/**')
                .pipe(template(answers))
                .pipe(rename(function (file) {
                    if (file.basename[0] === '_') {
                        file.basename = '.' + file.basename.slice(1);
                    }
                }))
                .pipe(gulp.dest('./'))
                .pipe(install())
                .on('end', function () {
                    done();
                });
        });
});
```
I should add that the default generator is not concerned with testing at all, but we are. So what is happening, big picture in this file?

- we are requiring "stuff"
- we need some default answers
- we ask some questions
- we do a task.

These are, in my opinion good ways to break apart the tasks. Have one file for default answers, one for questions, and one for tasks. Let each file require only what it needs. Instead of putting everyting in one big file, lets break it up.

We add the `slush folder` in the root and give a folderr to the default task. We will do this for every task henceforth!

{% img /images/slush-howto/organized-files.jpg %}

A single slush file becomes something like this.

How, we can divide the tasks up.


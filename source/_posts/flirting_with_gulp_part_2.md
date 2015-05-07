title: "Flirting with Gulp: Part 2"
date: 2014-04-1 12:00:00
tags: [Gulp, Task Runners, Javascript]
---

At this point I've played with gulp enough to make the decision that I'd like to consider moving from grunt to gulp. I respect there is a lot of trepidation about this in the community. I can hear their objection. I sounds like an old saying we all heard around here as youngsters:

{% blockquote %}
If it ain't broke, don't fix it.
{% endblockquote %}

At the same time, broke and "ain't broke" is a fairly binary way to see a system. Where does functional, yet unoptimized lie? Where does "working, but unscaleable" sit? Any software engineer knows there are often multiple approaches to solving a problem, and there is often merit to improving a system, for the sake of organization, optimization, maintenance and stability. I think switching to gulp is worth the work investment.

You can bet I have a massive Gruntfile to rework. Every journey begins with the smallest step.

<!-- more -->

First we do the node dance. Install Gulp!

```sh
npm install -g gulp
```

And then:

```sh
npm install --save-dev gulp
```
Now I add a gulpfile.js to my project, and

```js
var gulp = require('gulp');
```

so far, so simple.

now I actually need STUFF. Gulp plugins. Thankfully here is many. I just follwoed the same procedure for each

```sh
npm install --save-dev gulp-changed
npm install --save-dev gulp-concat
npm install --save-dev gulp-copy
npm install --save-dev gulp-stylus
npm install --save-dev gulp-clean
npm install --save-dev gulp-htmlmin
npm install --save-dev gulp-cssmin
npm install --save-dev gulp-uglify
```

If you used it in Grunt, most likely it is also in gulp. Notice I didnt add watch? That's already included in gulp. Awesome!

Now one little thing you can do is explicitly include each plugin. That is well and good. Actually, if you start breaking your files up for organizational reasons (protip: we will) that's the way to go. If you have a shorter gulpfile.js, or just want to include everythign easily, heres a little trick

```sh
npm install --save-dev matchdep
```

```js
function loadModule(module) {
    global[module.replace(/^gulp-/, '')] = require(module);
}
require('matchdep')
    .filterDev('gulp-*')
    .forEach(loadModule);
```
    
Neat, huh? Autoloads every gulp plugin, since they are conventionally named. Globbing is neato!

Again, this is more practical, if you're going to stick to one file.

Now here is what is interesting. I'm immediately seeing pros to Gulp. I'll set up a simple script that goes into a series of theme folders, and makes basically a series of theme stylesheets.

In grunt I have a function declared somewhere in my options object that I use throughout the file. I had to do it like this because Grunt is massively configuration-heavy.;

Its someting like this:

```js
options.themeList = function themeList(themes) {
            var obj = {};
            for (var i = 0, ii = themes.length; i < ii; i++) {
                var root = 'src/public/styles/themes/' + themes[i];
                obj[root + '/theme.css'] = root + '/theme.styl';
            }
            return obj;
        }
```

and somewhere I have a list of themes corresponding to folders, in my case I have 9 total. Lets pretend these are also attached to my session grunt options object. Basically:

```js
options.themes = [
        'ThemeOne',
        'ThemeTwo',
        'ThemeThree',
        'ThemeFour',
        'ThemeFive',
        'ThemeSix',
        'ThemeSeven',
        'ThemeEight',
        'ThemeNine'
    ]
```

And then, this is used in the stylus plugin for Grunt, in the Gruntfile.js

```js
//...
stylus: {
         themes: {
             options: {use: [require('nib')]} //some stylus options
             files: options.themeList (options.themes)
         }
 
//...
```
There are probably simpler, and more elegant ways to do this. Actually, in my actual code there are some minor varitions from this, taht are related to implementation, but this should get the point across, easily. Grunt's initConfig Method is a big, honkin pile of JSON. Actually, in practice mine had gotton so large that I was looking at strategies to organize it when I stumbled upon Gulp.

I wrote a similar setup in Gulp. I was able to encapsulate the method much more nicely, because Gulp is code-first! Its something like this:

```js
gulp.task('stylus:themes', function stylusThemes() {
    var themes = options.themes;
 
    function makeThemes(themes) {
        for (var i = 0, ii = themes.length; i < ii; i++) {
            var theme = themes[i],
                themeFolder = 'src/public/styles/themes/' + theme,
                stylusConfig = {
                    use: ['nib']
                };
 
            gulp
                .src(themeFolder + '/theme.styl')
                .pipe(stylus(stylusConfig))
                .pipe(gulp.dest(themeFolder));
        }
    }
 
    makeThemes(themes);
});
```

As a side note, I can't help but reflect upon some of the same organizational issues I encountered whilst working in say, Ext.js. There you have another, really big, configur-ish way to implement a module pattern. Its nice, but when you need to go outside the box, it can be challenging at a glance to figure out where stuff should live. I think theres a very reasonably attraction to tools like node and browserfy, and other very code-first approaches to code organization. I mentioned this in the last post, but in C# land, using more convention over configuration has proven time and time again to be a better way of doing things. I pity the dev using XML instead of FLuent nHibernate to configure nHibernate. It really is much better.

So, there you have it. a Very very slightly not run-of-the mill task cross written on both platforms. To my eyes, the Gulp version is way more maintainable in the long run. However, the story doesn't end there. I ran the two side by side, just out of curiosity, to see which one is faster. Now, recall, these are pretty big tasks here. These aren't fired at request time from a client, but are preprocessed to make static resources. The only thing that truly benefits is like a file watch while working in the .stylus files. But, omigosh Gulp is faster.

Here is the Grunt task ( I am using time-grunt to measure the speed):

{% img /images/gulp/grunt-stylus-speed.png %}

Theme names have been hidden to protect the innocent. Just pretend they are named after American Presidents. eg. FranklinPierce\theme.css

Now, here is the same thing in Gulp.

{% img /images/gulp/gulp-stylus.png %}

I had to run it twice to be sure. Did it actually make the files? I looked. Yes, they're there. That's literally over 1000 times faster. 1064.891 times faster, to be more accurate.

Wow. I expect huge gains during tasks that run under file watch during development.

title: "Lets Make a Basic Slush Generator"
date: 2015-03-30 23:09:52
tags: [slush, javascript, gulp, generator, scaffolding]
---

![Slush.js](http://slushjs.github.io/assets/slush.png)

I've been using gulp for a while now, and it has become my go-to build system. I have also spent a lot of time trying my very best to getused to using generators regularly in my workflow. I gave talk some time ago about Yeoman, which was a seemingly obvious first choice for a generator.

As I've used gulp more and more, I've appreciated its direct simplicity. Its a quick, streaming build system, and I find I am using it more and mor eon a regular basis to solve problems. Yeoman, while an excellent tool is something I've felt like when I want to really customize something to my liking, can be a bit of work. I was really excited when I came across Slush.js

<!-- more -->

Its seriously hard to believe that was actually a year ago!

![My Yeoman Talk](http://okcjs.com/images/posters/2014-feb-yeoman.jpg)





## About Slush 

Slush can best be summarized by the info on the website

{% blockquote https://github.com/slushjs/slush %}
Slush is a tool to be able to use Gulp for project scaffolding.
... a tool to help you generate new project structures to get you up and running with your new project in a matter of seconds.
Slush does not contain anything "out of the box", except the ability to locate installed slush generators and to run them with [liftoff](https://www.npmjs.com/package/liftoff).
{% endblockquote %}

OK. perfect! All the scaffolding goodness I want, but with gulp as the core mechanism to get stuff done. 

I'm ready to make a scaffold. The website spells it out pretty straightforward.

You need `gulp` and `slush`
```sh
npm install -g slush
npm install -g gulp
npm install -g slush-generator
```

If you've used Yeoman, you know there's a `generator-generator`. That helps you generate , well ..generators! This is like the `npm init` of slush. Its how we are going to kick off our new slush project.

```sh
slush generator
```

You will be asked things, not unlike when you run `npm init`. Fill out the project name, the decription, your name and stuff like that. Consider letting it replace your readme and your package.json. The scaffolded readme is handy and the scaffolded package.json will have just about everything you need. Also just be aware that the scaffold will handle the `slush` prefix for you. If your project is `slush-radgenerator`, just call it `radgenerator`.

The fully scaffolded project is really nice. It includes your gitignore, liscence file, travis file and a number of other things for you.

Here's where we start really making this thing work...

Open the file called `slushfile.js`. You'll notice this essentially bears a very strong resembelance to a `gulpfile.js`. That's because this basically *is* a gulpfile, that is using inquirer to ask you questions. Its that simple. Before we do any analysis and start plucking this project apart, lets do a quick test.

```js

```




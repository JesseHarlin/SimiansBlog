title: "Lets Make a Basic Slush Generator: Part I"
date: 2015-03-30 23:09:52
tags: [slush, javascript, gulp, generator, scaffolding]
---

{% img /images/slush-howto/slushlogo.png %}

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


First go to the generated project directory and link it.

```sh
npm link

```

Then go make some new folder and run your generator

```sh
slush your-generator-name-here
```

You will basically run the absolute default generator stuff. It should scaffold a small project.

Here is me making a non-project taht could be about basketballs

```sh
? What is the name of your project? ballz
? What is the description? Lets all do things with our balls!
? What is the version of your project? 6.6.6
? What is the author name? (Jesse Harlin)
```

Actually with inquirer it looks very nice, like this:
{% img /images/slush-howto/ballz.png %}

Of course not much is gonna happen yet.


If you look at the code, things are really pretty straighforward. Its refreshing.

1. Ask questions with Inquirer.js.
2. Take the answers and pass them to a callback.
3. Do stuff. Template Stuff.

Check it out, logging the json from the answers you can see exactly what inquirer is really doing, a simple question with a callback.

```js
//every inquirer statement is made like this.
inquirer.prompt( questions, callback )
```

And this is all driven by this simple bit of JSON
```js
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
```
Which makes these qppear in the console:
```sh
? What is the name of your project? ballz
? What is the description? Some description
? What is the version of your project? 6.6.6
? What is the author name? Jesse Harlin
? What is the author email? harlinjesse@gmail.com
? What is the github username? Jesse_Harlin
? Continue? Yes
```
Yielding this JSON..
```js
{
  appName: 'ballz',
  appDescription: 'Some description',
  appVersion: '6.6.6',
  authorName: 'Jesse Harlin',
  authorEmail: 'harlinjesse@gmail.com',
  userName: 'Jesse_Harlin',
  moveon: true
}
```

Nice.
And you can just make templates with that.

In case you're wondering, the `defaults` object you see above is just an object created elsewhere in the code that provides some sensible default values for the string of questions. You can put whatever you what there.

I want to point out that [inquirer](https://github.com/SBoudrias/Inquirer.js/) has some very very handy features. Its the same tool that Yeoman uses, if you're familiar. You can do a lot of cool things like, have folks [select from lists](https://github.com/SBoudrias/Inquirer.js/#list----type-list-), [check items in a list](https://github.com/SBoudrias/Inquirer.js/#checkbox----type-checkbox-) they want and stuff like that. It all looks really nice. All you really must do it change the `type` of question being asked, and then provide some options, depending on the type, like provide choices for a list of things to select.

The question obejct is [fully documented here](https://github.com/SBoudrias/Inquirer.js/#question).

In the boilerplate, you can see what happens after the questions..

```js
if (!answers.moveon) {
  return done();
}

answers.appNameSlug = _.slugify(answers.appName);

```
    
The function `done()` is just the callback. That means the first few lines, before the gulp statement are just to handle situations where no answers were provided or to clean up the project name fromt he first question. Big picture is that, you'd handle all data manipulation before doing templating here.

```js
gulp
  .src(__dirname + '/templates/**')
  .pipe(template(answers))
  .pipe(rename(function (file) {
    if (file.basename[0] === '_') {
      file.basename = '.' + file.basename.slice(1);
    }
  }))
  .pipe(conflict('./'))
  .pipe(gulp.dest('./'))
  .pipe(install())
  .on('end', function () {
    done();
  });
```
Here is the meat and potatoes. We go to templates, grab *everything*, template it all, and move it to the destination folder - which is what you are scaffolding. Simple. Of course nothing is happening simply because theres nothing to template. The template folder starts empty.

Try this, throw a quick file in there. I made a markdown file called `example.md`. with the contents:

```md
#<%= appName %>

##About
<%= appDescription %>

###Author
Name: <%= authorName %>
Email: <%= authorEmail %>

```

Run the generator again.

```sh
? What is the description? Bouncy, happy, glorious, scrumtrelescant balls! Basket Balls! Bowling Balls! Redball!
? What is the version of your project? 0.1.0
? What is the author name? Jesse Harlin
? What is the author email? harlinjesse@gmail.com
? What is the github username? the-simian
? Continue? Yes
```
These anspwers make a file with these contents

```md
#ballz

##About
Bouncy, happy, glorious, scrumtrelescant balls! Basket Balls! Bowling Balls! Redball!

###Author
Name: Jesse Harlin
Email: harlinjesse@gmail.com
```

How simple is that? In tis case we are using `gulp-template` as the module to handle templating. Its a very common syntax, sort o flike what `lodash` uses. I suppose there is no reason a developer couldn't use really anything she wanted to template with.

At this point, really you just template your project, like you would any project! Its probably good to make templates for a `readme.md`, a `package.json`, a `.gitignore`, and so on!

Perhaps in another post, I will tlak more about some strategies for slushfile project organization. If tis project grows, especially if we add subgenerators - I would venture that a single slushfile would be a bad idea.

Have fun getting slushy!



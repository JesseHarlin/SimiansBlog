title: "Orchard CMS: Getting Setup, Part 2, Source Setup and Workflow"
date: 2013-4-5 1:00:00
tags: Orchard CMS, Orchard CMS Tutorial
---

Now that you understand how you can start straight away from an already built project (what Webmatrix provides), I think its best at this point to work henceforth directly from the source. There are a number of reasons for this.

<!-- more -->

First of all, there’s not layer of abstraction for you. Starting from the build is great, but there’s a lot of advantages to being able to start form the source, and build it yourself. This gets you in the game. Sometimes , for example there is an older version of a module in the gallery, and you want the latest one, which is available on the modules site. Maybe the way to make a module work, is just a simple fix, and would be easy if you could see the source. In fact, on the subject of modules, the source has many “must have” modules already included in the source drop, such as Tag Cloud, Lucene, Search and others. The built version you can download has been slimmed down for size. This isn’t an issue for use, since we will build ourselves. Since theres a lot already included, this helps you configure very quickly. Maybe you want to make your own module. Seriously, working from the source is the way to go. Plus, most importantly, its much more comfortable to work in Visual Studio. You know your personal arsenal of plugins is working fine, when everything builds and runs, and when you can run all your tests.

There’s a number of ways to acquire the source. If you go to the Orchard project site, they are going to basically walk you through setting up Mercurial, and pulling the source down that way. You don’t necessarily have to do this. Clearly, I recommend using some manner of source control- and if you want to hang with the Orchard team you need to use Hg. That’s well and fine but I’m really into Github, so that’s what I use. If this is what you want to do, just go download the source from codeplex and then start your own repo. From here on out, this will be the main tool that powers all your orchard sites. When you build you can just include and exclude from this. If a site gets messed up, you can make a quick build and override the files. This source won’t overwrite your Media and Site folders; in fact- I put those folders in my .gitignore file. Whatever works for you in terms of deployment and source control is what you should do.

I will add that I keep my personal clone of the orchard source on my local dev machine. Orchard is actually pretty big – well the source is. I suppose that’s a side effect of having so many projects and so much modularity.

Go ahead and explore the Source drop. It should look like the image below (more or less) There’s some things to note here.

{% img /images/orchard/orchard7.png %}

First off there’s a ton of .hg files like hg tags and so on. This is all just stuff Mercurial uses. (Hg is the periodic table abbreviation for Mercury – get it? har har.) Neat.

There is a lib folder. A bunch of third party libraries live there. Orchard uses them. What libraries? Autofac, Clay, Castle Windsor, Nuget, Lucene, Log4Net, jQuery, nHibernate and stuff like that. Basically a ton of flippin’ awesome open source tools you should probably learn about anyways.  Honestly seeing this lib folder was a huge step in helping me decide to adopt Orchard as a core tool in my toolbelt. The Orchard team- they have taste!

Then, of course there are a few .cmd files. These are scripts we utilize to build. Can you guess what the file ClickToBuild does? You probably guessed correctly. It takes this Orchard source, and generates a built project we can use to deploy. It also runs your tests for you. I should mention that you can *technically* put the source on your server, and it will work. Its also a massive waste of disk space. Don’t do it.

Then finally there is the orchard src itself. Take a good look at how this entire source is laid out. This is par for the course how a good project should be laid out. The source has its own folder, and the parent folder has a lot of ‘meta’ stuff related to the source.

Now look in the source itself, and then open Orchard.Web.

{% img /images/orchard/orchard8.png %}

Look familiar? it’s the same folder structure we saw from the Webmatrix project we got from the Web Platform installer. I’m pointing this out, because this is basically going to be the part that gets deployed, after the build, with some exception. I’ll explain Look in the Modules in this directory.

 
{% img /images/orchard/orchard9.png %}

Each one of these modules is an entirely separate self contained project. Each has its own code, dependencies, models, controllers – everything. During the build, each module’s dependencies are reduced down, otherwise the final build would be really really big.

Now for the fun part, go back to the src folder and find the .sln file. That’s what Visual Studio Uses. First, hit the collapse all button. You’ll thank me later. Orchard is made up of a ton of smaller projects, one for each of those modules we saw, and of course the core and orchard.web itself. check out the solution explorer.

{% img /images/orchard/orchard10.png %}

There’s only a few projects in the root. the most important ones are Core and Web. Remember when we explored Orchard.Web earlier in the actual windows File explorer? Modules was inside of Orchard.Web. Here it appears outside, and at the top. That is, in fact the exact same folder. Now if you open in VS2010, it becomes more obvious. The folder looks like its opacity has been lowered, as if it’s a ghost folder. Nonetheless, that’s just an alias to the folder in Orchard.Web. Its listed atop for convenience, I assume.  Same case for Specs,Tests, Themes and Tools. Themes, for example is like Modules. The folder actually resides in the startup project, Orchard.Web.

I’m just pointing this out, because at first it confused me. Then I realized how handy that actually was.

So to rehash, these folders are all really located elsewhere:

Modules – Actually is located in Orchard.Web and is literally every extra module you’ve downloaded or added to Orchard, as well as the ones that were already there. This thing is big.
Specs + Tests  - These are for unit testing, integrations tests and that sort of thing..
Themes – All you themes go here. This is the last stop for overriding the appearance of Orchard. The folder really lives in Orchard.Web
Tools – This has testing helpers, and the orchard command line. More on this later.
I think more project exploration is in order, and also covering the sparkle magic that is the orchard command line environment. But that is for another day!

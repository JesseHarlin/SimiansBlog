title: "Orchard CMS: Getting Setup, Part 1, the Non-Source Setup"
date: 2013-4-5 1:00:00
tags: Orchard CMS, Orchard CMS Tutorial
---

Orchard is generally easy to setup. I think there is a larger question about workflow and deployment one has when they’re asking about setup. Really there’s two kinds of people interested in setting up Orchard. For lack of better terminology, I’ll say its either an end-user or a developer. We will be working , as a developer, off of the Orchard Source, but for the sake of completeness I’ll mention how to set up the built version also,as if you were an end-user.

<!-- more -->

##Prebuilt Setup

I’m mentioning this here, because this is what you’ll find most often out there on the web. Want to get started in Orchard? Just go get Webmatrix and click install for orchard! Its important to understand what you’re installing here! You’re installing a build of Orchard. This is the small ready-to-deploy version. This is not what you want if you are interested in building modules yourself. This is for absolutely minimal resistance in setup.

First get [Webmatrix](http://www.microsoft.com/web/webmatrix/). Its already on version 3! This is really amazing to me. It was just released in 2011, and its made so much progress. Version 2 was out just last September. This particular tool is moving at breakneck speed, and its free.

So once you have Webmatrix, and you’re ready to make a site you go to the App Gallery (formerly Web Gallery).

{% img /images/orchard/orchard1.png %}

Then, you click on the Orchard CMS Button.

{% img /images/orchard/orchard2.png %}

Easy. Click Next a few times, and the installer literally does all the rest.

{% img /images/orchard/orchard3.png %}

Couldn’t be easier, right?

Within seconds of the bar completing, you click OK, and you’ll be prompted in Webmatrix with a folder structure sort of like this:

{% img /images/orchard/orchard4.png %}

More on that later…

And a big giant prompt like this:

{% img /images/orchard/orchard5.png %}

Go ahead and fill this out, you site name and what you want the superuser account to be. Just select Default for the Orchard Recipe. Select SQL Server Compact for data storage.

Now you have a site. Cool! At this point, you’d start configuring it like you need to, possibly install a module or two form the gallery. Probably install a different theme, and so on. Even though this is the non-source setup, you do have some latitude in configuring the project, you can alter the html on the views, namely and possibly fiddle around with some CSS. Don’t expect to do heavy development here though, that’s not what this workflow is really for.

Look at the folder structure for a moment. You are looking at what in the source will be the Orchard.Web project – one of 67 or so projects. You’ll notice that in this built project, you have every module listed in your module folder. It will be like this in the source too, though in Visual Studio you’ll see an aliased folder. Every image you add, and every piece of media you upload ends up, some way in the Media Folder.  Every theme you make or download ends up in the theme folder.  Its generally quite organized.

I want to draw attention to one very important folder: App_Data. Open that. Now drill down Sites –> Default. Look in there. see the Orchard.sdf file? That’s your site. Basically.

{% img /images/orchard/orchard6.png %}

If your site gets completely blown away, you can reinstall it all, and drop that in and all your blog posts, and content will be back. I hope this makes it clear how easy it is to back up an Orchard site, you just really need to account for the Media folder, because it has all your pictures and stuff, and this folder in your Sites.

So at this point, you can put this Orchard install on your server, or push it to Azure or whatever you like. Boom, you installed Orchard. Beyond that its up to you. You might like simply editing posts in the online editor, or you might set up Live Writer to edit them. I’ve written about how to set up Live Writer in the past. Its not hard, and it works well. The bottom line is that I can’t stress the importance of your Media Sites –> Default folders.  Back these up.

Recently, my wife’s blog was spammed to the max. Eventually the attackers were able to bring down the blog. A similar situation in Wordpress or some other system would have really bothered me, but with her we simply backed up the imporant folders, then pulled our personal Orchard build from github, replacing everything. Then? We replaced the Media and Sites—> Default folders.  It was a complete recovery.

Next though, we need to get setup from the source. I’m gearing these tutorials to be used in Visual Studio. Honestly, if you’re a c# developer that’s the way to go. you get all the advantages of your favorite IDE, such as Resharper – and also you can build the project.

We’ll cover that next.
 

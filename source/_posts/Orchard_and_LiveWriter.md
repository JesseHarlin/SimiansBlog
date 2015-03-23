title: "Orchard and Livewriter"
date: 2012-2-1 8:30:00
tags:
---

I decided to make a quick post detailing how to connect Orchard CMS with Live Writer. There are a number of advantages to using LiveWriter, rather than the HTML editor on the remote blog. For one, you can work offline, which might be handy now and then if you’re in a situation where you don’t have a decent internet connection. A second advantage is the fact that you can keep a local backup of all your posts. I don’t recommend this being your only backup, of course- but hey a little replication doesn’t hurt. Of course my favorite reason is to make use of the superior editor and plugins. Really, the editor is nice. It feels like Word with spell check and such, but you can also go into the source view and make sure everything is clean. It doesn’t inject a bunch of junk into your markup either.

Ok, so the first thing to make sure of, is that you actually have Live Writer already installed. In my case I did, because it came with the Windows Live suite, but if you don’t : Here is the link to get it.

Now lets assume you’ve already deployed Orchard on your server, or into the cloud or however you wish to do it. I started Live Writer and was Immediately prompted with this:

{% img /images/orchard_and_livewriter/choose-blog-service-orchard-livewriter_thumb.jpg %}

Make sure you pick the last option: Other Services. Now you’ll be prompted to fill our your blog information:


{% img /images/orchard_and_livewriter/orchad-and-livewriter-fill-out-settings_thumb.jpg %}

This part may give you some trouble if you’re not sure what to type, so I’ll be clear here. The address is not for the address of your root Orchard installation(your homepage) , but for the blog itself. Orchard is capable of hosting many blogs per orchard site, you see. If you are using Orchard though, for just one blog – that blog may very well be your homepage in that case. For me, my blog wasn’t my homepage at all – it was somewhere else. As for the Username and password, that is the user that will be posting. The user needs permissions to post of course. Then type that user’s password in. This might be your admin name, it may not- but either way the user needs to exist on the Orchard site and needs to be able to make blog posts.

So everything is good … you click next and

{% img /images/orchard_and_livewriter/orchard-and-livewriter-mess-up_thumb.jpg %}

Whoops. If you saw this, then something went wrong. The most likely culprit is that your Orchard installation did not have remote publishing enabled. No problemo, that’s an easy fix.

{% img /images/orchard_and_livewriter/orchard-and-livewriter-enable-remote_thumb.jpg %}

I logged in, and went to the modules section in the Dashboard of my site. You can search for the module, and enable it. In case you’re curious, this all works by Xml-RPC. That’s “eXtended Markup Language Remote Procedure Call”, for what its worth. If you’re really curious, Orchard has some documentation about it. Try connecting again, just hit the back button and then re run it.

If things go well, you get a different window, after Live Writer Discovers your blog.

{% img /images/orchard_and_livewriter/orchard-and-livewriter_thumb.jpg %}

Basically, Live Writer is attempting to discover your blog’s theme in order to make the blog preview. go ahead and select ‘Yes’. In my case, the preview wasn’t perfect – some of the formatting wasn’t quite right, but it was useful enough to help give me an idea.


{% img /images/orchard_and_livewriter/livewriter-and-orchard-name-blog_thumb.jpg %}



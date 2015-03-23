title: "Flirting with Gulp: Part 1"
date: 2014-03-6 12:00:00
tags: Gulp, Task Runners, Javascript
---

Observation: Quality Javascript taskrunners tend to be named after gutteral noises. Fascinating.

I guess lately I've been having a sort of love afair with Gulp. Its interesting, considering I actually adopted Grunt fairly recently, just last year. ( I know, I'm late to the party). I picked it up when Grunt 0.4 Came along. I remember it was much nicer than the previous version. Everything was a plugin (rather than builtin), and it made sense to me. Grunt rocked, and still rocks my world. Its so very easy. Theres really only three things you need to know: To pull in the plugins, to configure them declaratively, and to make tasks. Then, you run them. Simple. 

The big thing with grunt, I've noticed whilst working with it, is that the declarative JSON...mammoth.. gets pretty big after a while. I'm aware there are strategies for refactoring and organizing your Gruntfiles, i just feel like I often can't bring myself to find time to just sit and simply refactor my Grunt tasks. So lately, at the shop, we've been working on a scaffolding project to help us get up and running on future applications more rapidly. Now is my golden chance to refactor Grunt.. or?

Gulp? Should I explore this as a viable option? Why not?

These tools clearly compete for the exact same 'job' in my toolbox. One works off of configuration, the other uses streams; its sort of 'code-first'. I like that.  I am reminded of when I switched to fluent nHibernate configuration years ago, rather than xml configuration. It really made everything easier to read and compose. Maybe I could achieve similar benefits here. It seems equally simple. Theres four main things, 'task, watch, src, dest'. That's really all. Also the modules seem smaller, and the concerns of each seem more seperate. And the most interesting part, is that the tasks seem to run as concurrently as possible. That, plus the fact they are essentialyl streams could result in much faster execution times. I suppose that isn't as big a deal when you're doing a preprocessing task, but during a file watch it can be really handy. It can also be handy if there are tasks related to CI.

OK, let's play.
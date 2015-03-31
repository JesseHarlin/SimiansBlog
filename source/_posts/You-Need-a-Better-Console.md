title: "Using Windows: You Need A Better Console"
date: 2014-3-7 11:30:00
tags: Conemu, Console, Tools
---

I use a lot of operating systems. Regularly, I use Windows, OSX and Linux. It seems like at work, unless I'm doing somethig with virtual machines, I'm typically on Windows. Now, I am not a fanboy of any particular OS. I believe they all have strengths and weaknesses, and I find I use them differntly, for differnt tasks. One very admitable shortcoming in Windows, is most certainly its console. It Just Sucks™.

Now if you work in git, you might have installed, at some point, for windows the bash console emulator, Msys Git. Its nice, but not perfect. If you do any node development, you probably have many, many consoles open. Some are bash, some are standard windows. It gets annoying.

Lately I've been using Conemu. Its honestly fantastic, and frequently updated. And really resolved the issue of working between worlds. I did a few things to get it set up like I like.

One of my favorite things about the Git console is, admittedly this little feature:

{% img /images/you_need_a_better_console/Git-Bash-Here.png %}

uch a little thing, makes a big differnce.

In conemu, do the following:

1. Open the settings. Either ightclick and select fromt he menu or Win+ Alt + P. Whatever.

{% img /images/you_need_a_better_console/conemu1.png %}

2. Go to integration. Make sure the "Conmemu Here" is there, and the command looks correct. Should say someting like "smd -cur_console:n"

{% img /images/you_need_a_better_console/conemu2.png %}

3. Click Register.

4. Magic

{% img /images/you_need_a_better_console/conemu3.png %}

You can also do cool stuff like automatically open Conemu with certain tabs open, use differnt themes and a lot of other stuff.
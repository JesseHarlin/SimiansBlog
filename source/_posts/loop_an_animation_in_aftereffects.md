title: "How to Loop an Animation in After Effects"
date: 2013-3-3 9:00:00
tags: Animation, After Effects, Scripting
---

Occasionally I fond myself doing work in Adobe After Effects. One of the most common tasks I do, when compositing is have one of my clips loop indefinitely. This seriously beats the alternative: copy and paste hell.

Its really simple to do, and all you have to do is write one line of script.

Here is an example of how to do it. I have a clip I’m working here, where I’m trying to simulate an old school NES password prompt.

{% img /images/loop_an_animation_in_aftereffects/ae1.png %}

<!-- more -->

See that Gold  Square up there? I need that to blink, repeatedly- like a cursor. Then, I need to move that around and a typing animation plays, and this leads the text as it comes in. The effect is as if a person is quickly entering a password. Its really kickass. Lets make the cursor.

I go into Photoshop, make a rectangle, add bevel and emboss  as a layer effect and voila: a cursor.

Cursor
{% img /images/loop_an_animation_in_aftereffects/ae2.png %}


There is is. Bask in its majesty! A Cursor. Whooo!

Now, I take the cursor and add it to my After Effect’s project resources.

{% img /images/loop_an_animation_in_aftereffects/ae3.png %}

Wonderful. So far, so easy. Now I want it to flash. I make a new composition, and simply adjust the opacity, to make it fade out.

{% img /images/loop_an_animation_in_aftereffects/ae4.png %}

This composition is small – only 5 frames. this thing is going to flash quickly, like a password cursor prompt does. So, lets make it flash in my composition. This is a simple 3 step process.

{% img /images/loop_an_animation_in_aftereffects/ae5.png %}

Step one: Enable Time Remapping. After you drag your composition onto the track canvas, just right click and go to the time submenu and click it. It will be checked now, and then you’re ready for part two.

{% img /images/loop_an_animation_in_aftereffects/ae6.png %}

You select that Fancy new Time Remap row that just popped up under your composition, and Add Expression. Its under the Animation submenu. A new track will appear under the Time Remap, and you can click in the row, and type whatever you need to. You’re ready for step three: the script.

{% img /images/loop_an_animation_in_aftereffects/ae7.png %}

On the row, type this : loop_out(“cycle”, 0). Now the 5-frame loop I made will repeat over and over.  The cursor is blinking! I can now apply any normal compositing techniques from here.

This technique is insanely useful, I use it all the time. A flickering flame, a flashing led, a walking sprite… It can all be done with this easy trick.

{% img /images/loop_an_animation_in_aftereffects/password.gif %}

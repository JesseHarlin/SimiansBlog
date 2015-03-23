title: "Constructing Your Own Drumkit in Battery (From Scratch)"
date: 2012-10-1 11:30:00
tags: Audio Editing, Digital Performer, Monitor, Aux, Bus, Insert Effects
---

One of my favorite sound sets is the 8-bit retro sound palette that one gets from the sample channel of an old Nintendo. I have a tool I really enjoy using in my studio, and that is a Nintendo that has modified cartridge that acts as a midi controller in it. We’ve had some good times together. I’ve been using this thing, literally since the 90s and its been a frequent go-to toy in my arsenal. As some point along the way, I swapped out my bulkier TV screen I used with it for a smaller one, I mounted on the front. 

<iframe width="100%" height="450" scrolling="no" frameborder="no" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/168244021&amp;auto_play=false&amp;hide_related=false&amp;show_comments=true&amp;show_user=true&amp;show_reposts=false&amp;visual=true"></iframe>

I’ve used it in a ton of musical tracks. A prominent one, was “God in my automatic Vacuum”, that I wrote nearly 9 years ago

{% img /images/battery_drumkit/nes_thumb.jpg %}

I’d like to keep using it and generally the melodic lines aren’t so problematic- but the percussion parts I tend to write choke this baby up quite fast. Do you remember when you were a kit, and the Ninendo would freeze up, and you and your friends would pull out the cartridge and ‘blow on it’. Yeah, I’m not sure why we did that. The trick was actually simply reseating the cartridge. What I am saying is that, imagine having to go through that ritual periodically as you are writing a piece of music. See folks this is why old gear has a MIDI panic button. On this? The Panic button is labeled ‘power’. that right, simply resetting the unit isn’t good enough; you also have to drain that flea power too. Seriously, it can get tedious.

{% img /images/battery_drumkit/8-Bit-NES-Cartridge-Blow-Me-Poster-Post-545x300_thumb_1.jpg %}

For those who know me, know that one of my favorite tools is Battery 3 by Native instruments. When it comes to working with percussion lines, especially in the context of electronic music, I consider it second to none. I’ve always wanted to make a battery kit of this thing. I clearly like it enough to keep it for a long time, so why not get it into the Virtual Instrument space? There is a clear theme, to this collection of sounds – and with Battery I can make multiple copies of my ‘8-bit’ drum set, run the individual cells through some awesome dsp, and generally have more control.

At this point, I am going to assume a very novice level of understanding of Battery 3 if you’re reading this. 128 sounds is the largest size of a patch Battery 3 can support (16 x 8). For me, at least ; this is what we want. There is no reason to make a smaller kit than this, we have 8 bit samples aplenty.

In battery you can use the + and – buttons in the corner of the matrix to add and remove columns.


{% img /images/battery_drumkit/bat_thumb.jpg %}


That’s nice and all, but the most efficient way is to click the edit dropdown and select the appropriate matrix size..in this case 16 x 8.

{% img /images/battery_drumkit/batttery-select-matrix_thumb.jpg %}

Now we need to get our samples. In my case, I am going to be using Digital Performer to capture them from the NES. I set up a MIDI track an aux and an audio track.

{% img /images/battery_drumkit/analog.png %}

I’ll explain why I’ve done this. The mono audio track is where I will actually be capturing the samples. The aux track is so that I can use inserts to clean up the samples on the way in. In this instance, the NES is a dirty , dirty little machine. Its noisy, glitch and in dire need of dynamics effects.


{% img /images/battery_drumkit/cells.png %}


The first item is the gate. Really the only point of this is to make the track actually silent when the samples aren’t playing. Next, is a preamp. This, I am using simply to sweeten the sound a bit. Directly from the NES the sample was a bit dull. This added a bit of compression and some punch on some of the bassier clips. The final unit is a multiband compressor, that I am using to tame some of the wilder samples. and bring up the quieter ones. Since the NES responds pretty differently depending on the frequency range, I used a 3 band multiband compressor. I had good results with that. The final effect you see above in the insert sequence was a Limiter. I found I really ended up not needing that after all, so its bypassed here.

The midi track is dead simple- its simply a chromatic scale that ascends, to fire each sample one after the other on the hardware unit.

Now? We capture. This admittedly took a few tries. 80s hardware likes to croak now and then, and occasionally I had it fire samples twice or thrice.

{% img /images/battery_drumkit/slices.png %}

See how frequently it croaks? Those dark bars are where it fires more often than It was supposed to. Don’t get me wrong- I really really like the unpredictability of it on some level, but for this I wanted to keep things a bit clean, relative to my usual workflow.

Now Lets make a place to save out kit, but first a little bit about battery kits.  Check out the folder structure below. As you can see, we can organize our kits by folder, and then the kit consists of a folder with a particular name, and then in that is a folder with the samples, and in the parent directory is the .kt3 file.

{% img /images/battery_drumkit/directory.png %}

This corresponds to this in the editor:

{% img /images/battery_drumkit/editor.png %}

This is handy because it makes it pretty easy to organize your kits. So, I’m going to essentially duplicate this kind of structure for my own kit. There will be a folder for samples and I will save the kit itself in the parent. So first I duplicate the folder structure.

{% img /images/battery_drumkit/directory2.png %}

Then, when I go back into battery  “Save As” and I choose the Nes Kit Samples for the sample Sub folder. Unfortunately when I did this, I got an error related to permissions.

{% img /images/battery_drumkit/directory3.png %}

The fix is simple. - just leave the chosen folder spot blank- let battery make the folder for you. Thanks Battery. After saving you have this:

{% img /images/battery_drumkit/directory4.png %}

So far, so good.

Ok honestly, this next part is the tedious bit. There might be some shortcut way of doing this, but I don’t know it.  It will involve exporting each sound, and naming it. Thank goodness this only happens once.

First Select the waveform, and make sure you also select the master track too. Bounce it to disk. I bounced as a 16-bit wav. That works fine.

{% img /images/battery_drumkit/bounce1.png %}

{% img /images/battery_drumkit/bounce2.png %}

Make sure you saved to the samples folder, it should appear in the folder now.

The final step-You drag the sample to the target cell you wish to map to. You can even drag a batch of samples, and they will map sequentially, one square after another.

{% img /images/battery_drumkit/just_drag.jpg %}

Boom just drag. Here is my amazing kit with ONE whole sample! Now I’m going to do this for all the others. As I go thorough he waveforms I intend to audition each one, normalize it and then bounce it. Name them as best you can.

It’s the same repetitive process over and over. Snip out the waveform, then I was using the Mono Trim Plugin to normalize and audition simultaneously. Honestly, naming these things is the hardest. Ever think about how many adjective you really have to describe a bass drum? Less than you think.

That’s it for part one. Next time I am going to cover how to make multiple kits form the sample set, and what, in my opinion constitutes a good kit. Truthfully, one sample set can yield a lot of versatility, and with a few tricks you can make you kits sound absolutely awesome with Battery.
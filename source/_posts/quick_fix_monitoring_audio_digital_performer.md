title: "A Quick Fix: Monitoring in Digital Performer"
date: 2012-8-1 11:30:00
tags: Audio Editing, Digital Performer, Monitor, Aux, Bus, Insert Effects
---

Recently I was working on a short project, and I was in need of monitoring my input audio as it was being passed thorough my inserts. As you know the later versions of Digital Performer (in this case 7.24) have a really useful column you may have noticed: the “Mon” column next to the input column in the tacks view of the sequencer.

That’s the column there, with the lighted speaker icon. Now you can hear the input as you send your midi sequence.

{% img /images/quick_fix_monitoring_audio_digital_performer/tracks.png %}

However I ran into a problem. Usually when I’m recording a certain persnickety external module – and by that I mean my MIDI enabled NES. I need to run the incoming audio through a little bit of dynamic effects. For one, the Nes emmits a terrible 60hz hum, and is generally noisy. I don’t mind this so much on the actual percussive impulses of the notes – it adds..er… flavor (read: 8-bit trash ). the problem is when the instrument should be tacet it makes a hum.

The point of this, is that I needed to be able to monitor the affect signal. I tried my usual trick of running through an aux..but.. it wasn’t working! Why?

The stranger part of it all,w as that it worked wonderfully with virtual instruments. And I said to myself “Bro, do you even know how to Aux/bus?”

Then I found the answer. It was in a strange location, so hence the post – After messing with the Audio Patch through, Audio Monitor, Settings and a host of other settings, the answer was right up in the menu.

{% img /images/quick_fix_monitoring_audio_digital_performer/setup.png %}

In the Configure Audio System fly out, there is an option for Input Monitoring Mode. Select that.

Then, allow Monitor record-enabled tracks through effects.

{% img /images/quick_fix_monitoring_audio_digital_performer/input.png %}

I don’t know about you, but I found this setting to be in sort of an unusual place. I think I initially predicted its location to be in the Bundles menu, but I suppose that’s not an optimal spot eiter.

In any case, that’s the fix.
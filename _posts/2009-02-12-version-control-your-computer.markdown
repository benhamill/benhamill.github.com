---
layout: post
title: Version Control Your Computer
---
I've mentioned @<a href="http://twitter.com/carl_youngblood">carl_youngblood</a> here before. Someone once was trying to buy him something with his name on it. I think it was a key chain. You know the kind, right? However, then didn't have "Carl" only "Carlos". So we joked that, one day, he needs to write an operating system and name it CarlOS. Aren't we funny? I know. I'm sorry. Anyway, the other day, we actually got into some OS discussion that I thought had some interesting enough ideas to post here.

So how many computers do you own and use? I've got a desktop at home, a laptop and a machine at work. It's sort of a bummer to have different stuff or different versions of stuff, or stuff with different preferences on different computers. At least, for me it can really jack up my work flow. Especially if there is some application I use a lot with non-default preferences. Man, that bugs me! Remembering it all, bleh.

One thing Carl's fantasized about is having a computing environment the same everywhere you go. That's sort of a mainframe or dumb-workstation idea, which is not new at all. However, what if your whole computer were version controlled? You could branch it (so you don't have your work apps at home, etc.) and merge changes from one branch to another, if you wanted. You could check out a different branch on one machine and it would feel like you were on another.

Clearly an OS would have to be built from the ground up for this idea. You'd also have to have some kind of provision about storing the non-checked out branches locally. Also cloning the repo would be a hassle at current average (even high speed) connection speeds. But how cool would it be to install, say, Textmate at work and get all your settings right, etc. and then go home and merge that change in (You could merge it from work, I guess and then just pull from home. Whatever.)? You could get diff data (hard to implement, but with metadat not impossible):

``` diff
$ os diff gaming HEAD
+ Steam
+ Half-Life 2
+ X-Fire
- Textmate
```

Or whatever. You get the idea. Reverting would making backing up and creating, uh... what does Windows call them? Recovery Points? It would make all that easy and moot. Clearly Linus Torvalds needs to be in on this "project"; he has the experience in both OS design and version controlling that would be invaluable. Not that, you know, Carl or I are actually considering doing anything with this idea. It's an interesting thought experiment, though.

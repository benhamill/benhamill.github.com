---
layout: post
title: Save Versus GitHub!
---
I'm reaching a point where I want everything in my life to be version controlled. I made a desktop making fun of my friend in the GIMP the other day and realized that I wanted it version controlled. I don't think Git will handle .xcf's. Pity, though.

My most recent action on this front doesn't actually have to do with Dungeons &amp; Dragons, but that's only because my system of choice is GURPS. I game master role playing games as a hobby and keeping track of campaign ideas has, in the past, been very disorganized and messy. I had scraps of paper all over and various emails to myself. If I have some brilliant idea at work, I can't just incorporate it into my notes or whatever, I'd have to email it to myself and then hope it was clear enough to remember what the actual idea was later, etc. If I was on the bus, I had my laptop (which I use to assist in GMing) and I could put it right into the notes, but recently I had some major stability issues with that machine and so became concerned about backups, etc.

Thus, I <a href="http://twitter.com/benhamill/status/1090481503">had an idea</a>. I've converted the essentials from my current campaign and put them in a repo and I'm working on notes for my next campaign there. There are several benefits, here:

The first is that before, I was using OpenOffice documents for my notes. This allowed for some pretty formatting, but when I had my timeline open, my NPC list, my session notes and my location notes all open, well... Open Office isn't a lean program and my lappy isn't the beefiest of machines. So now everything is a .txt and that's super lean. Yay. I'm aware, by the way, that this is very tangentially related to version controlling my notes, but still.

Secondly, I can check out a copy on any machine I'm sitting at when I have an idea. Or, if I really want to, I can edit them right on GitHub. Neat. As a sort of corollary to this is the fact that if my lappy were to get dropped, say, off a mountain, I could borrow anyone else's laptop and be ready to roll in about 20 minutes as long as I had internet access.

Thirdly, since I'm using git as opposed to, say, SVN, I don't <strong>have</strong> to have internet access. Local repos means I can make a commit on my laptop while on the bus and then push when I get home. Very handy since I do a lot of my thinking about campaigns on the bus.

Fourthly (this list is getting longer than I thought it would), is character data. So there is a piece of software that you can buy to help you create and track GURPS characters (whether non-player character or player character). Handily, it saves them in plaintext (I think it's actually .Net code or something unhelpful, not YAML or XML or similar, but nothing's perfect), so I can version control the characters, too, not just notes.

Fifthly (good grief), a friend of mine and sort of my GM mentor moved away and doesn't have a gaming group. In order to get his fix, he's convinced me (for the good and the bad of it) to let him help me plan and brainstorm my next campaign. He can check out a copy, branch it, issue a pull request (or I'll give him push access, not sure). Collaborative GMing is something that often can go wrong, but this tool, teamed up with some other guidelines we've adopted will help ensure that we get only the benefits out of this.

About the only things that I'd use for a campaign that it won't version control are pictures and sound files, but I don't expect to do a lot of changing of those over the course of things and, any way, it <strong>will</strong> save them, so it at least acts as a backup. While we're talking about negatives... My players could snoop the notes. Oh noes! In reality, I'll have to just trust them to stay out. They'd only be ruining their own fun, anyway.

So, a sixth, I guess, benefit is that I can share my notes with the world and if someone else sees something cool they want to steal or sees something sucky that they can do better then I've inspired them or at least helped them out a bit. If you like (and aren't one of my players), feel free to <a href="https://github.com/BenHamill/rpg-notes/tree">check it out</a>. If you have questions about anything in there, feel free to shoot me an email. I make no promises that anything in there will be better than total suck.

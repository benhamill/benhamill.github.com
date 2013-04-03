---
layout: post
title: A Hub for Gits
tags:
- git
- philosophy
- programming
status: publish
type: post
published: true
meta:
  _edit_last: '2'
---
I've recently started using git to version control my personal projects. I've also recently started using GitHub for hosting remote repos of that stuff. So I'm new to it all and I might be wrong, here. But, having read a few articles here and there and talked to some other people (most notably, another git newbie @<a href="http://twitter.com/carl_youngblood">carl_youngblood</a>), aren't cherry picking and rebasing really, really horrible things to do to a repo? Even if it's just your local one? They destroy history, which is sort of the point of version control, no?

I've seen, in the last few days, <a href="http://github.com/blog/270-the-fork-queue">two</a> <a href="http://schacon.github.com/history.html">articles</a> on GitHub that make me wonder which of us (me or GitHub) doesn't get it. My inclination is to assume it's me who's missing something. If so, I'd love for someone to tell me exactly where I've missed a step.

The first article I want to talk about is the <a href="http://github.com/blog/270-the-fork-queue ">Fork Queue</a> announcement. It's basically a tool that makes it really easy to see which of the people that've forked your project have pushed commits you don't have in your repo and then to cherry pick them in. You can pick your branch, etc. This is to keep you from having to create a lot of remotes, I guess. It's supposed to "[allow] you to do a email patch style workflow without actually having to deal with patches over email". I thought that part of the point was that that work-flow was a pain? I also feel like it's missing the part where the person making the patch tells you about it, rather than you going and getting it from them. A pull-request is much more like that.

I don't know... I sort of feel like we should be putting roadblocks in the way of cherry picking; make it harder for people to adopt work-flows where cherry-picking is common. My understanding (and again let me stress that this may be incorrect) is that the <strong>best</strong> work-flow is for the patcher to fetch your code (because it represents some kind of "core" or "official" release, yes?), merge it into a new branch, make his(her, etc.) changes, test them, fetch your code again to make sure he has the latest, perhaps retest, then issue a pull request. When you're acting on the pull request, you fetch his stuff down to a new branch, test, possibly merge in any changes you've made to master since he issued the pull request, then _merge_ into master. This preserves all the history. It's all fetches and merges.

So the other article is the <a href="http://schacon.github.com/history.html">Changing Git History</a> article. This one is about going and messing with old commits. I'll try not to rant as much on this one. I'm not as adamant, but I do find it kind of silly, this idea of making a commit a perfect little gem. I can see fixing a typo in the commit you just made, so <code>git commit --amend</code> doesn't seem so bad. However, the <code>git rebase -i</code> portion after that... bleh. I realize this isn't a GitHub feature like the above; it's a part of Git, but I wonder why. If the commits were that horrible, revert them all and do it over. If they weren't that bad, just live with the typo. No? Even if you haven't pushed it, rebasing just seem icky and to be avoided, especially if it's just to fix commit messages.

Okay... So, maybe I'm out of line or off base or insane. It's entirely possible. Some would say probable. It wouldn't surprise me if I've failed in some very basic way to understand the Git philosophy. It also might be that I've misunderstood what GitHub's written and that the clash between these articles and Git philosophy is all imagined by me. I'm open to these possible realities. Correct me or soliloquize or slam me in the comments, if you like. I am all ears... or eyes. Whatever.

<strong>Update:</strong> Before anyone gets the wrong idea, here... I've been loving using git and GitHub. They're both spectacular. Without them, I wouldn't have found enki for use in powering this blog. This post is about trying to understand something confusing in something great; I'm not trying to imply that either should be done away with or that I could do better. I just wanted to head off the most major take-it-the-wrong-way that occurred to me on the way in this morning.

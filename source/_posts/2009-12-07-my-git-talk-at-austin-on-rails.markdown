---
layout: post
title: My Git Talk At Austin On Rails
tags:
- austin on rails
- git
- rails
- ruby
- tutorial
status: publish
type: post
published: true
meta:
  _edit_last: '2'
---
At the last <a href="http://austinonrails.org">Austin on Rails</a> meeting (Nov 17), I gave a talked entitled Practical Git Quickstart (<a href="http://prezi.com/ovik2bljor84/">Prezi link</a>). The slides don't have a lot of content and mostly underscored what I hoped to talk about. I blew through them in about ten minutes or less. The short of it is that I feel like a lot of git tutorials and introductions start off with the high-level stuff and that, especially for people new to git, that that's confusing. My goal was to give git newbies the most basic commands they'd need to be able to use git on a daily basis so that they could build their own abstractions before diving into the more heady stuff. I was aiming for an 80% solution to that, anyway.

After I finished the slides, I fired up a command line and an editor and just worked through some stuff. This post should sum up what I talked about, more or less. I started out covering the same stuff I covered in my <a href="http://garbled.benhamill.com/2009/03/18/git-tutorials-suck-a-sucky-git-tutorial">previous git tutorial post</a>, so maybe go check that out first. It should get you through setting up a new repository, adding files to the staging area, making a commit, checking your status and committing to a remote repository.

So let's pick up there, with remote repositories. The way you get code up to your repo is with <code>git push origin master</code>. Once it's up there, other people can get at it. If you recall, you told git where your remote repo was going to be with <code>git remote add origin git@github.com:&lt;username&gt;/&lt;project&gt;.git</code>. Someone who wants their own local copy of your repo does so with the clone command like so: <code>git clone git@github.com:&lt;username&gt;/&lt;project&gt;.git</code>. That will create a directory wherever the command is issued, named &amp;lt;project&amp;gt; and pull down the current state of the remote repo. Then, that person will be able to push their own changes, etc. This is all, of course, assuming they've got permission to do so.

So this new second person makes some changes and pushes them on up. How do you get them? Well, sensibly, the opposite of push is pull, so you issue <code>git pull origin master</code>. This is actually a two step process that's just for convenience. I don't want to get into the plumbing too much, but it basically grabs the state of the remote repo (<code>git fetch</code>) and then attempts to merge (<code>git merge</code>) it with your local stuff. So that's the most basic case of working with someone else on a project, or working alone on one using different machines, if you like. I use that case all the time.

So what about conflicts? If you both make a change to the same file and they <code>push</code> it first, you'll not be allowed to <code>push</code> because git can't handle the <code>merge</code> on it's own. Similarly, if you try to <code>pull</code>, it will do the <code>fetch</code> part, but be unable to <code>merge</code> and will tell you so. You can use <code>git diff</code> to see what the changed were and do the merge yourself. You can also use <code>git difftool</code> which is awesome, but takes some setup, so you should look into it later on (I skipped it in my presentation).

Once you handle the conflicts, you'll add the conflicting files to the staging area and make a commit. With all merges, I should note, git makes a commit just for the merge, so when you have conflicts, it'll have staged the things it can merge on its own and left the conflicts unstaged. As you fix them, you stage them and then you commit the merge commit. Git doesn't know if you really fixed the conflicts, so you can <code>git add</code> whatever version of the file you want, even a broken, not-conflict-resolved one. Just be aware.

That was more or less the end of my ordered presentation. There were some questions afterward and I'm going to attempt to sum up the discussion that followed, here:

First off, I wanted to mention how you ignore files in git. Unlike subversion, there is no <code>git ignore</code>. If you want git to ignore a file, you have to add it to a <code>.gitignore</code> file. This file is a list of patterns that git will ignore for the directory it's in and all directories below it. So you might have one for a python project like this:
<pre lang="text">tmp/*
*.pyc
</pre>
This will ignore all compiled python code (*.pyc) and everything in your tmp/ directory. I was baffled by this when I first came to git, but it's not really that hard. Note that you generally commit your <code>.gitignore</code> so that others can share it. If there's something you want to ignore on a per-machine basis, rather than a per-project basis, then you need to turn to my next topic.

Which is global git preferences. On Linux and Mac, git will look for a file in your root directory called <code>.gitconfig</code> and take global behaviors from it (it's tricky on Windows, and I haven't figured it out to my own satisfaction, sorry. If someone asks about it, I'll try to sum up what I know in the comments). In my other git post, I had gone through setting up a repo on <a href="http://github.com/">GitHub</a> and said to follow the directions there. Two of those steps were these:
<pre>git config --global user.name "&lt;your name&gt;"
git config --global user.email &lt;your_email&gt;
</pre>
Those created entries in your <code>~/.gitconfig</code> telling git your name and email address. You can also declare a global ignore file there. I like to call mine <code>.gitignore</code>. This is shockingly original, I know. On the machine I'm typing on right now, my <code>~/.gitconfig</code> looks like this:
<pre lang="text">[user]
email = blah@blah.blah
name = Ben Hamill
[core]
excludesfile = /home/ben/.gitignore
</pre>
I bet you can guess it, but just in case, you can either put your excludesfile in manually or do <code>git config --global core.excludesfile /whatever/file/path/you/want</code>. For reference, my <code>~/.gitignore</code> looks like this:
<pre lang="text">*.kpf
*.swp
</pre>
A .kpf file is a project file created by Komodo Edit, which I used to use for all my code editing needs, but not since I switched to <a href="http://blog.benhamill.com/2009/11/25/vi-improved">vim</a>, which is what creates *.swp files.

Finally, someone had asked about <code>git stash</code>. It's what I'd consider a more advanced command, but a lot of git fanboys sell it hard because it's cool and svn doesn't have it. However, as cool as it is, I think it can get you into a lot of trouble. Basically, you can be working on something and issue <code>git stash</code> and git will store whatever changes you're in the middle of and hide them away, putting your repo back in the state it was right after the last commit. You can then work on something more pressing, make commits, merges, new branches, whatever and when you're done, issue <code>git stash pop</code> and it applies your changes back (if it can).

The really hairy bit is that you can name stashes and so have more than one stash going at once. While a super organized developer might find this really useful, I find that it's easy to get stuff lost in there. You don't want to have tons and tons of stuff stashed and not remember, anymore, what changes were in which stash, etc. I advise, as a basic rule of thumb, that if you've already got one thing stashed and find yourself wanting to stash something else, then you should be looking at branching, not stashing.

I think that about covers it. I think someone recorded audio of my talk or maybe video. If it ends up posted somewhere, I'll come edit this post with a link to it. If you were at my talk and notice something I talked about then that I haven't covered here, let me know and I'll try to amend. Or, if you weren't there and feel there's a topic you have questions about, drop it in the comments and I'll do what I can.

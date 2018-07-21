---
layout: post
title: Git Tutorials Suck, A Sucky Git Tutorial
---

<h2>Context... Perhaps Too Much Of It</h2>
So I was reading <a href="http://byorgey.wordpress.com/2009/01/12/abstraction-intuition-and-the-monad-tutorial-fallacy/">this blog post</a> about learning and explaining because Carl Youngblood <a href="http://twitter.com/carl_youngblood/status/1115967090">tweeted about it</a>. I think Carl's right: I had a hard time learning git (by which I don't mean to imply I'm some sort of expert now, but the learning is going easier now).

I think the main problem that I had was this: Having learned Subversion, with it's central repository, it was a hard abstract thing to understand. And some (I feel many of the ones I read, anyway) of the tutorials out there try to start at the abstract. Little help that did me (see above-linked article. Really, it's very good). And even ignoring those, I had to read a lot lot <em>lot</em> of the practical ones before things started sinking in.

So I've sort of come to understand that, actually, the tutorials don't suck; learning abstract things just takes time and, at the time, that can be frustrating. So I'm going to offer my own little sucky tutorial, which will focus on the practical aspects and, if you read this and don't get it, you can follow some links at the end to other articles I found helpful and maybe, after roughly a week, you'll have your 'ah-Ha!' moment and think about how git is just like monads... whatever the heck those are.

A lot of tutorials for git newbies start out explaining the Staging Area with some kind of metaphor so that it seems friendly or, I suspect, out of some subconscious wish to actually obscure it from Subversion converts so that git seems more familiar--more like SVN, which it is not very much like at all. I'm not going to really talk about it much. When we get to the commands that affect it (shortly, here), I'll explain what they do. You can make the abstraction your self.

I'm intentionally writing this off the top of my head for two reasons: If I have to look up a command, then you might as well read whatever tutorial I looked it up on and if I have to look it up, then I clearly don't use it all the time and thus, you don't need to know it to get going on Git.

<h2>The Tutorial</h2>
I've got six sections to this thing with (I hope) at least vaguely descriptive names. They are:
<ol>
	<li>Setup</li>
	<li>Initial Commit</li>
	<li>SitRep</li>
	<li> Staging Area</li>
	<li> Remote Repo</li>
	<li>Conclusion/Links</li>
</ol>

<h3>Setup</h3>
You have a project you just started in a directory called 'notes'. This isn't even code, it's just notes about something that you want to version control and back up. It's a collection of text files and the directory structure is something like this.

```
$ pwd
~/notes/
$ ls
contact_info.txt  general.txt  outline.txt
```

After <a href="http://git-scm.com/download/">installing git</a> as appropriate for your operating system, you start out by typing in the command line <code>git init</code>. This will create a directory called <code>.git</code> in <code>notes/</code>. There's some stuff in there, but for the most part, you can ignore this for now. Suffice to say it's where git does it's book-keeping. What you've got now is a local git repository or, as the kids say, a "local repo", but nothing's in it.

<h3>Initial Commit</h3>
So you do a <code>git add .</code> (note the trailing period). This will toss everything (that's what the period means) in <code>notes/</code> into the staging area (including stuff that's in directories that're in directories that're in <code>notes/</code> etc.). The repo is still empty. To actual save stuff once it's been staged, you do like this:

```
$ git commit -m 'Initial commit.'
[master (root-commit)]: created 7db8343: "Initial commit."
0 files changed, 0 insertions(+), 0 deletions(-)
create mode 100644 contact_info.txt
create mode 100644 general.txt
create mode 100644 outline.txt
```

The <code>-m</code> option says you're going to specify your commit message right after. Sometimes, you'll want to leave a longer message, in which case, you forget the <code>-m</code> and git will automatically fire up a default text editor where you can put in longer stuff. Since a lot of that varies widely from OS to OS, I'm going to skip it and you can read more details on other tutorials (see below). Notice that you get a list of what's changed (you created 3 new files in the repo) and you get your comment back in the output. Splendid.

<h3>SitRep</h3>
Now you've made your initial commit, and your stuff is in version control. Go into `contact_info.txt` and add something (doesn't matter what for these purposes). Imagine you've made that change and then walked away and forgotten about it. You can use <code>git status</code> to see what's new, thusly:

```
$ git status
# On branch master
# Changed but not updated:
#   (use "git add &lt;file&gt;..." to update what will be committed)
#   (use "git checkout -- &lt;file&gt;..." to discard changes in working directory)
#
#       modified:   contact_info.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
```

Using <code>git status</code> is just like a reminder. It doesn't tell you much, but it can jog your memory about what you've already staged or what you changed and didn't stage or what files you added. To get the real scoop about how a file changed, you use <code>git diff</code>. When you run <code>git diff contact_info.txt</code> the output will vary depending on what you had initially and what you added, but the gist is this: It will show you the changes (all of them) with a + before the line for additions and a - before the line for deletions. Generally, it gives a few lines before and after a change for context.

So let's add our new `contact_info` change to the staging area and commit it, yeah? Do <code>git add contact_info.txt</code> and then <code>git commit -m 'Updated contact info'</code> or similar. Whatever comment you write is fine. Note we could've used <code>git add .</code> but I wanted to show the single-file syntax.

<h3>Staging Area</h3>
Now let's put in some stuff into the <code>outline.txt</code>. Whatever you want. Just some stuff. Save it. But wait! We should also add some stuff to the general notes, just a quick overview at least, so put some stuff in there. We'll finish the outline changes in a second. This is so much more pressing. Obviously.

Now, it's good repo etiquette to only commit stuff atomically, which is to say that all the changes have to do with each other. Some people will say that you should only commit stuff that works (code compiles or whatever), but with git that's less of a concern. I'll come back to this point. What I'm getting at now is that you started one change and realized another needed to be made before you finished the first one. Now you want to commit only the second one, right? Simple: <code>git add general.txt</code> then <code>git commit -m 'Added overview'</code>. Because you never staged the outline (with your half-way-made changes), it doesn't get committed. Later, if you need to revert that commit or whatever, you won't have to worry that something else is mixed in there. Now, go ahead and finish your outline changes, and commit them. You should be able to do it on your own now.

<h3>Remote Repo</h3>
So, then... we're version controlling this stuff. What if you want to get at it from another computer or let someone else get at it or... something? Pop on over to <a href="http://github.com/">Git Hub</a> which is my remote repo host of choice. There are others. Shop around, if you like. After you create an account, you can <a href="https://github.com/repositories/new">create a new remote repo</a> called whatever you want. You'll then be shown a page with some directions. Follow the ones under the heading "Existing Git Repo?"

The <code>git remote add origin git@github.com:&lt;username&gt;/&lt;project&gt;.git</code> command basically tells git where your remote repo is. You can have more than one if you like and, actually, do all sorts of crazy things with naming if you like, but I just want to handle the default, assumed case with this tutorial. One interesting thing: Github gives you two addresses for each repository (other hosts may do the same, I don't know). The one that starts <code>git@github.com</code> is your read/write address and there's one that starts <code>git://github.com</code> which is your read-only address. Since this is your own repo, you want to make sure to use the read/write address.

The <code>git push origin master</code> command is what actually moves your commits to the remote repo. <em>This</em> is where I recommend you adhere to the "only stuff that works" doctrine. If this is code, and you're sharing the repo with your team or whatever, this is where they can get at it, so you don't want to hand them broken stuff or half-finished ideas or whatever. So only <strong>push</strong> code that compiles/works. Pushing your code updates the remote repo with all the commits you've made since your last push.

The way you (or someone else) gets commits out of a repo is by using <code>git pull</code>. It takes the same arguments as <code>git push</code>. It will pull the commits down and then try to reconcile those changes with any that you've made since the last time your local repo was in the same state as the remote repo.

<h3>Conclusion/Links</h3>
I feel like this has gotten pretty long and I don't want to put too much information all at once. That should be enough to get you started and, really, just try it out for a while and get comfortable with the basics. Don't be afraid, if you get something out of whack and realize you've done something wrong, to kill your .git directory (which will delete the local repo) and start again from the top. I've intentionally left a lot of stuff out (like push/pull and branches and multiple remote repos can get kind of hairy), so here's some documentation, blog posts and articles that I've found helpful. These are in no particular order and some are more advanced than others, so just start clicking and see what you like:

<ul>
	<li><a href="http://github.com/guides/home">Github's Guides page</a></li>
	<li><a href="http://git-scm.com/">The Git Homepage</a></li>
	<li><a href="http://www-cs-students.stanford.edu/~blynn/gitmagic/">Git Magic</a> (a huge work)</li>
	<li>Using Git Without Feeling Stupid <a href="http://smalltalk.gnu.org/blog/bonzinip/using-git-without-feeling-stupid-part-1">part 1</a> and <a href="http://smalltalk.gnu.org/blog/bonzinip/using-git-without-feeling-stupid-part-2">part 2</a></li>
	<li><a href="http://stackoverflow.com/questions/315911/git-for-beginners-the-definitive-practical-guide">Awesome StackOverflow page</a></li>
	<li><a href="http://cheat.errtheblog.com/s/git">$ cheat git</a></li>
	<li><a href="http://www.eecs.harvard.edu/~cduan/technical/git/">Understanding Git Conceptually</a></li>
	<li><a href="http://eagain.net/articles/git-for-computer-scientists/">Git for Computer Scientists</a> (highly technical and abstract)</li>
	<li><a href="http://whygitisbetterthanx.com/">Why Git is Better Than X</a> (Not only evangelical, but some helpful stuff)</li>
	<li><a href="http://learn.github.com/">Github's Learn page</a></li>
</ul>

If you want to ask me about git or whatever, feel free to email me or leave something in the comments. Also, if you spot a mistake or something here doesn't make sense, _please_ let me know. Hope this is helpful to someone.

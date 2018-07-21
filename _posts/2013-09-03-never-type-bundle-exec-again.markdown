---
layout: post
title: "Never Type bundle exec Again"
date: 2013-09-03 22:35
---
Sometimes people complain about having to type `bundle exec` before every
freakin' command in a Ruby project. Honestly, I think the benefits Bundler gives
you outweigh that downside by a *huge* margin. However, there is good news: You
can use this one weird trick to never type `bundle exec` again.

OK. It's actually an option of two weird tricks. And then a bonus trick. This
joke is ruined.


## Somewhat Involved, Works Everywhere

If you've got Ruby installed, you can do this trick. It's a little hacky and
comes with some downsides.

First up, make `~/.bundle/config` look like this business:

``` yaml ~/.bundle/config
---
BUNDLE_BIN: bin
```

This will tell `bundle install` to always run as if you passed the
`--binstubs=bin` option. Now, when you bundle up a project, it'll drop wrapper
scripts into the `bin` directory of your project that ensure the correct Bundler
environment is set up before running their normal stuff.

As a courtesy to everyone you share a project with, when you do something like
this which will litter your project up with files to do with your personal set
up, you should make sure you don't commit them to anything. So go to your global
git ignore file and make sure it includes this:

``` text ~/.gitignore
bin
```

Here's downside number one: You will probably have to override this for specific
projects. For instance, gems often ship with a `bin` directory with files to do
with the gem. You'll want to make sure those aren't ignored. If you explicitly
`git add` them, they should be tracked by git thereafter, but you have to go out
of your way. In particular, a Rails project contains wrappers for `rails`,
`rake` and `bundle` in the `bin` directory and those should be committed to the
repo... and *not* over-written by your `bundle install` binstubs. So take care,
there.

At this point, you could stop, except you'd have to type `bin/` in front of
every command, instead of `bundle exec`. Sure, it's shorter, but still annoying.
The next thing to do might be to prepend `./bin` to your `$PATH` so that your
command line looks there for commands first. Downside number two is that there
are some negative security implications to this, so I for sure don't recommend
it for servers, but I'll leave it up to you whether you do it on your personal
dev machine.

## Less Involved, Requires `rbenv`

"But, Ben," you say, "I want to be *just like you* so I use rbenv." Well, you
know, that's flattering and I have to say you make some good life decisions,
friend. Let me tell you about some nice tools.

If you use [rbenv](https://github.com/sstephenson/rbenv) (and I like it so
far), then you can take advantage of it's plug-in system. Probably you're
already using [ruby-build](https://github.com/sstephenson/ruby-build) as a
plug-in to install Rubies. I have two more for you:
[rbenv-gem-rehash](https://github.com/sstephenson/rbenv-gem-rehash) and
[rbenv-binstubs](https://github.com/ianheggie/rbenv-binstubs).

You can read more in the `README`s of those projects, but in brief, you install
them to your `~/.rbenv/plugins/` directory, then make your `~/.bundle/config`
look like this business:

``` yaml ~/.bundle/config
---
BUNDLE_BIN: .bundle/bin
```

Undo any steps you may have taken above (like ignoring `bin`) and shit should be
taken care of for you. Now you won't have to `rbenv rehash` *or* `bundle exec`!
Rockin'.

Note: The `.bundle` directory of a project that uses Bundler should already be
ignored in git, so tossing your binstubs in there should protect you against
barfing them into a repo unintentionally. Rbenv-binstubs handles making sure
they're in your path for you.

## Bonus Trick: `alias be`

I kinda lied to you. I'm sorry. You may still have to type `bundle exec` on
occasion, even with nifty rbenv-related goodness. Things that aren't gem
commands don't get binstubs from Bundler: notably, the `gem` command. For me,
this most commonly comes up when I want to see what the gem list is that
Bundler's resolving.

Doing `gem list` will show you all gems installed for the active Ruby. No help.
So you can do `bundle exec gem list` and Bundler will do it's path-related mojo
and then execute `gem list` for you. Nice, but typey. So the bonus trick is to
add this line to your `.bashrc` (or `.bashprofile` or whatever madness your
system might use):

``` bash ~/.bashrc
alias be='bundle exec'
```

Now you can just `be gem list`, which sounds sort of like an existential
confusion to me, but involves less typing, so... win!

## Final Caveat To All This

One last thing that's important to remember with all this: whenever you do this
kind of optimization of your local system, whether it's installing vim plug-ins
or putting `set -o vi` in your `.bashrc` or whatever, you have to keep in mind
that it's not portable. When you shell into your alpha server to tail some logs
or debug something, you won't have this stuff. So while you may never type
`bundle exec` again locally, you should remember that it's still there in the
background, and remember that on machines you didn't set up, you'll still need
to type it.

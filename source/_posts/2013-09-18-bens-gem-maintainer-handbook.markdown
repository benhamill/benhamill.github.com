---
layout: post
title: "Ben's Gem Maintainer Handbook"
date: 2013-09-18 21:31
comments: true
categories: 
---
[At work](http://context.io), recently, we were discussing internal gems and how
to deal with releases and crap. Of course, because this is a topic, I have
opinions on it. I can't help myself.

Actually, someone was like, "Why do we do it this way," and someone else was
like, "I don't know, ask Ben," and I was all, "Ranty rant-ra--You know what?
I'll just write a blog post." So, now you're all inflicted with this. You have
[Bob](https://github.com/bpot) and [Ryan](https://github.com/kerinin) to thank
for it.


## SemVer

So this is pretty simple, and other people have explained it better than I can.
**You should be using Semantic Versioning** for your gem. You can read the
[full spec](http://semver.org/), but I'll break down the general idea real quick.

Version numbers are 3 segments in the format major.minor.patch. You increment
the major version if you're shipping any backwards-incompatible changes. You
increment the minor version if you're adding backwards-compatible new features.
You increment the patch version if you're just doing bug fixes. When you
increment one segment, all the segments to the right reset to 0.

This way, a consumer of your gem can tell by looking at the difference in
version number whether the changes should be backwards compatible or not (major
version changed) or whether that new exciting feature they've been waiting for
might be in (minor version changes).


## Version Number

If I'm in a REPL with your gem loaded, I should be able to ask it what it's
version number is, so **put the version number in the code**. Ideally, I think,
it should be a string of the numbers. Some people seem to like to have
`MyGem::VERSION` be how you get it. Personally, I like to provide
`MyGem.version`, as well.

If you do this, you can then refer to it in your gemspec, where you officially
set the version of the gem. You should put it alone in it's own file, though, so
you don't have to load your whole gem just to interleave your version number
into the gemspec.


## CHANGES.md

**Keep a changes file**. There are a few file names that seem to be widely
recognized, but I like `CHANGES.md` the best. I prefer not to keep this
information in the `README` just to avoid cluttering that file for people who
are interested in documentation or contribution guidelines.

My `CHANGES.md` file has a `<h1>` at the top that just says 'Changes'. I don't
have strong opinions on that part. However, the following bit I have *very*
strong opinions on.

You have several sections, following the title. Each section is an `<h2>` that
is the version number of a release followed by a bulleted list of the changes
since the previous release. Each item in the list should be a brief summary of
a bug fix or feature--no reason to replicate your entire commit history here.
Personally, at least for publicly available gems, I like to also credit the
author of the change by name.

Each of those sections should go in descending order of version number so that
the newest stuff is at the top. There's also a special version: 'Unreleased'.
Between releases, you don't yet know what your next version number will be. Will
you add a new feature? Break backwards compatibility? Who knows. So I log
changes as they're merged to an 'Unreleased' section of `CHANGES.md`. This way I
don't forget what's in and it's easy for someone to see what kind of stuff is
coming in the next version (or what's in master that isn't released).

Rarely do people sending me Pull Requests add their own entry to `CHANGES.md`,
so usually, I'll merge their code, and then go add an entry for their change
myself. However, I would be delighted if a contributer wrote their own entry.


## Releasing

The point under discussion at work fits into this category. I always **bump the
version number a lone commit**, apart from any feature branch or other changes.
I feel like there are a couple of advantages:

1. It provides you an opportunity to review `CHANGES.md` in it's final state as
   you decide how you need to change your version number for release.
2. It means that you can add a feature that, as you write it, seems like it'll
   be the last one before a release and then at the last minute think, "Oh, just
   one more," and still have the version bump be the last commit before release.

Relatedly, **tag your release commit**. I just tag mine with the version number,
but you can put a 'v' in the front if you like. I don't care. The point is to
make it easy for a consumer of your library to go to Github and easily find the
commit that matches whatever released version they're using. Make sure to push
that new tag up to Github.

When I'm ready to release, I do like this:

1. Review `CHANGES.md` and select a new version number.
2. Update the version number in the code.
3. Change the 'Unreleased' heading in `CHANGES.md` to the new version number.
4. Run `gem build my_gem.gemspec`. If it doesn't build right, stop and fix it.
   Don't commit the new version number as part of that fix.
5. Commit the version bump changes.
6. Tag the new commit with the version number.
5. `git push && git push --tags && gem push my_gem-1.2.3.gem`
6. `rm my_gem-1.2.3.gem`.

I'm sure the above could be automated, but it never seemed like enough of a pain
in the ass to be worth my while.

So them's my opinions on gem maintenance. If you tweet or email me questions,
I'll update this post with answers.

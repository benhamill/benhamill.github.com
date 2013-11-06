---
layout: post
title: Install Ruby Enterprise Edition With ruby-build On Arch Linux
tags:
- arch linux
- os
- ruby
- tutorial
status: publish
type: post
published: true
---
I run [Arch Linux](http://www.archlinux.org/) on a computer, if I have a
choice. I recently installed it on the MacBook Air my employer bought me (which
saga is another post entirely). One of the things that I like about Arch is
that they don't do big releases every X-period of time; the package repository
is updated on a rolling basis. So I can get the latest version of whatever much
sooner than my Ubuntuan brethren.

That's awesome, but also has some costs. One of those costs is that gcc gets
updated for me way sooner than it does for people releasing Ruby versions and
sometimes there are weird things that older releases of the compiler were more
forgiving about or interpreted differently or whatever. So in order to get our
app at work that we haven't yet ported to Ruby 1.9.2 to run, I need to have REE
installed. This is how I got it done. This is one of those blog posts that I
hope gets indexed so that the next time I have to do this and I ask Google
about it, it will show me my own blog post.

<h2>Ruby-build</h2>

I like <a
  title="rbenv on GitHub" href="https://github.com/sstephenson/rbenv"
  target="_blank">rbenv</a>. A lot of people prefer <a title="RVM"
  href="http://beginrescueend.com/" target="_blank">rvm</a>. I don't want to
get into pros and cons or whatever now, but because I use rbenv for managing
multiple ruby versions, I use <a title="ruby-build on GitHub"
  href="https://github.com/sstephenson/ruby-build"
  target="_blank">ruby-build</a> to install multiple ruby versions. The way
ruby-build works is by executing recipes, which are pretty simple. Here is the
one I wrote to handle the install. It's just a variation on the stock REE
install recipe from ruby-build.

``` ruby ree-1.8.7-2011.12-stdout_patch
build_package_stdout_patch() {
  wget 'http://bugs.ruby-lang.org/attachments/download/1931/stdout-rouge-fix.patch'
  patch -p1 < stdout-rouge-fix.patch
}

require_gcc
install_package "ruby-enterprise-1.8.7-2011.12" "http://rubyenterpriseedition.googlecode.com/files/ruby-enterprise-1.8.7-2011.12.tar.gz" stdout_patch ree_installer
install_package "rubygems-1.6.2" "http://production.cf.rubygems.org/rubygems/rubygems-1.6.2.tgz" ruby
```

I won't get deep into the details, but let's start with the last 3 lines.
Ruby-build executes each of these directives in turn. The first of the three
sets up the compiler and the next two each take a series of arguments. The
first two of those arguments are what it's installing and where to get it. Any
after that (you'll have to scroll right) are telling it what to do with it,
once it's downloaded it. It will run the last arguments in the order given. So
that brings us to the function I added at the
top: `build_package_stdout_patch`.

When you pass those ending arguments into <code>install package</code>, it will
look to execute a function by the name you gave it prepended with
`build_package_`. So the function I wrote downloads a patch for a
bug having to do with <a title="Ruby 1.8 - Bug #5108: ruby 1.8.7 fails to build
with glibc 2.14 - Ruby Issue Tracking System"
href="http://bugs.ruby-lang.org/issues/5108" target="_blank">creating rogue references to STDOUT</a>,
then issues the patch command to work it into the
REE source. I don't know a lot about the patch command, so there's a good
chance this could be done more nicely (see below). After that, it runs
ruby-build's standard ree_installer task wich builds and installs the REE
source.

<h2>Running It</h2>

When you're using ruby-build and rbenv together,
you get a nice command called <code>rbenv install</code>. You can either pass
it the name of a recipe that ruby-build already knows about or the path to a
recipe on your hard drive somewhere. So, in the directory where I had saved my
recipe, I ran:

```
$ CONFIGURE_OPTS="--no-tcmalloc" rbenv install ./ree-1.8.7-0211.12-stdout_patch
```

Note: There's some kind of problem with tcmalloc and the newest gcc. Or
something. I don't really understand, but since this is just for local
development, I decided not to worry too much about performance and skip it. If
you set the environment variable <code>CONFIGURE_OPTS</code>, ruby-build will
pass those options along in it's build process. So off it goes to the races.
Then, I get this output:


```
Downloading http://rubyenterpriseedition.googlecode.com/files/ruby-enterprise-1.8.7-2011.12.tar.gz...
Installing ruby-enterprise-1.8.7-2011.12...
--2012-02-03 12:07:35--  http://bugs.ruby-lang.org/attachments/download/1931/stdout-rouge-fix.patch
Resolving bugs.ruby-lang.org... 210.251.121.216
Connecting to bugs.ruby-lang.org|210.251.121.216|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 719 [text/x-patch]
Saving to: `stdout-rouge-fix.patch'

100%[======================================================>] 719         --.-K/s   in 0s

2012-02-03 12:07:36 (68.9 MB/s) - `stdout-rouge-fix.patch' saved [719/719]

can't find file to patch at input line 5
Perhaps you used the wrong -p or --strip option?
The text leading up to this was:
--------------------------
|diff --git a/lib/mkmf.rb b/lib/mkmf.rb
|index c9e738a..7a8004d 100644
|--- a/lib/mkmf.rb
|+++ b/lib/mkmf.rb
--------------------------
File to patch: source/lib/mkmf.rb
patching file source/lib/mkmf.rb
Installed ruby-enterprise-1.8.7-2011.12 to /home/ben/.rbenv/versions/ree-1.8.7-0211.12-stdout_patch
Downloading http://production.cf.rubygems.org/rubygems/rubygems-1.6.2.tgz...
Installing rubygems-1.6.2...
Installed rubygems-1.6.2 to /home/ben/.rbenv/versions/ree-1.8.7-0211.12-stdout_patch
```

Pay particular attention to the last half of this. After the progress bar,
patch gets confused. That's because the patch is really for Ruby 1.8.7 MRI, not
REE. So it complains about not being able to find the file it wants since the
projects have slightly different directory structures. You can see where I
entered the correct path (basically, I just prepended the <code>source</code>
directory). With that information, the patch applies cleanly, it builds REE
without tcmalloc and now I have a ruby called
<code>ree-1.8.7-0211.12-stdout_patch</code>. Hopefully that's helpful to
someone besides me.

---
layout: post
title: My First Ruby Gem
tags:
- code
- heygovote
- programming
- ruby
status: publish
type: post
published: true
meta:
  _edit_last: '2'
---
I just released my first ruby gem, <a href="http://rubygems.org/gems/twitter_alert">twitter_alert</a>. It's intended to be a component of the side project <a href="http://garbled.benhamill.com/2009/09/new-side-project-heygovote/">I mentioned</a> forever ago, HeyGoVote. It grabs all the followers for a Twitter account and DMs them a message. The messages have dates on them because their intended to be schedule ahead of time. I'll crib from the readme near the end, but if you want, you can <a href="http://github.com/BenHamill/twitter_alert">go read the whole thing for yourself</a>. First, I'm going to talk a bit about my experience writing my first gem.
<h4>Jeweler</h4>
I used the <a href="http://github.com/technicalpickles/jeweler">jeweler</a> gem to create a gem template for this guy. It's very simple and was a huge boon considering this was my first time. It scaffolds out the directory structure for you, sets up some handy rake tasks and generally gives you guidance on how to structure your gem. The readme for jeweler is very helpful. I won't bother to restate what it says, but seriously, new to gems or not, you should have a look at jeweler if you haven't already. One of the nicest things is that it'll generate your gemspec file for you and also handle version bumping in a <a href="http://semver.org/">semantic versioning</a> compatible way. It also integrates with git and <a href="http://github.com">GitHub</a> in some interesting ways.
<h4>Gemcutter</h4>
Jeweler will also handle publishing your gem to "gemcutter" for you, which is nice. I say that in quotes because, of course, gemcutter.org is no longer a thing and their stuff all got officially adopted by <a href="http://rubygems.org">rubygems.org</a>, so it actually publishes to there. The jeweler documentation, though, all acts as if gemcutter were still a separate thing. This could be potentially confusing, I guess, but it ends up working right, so it's fine. It does use the gemcutter gem to manage the publishing, so you'll have to have that installed, and make sure you have a rubygems.org account and api key set up. The api key goes in ~/.gem/credentials as you'll learn on your rubgems profile.
<h4>twitter_alert</h4>
I'll crib from my readme example to show the most basic setup:
<pre lang="ruby" line="1">require 'twitter_alert'

account = TwitterAlert::Account.new :user_name => 'benhamill', :password => 'thisisnotmyrealpassword'

class Alert
  include TiwtterAlert::Alert
end

alert = Alert.new 'Very important message.', DateTime.now

account.announce alert</pre>
In the wild, I don't imagine that your Alert class will be so simple. For instance, when I plug this into HeyGoVote, it'll be included in an ActiveRecord model so I can run a cron job that pulls out tweets that should go out today (based on the date) and sends them all.

I haven't plugged this into code yet, and note the version number 0.1. The tests pass, but they may not be comprehensive and I might have botched something up in my publishing, but it looks like everything's working to me. My next step is to start building up HeyGoVote and using twitter_alert in it, which might reveal some needed features. In the meantime, I welcome feedback. Leave a comment here, or <a href="http://github.com/BenHamill/twitter_alert">fork it</a> and issue me a pull request, if you have an idea.

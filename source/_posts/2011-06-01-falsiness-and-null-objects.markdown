---
layout: post
title: Falsiness and Null Objects
tags:
- code
- naval gazing
- programming
- rails
- ruby
- tips
status: publish
type: post
published: true
meta:
  _edit_last: '2'
---
Recently, I went to <a href="http://railsconf.com">RailsConf</a>. I saw a bunch of talks and met some cool people. This is not a RailsConf post-mortem post (if you're interested, though, I've collected some notes from myself and some others <a href="https://github.com/benhamill/railsconf_2011">here</a>). This post is about what, in retrospect, was probably the best talk I went to (and I went to several really awesome talks). I've been mulling over it since I got back, basically, and that seems like a good result from a talk. That talk was <a href="http://avdi.org">Avdi Grimm</a>'s Confident Code (<a href="http://avdi.org/talks/confident-code-railsconf-2011/">slides</a>, <a href="https://github.com/benhamill/railsconf_2011/blob/master/confident_code.txt">my notes</a>).

One thing in particular sort of caught at the edge of my thought patterns: <a href="http://en.wikipedia.org/wiki/Null_Object_pattern">The Null Object Pattern</a>. I don't have a CS degree and so I'm missing a lot of the formal training about design patterns that many programmers have (and, probably, forget), so I'd never heard of it. When Avdi started talking about how ActiveRecord's <code>try</code> method is a code smell, I was like, "Yes!" I would not say that I hate it, but I have seen several times some line of code that looks like this:

<pre lang="ruby" line="1">@user.try(:posts).try(:recent).try(:first)</pre>

I mean... bleh. But I didn't know a way that looked any better to me, really. Anyway, you can look at <a href="https://github.com/benhamill/railsconf_2011/blob/master/confident_code.txt">the notes</a> and <a href="http://avdi.org/talks/confident-code-railsconf-2011/">slides</a> to learn about what Avdi says about the Null Object Pattern. I thought it was awesome and so when I went home, I decided to take it for a test drive.

<h4>The Test Course</h4>
So in a project at work, we have Users and they may or may not have one Subscription. Hopefully, you can picture this complex object graph. Subscriptions may or may not be "current" based on various business rules mostly to do with whether you paid us or not. So, naturally, we have a <code>Subscrption#current?</code> method. But we're using the User as a sort of presenter for Subscriptions. So you don't want to call <code>@user.subscription.current?</code>. That's a code smell. So on User we had this method:

<pre lang="ruby" line="1">def current?
  subscription.try(:current?)
end</pre>

There's that rascal <code>try</code>. "This," I thought, "is a perfect spot for that <code>Maybe</code> method from Avdi's talk." So I rewrote it thusly:

<pre lang="ruby" line="1">def current?
  Maybe(subscription).current?
end</pre>

W00t, right? Wrong. The accompanying <code>NullObject</code> class looks like this:

<pre lang="ruby" line="1">class NullObject
  def method_missing(*args, &block)
    self
  end

  def nil?
    true
  end
end</pre>

That method missing treatment, so handy in avoiding the chain of <code>try</code>s in my first example, is the gotcha. It means that if I have a User without a Subscription for whatever reason, calling <code>User#current?</code> returns an instance of <code>NullObject</code>, which will pass, say, the boolean clause of an <code>if</code> statement.

So, not sure as to whether I'd misunderstood something, was making some dumb mistake or what, I emailed Avdi. He said, basically, "Awesome question. I will answer it in a blog post." And, lo, <a href="http://avdi.org/devblog/2011/05/30/null-objects-and-falsiness/">he did</a>. Go read <a href="http://avdi.org/devblog/2011/05/30/null-objects-and-falsiness/">that post</a> to see <a href="http://avdi.org/devblog/2011/05/30/null-objects-and-falsiness/">what he said</a>. The comments also have some good ideas.

<h4>Noodles</h4>
If you read my comment, I said I was going to noodle on stuff and post again. I had more thoughts than it seemed like would fit in a blog comment. Hence this post. So my initial thought was disappointment. It turned out the Null Object Pattern wasn't as powerful (in Ruby) as I'd hoped, since if you might have something (calling <code>Maybe</code>) the chances that you'll have some conditional asking a boolean business-rule question about it is not low.

So I thought about how to get around that. You could, for instance, make a more complex <code>method_missing</code> definition that grepped the message name for <code>/\?$/</code> and returned false. That's fail, though. It falls down the moment you have something like this:

<pre lang="ruby" line="1">if Maybe(@posts).empty?
  # Intuitively, you'd expect NullObject#empty? to have put you in here.
end</pre>

But then I realized that Avdi was making a higher-level point: since it is not possible to make your own objects look falsey in Ruby, you have to have another solution. Trying to define various question-mark methods on <code>NullObject</code> is trying to untie the knot, but I should be looking for a way to cut it. So it got me thinking: Why the hell to I have Users without Subscriptions, anyway? Shouldn't <code>User#current?</code> express that business logic clearly, rather than just express the logic that enforces it? Yes. Yes, it should.

We have some Users who are also admins, who have special rights. It's also conceivable that we could give away a free account for whatever reason. So, really, we want something like this:

<pre lang="ruby" line="1">def current?
  self.free_account? || subscription.current?
end</pre>

But, this thought it incomplete. It expresses the business logic cleanly: The User is current if they're flagged as free or if their Subscription is up to date. However, if the weird case of a User who is neither free nor has an associated Subscription crops up, we still have to hunt down the "Undefined method 'current?' for nil" error. It sort of has be reaching for <code>Maybe</code> again.

Or maybe (heh, you see what I did there) I want to steal another trick from Avdi's presentation and have <code>User#subscription</code> return <code>:no_subscription_defined_for_user</code> so that the error message makes some more sense. I don't like redefining <code>ActiveRecord</code>'s default accessor methods, though, to transparently return the symbol if the real object is missing.

If you've got any thoughts, I'd love to hear them.

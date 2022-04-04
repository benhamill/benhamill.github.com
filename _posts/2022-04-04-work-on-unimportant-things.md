---
layout: post
title: "Sometimes, something is so unimportant that you should spend a bunch of time on it."
date: "2022-04-04 13:00:00 -0500"
---

_This post was originally written for the internal engineering blog at my
current employer, [Auctane](https://auctane.com/about-us/), and was published on
2022-02-04. It has been edited for clarity to an outside audience._

Often it is a good idea for a group of people working on something together to
focus most on the thing that sets them apart. A business should often focus
effort on the things in their domain. A team shouldn't get distracted dealing
with things outside their area of responsibility, etc. Sometimes, however, there
are things that are outside everyone's domain and everyone needs to pay
attention to them.  Before we get into a tangent about caring for the commons
and, like, climate change or something, let's talk about software teams.

I've never been on an engineering team where I would say our core remit was
"deployment" or "tests". Neither have I ever been on an engineering team that
could entirely ignore those kinds of concerns. What I _have_ seen a lot is
engineering teams that understand those things to be outside their appropriate
realm of focus, but then construe that to mean that those things are unimportant
and so only pay as much attention to them as they have to to make some immediate
pain go away. Over the medium term, this ends up meaning that those things
unexpectedly steal time from them and interrupt things the team _should_ be
focused on.

I think that's a mistake. Every time deployment breaks and you don't know how to
fix it (having to get someone else to do so) or puts production in a weird state
(worse: causes an incident or data inconsistencies) or even just gives you
confusing output or silently fails or… _any_ of that. Dealing with that is time
working on something unplanned. If you always just kick the can down the road,
it's just going to come up again (maybe exploding in someone else's face).

Instead, I think we should try to remember that ownership includes a
responsibility for the dependencies we add to our code whether direct (like an
HTTP library or the service we're calling using it) or indirect (like your
deployment infrastructure or production execution environment). In order to
achieve true quality of service (both for users _and_ owners), we have to
address those responsibilities.


## Switching costs.

One might ask what the big deal is if it takes 10 minutes to fix the build every
so often. We're all familiar with [that one XKCD comic about the cost/benefit
analysis of automating things](https://xkcd.com/1205/) right? And I think many
folks have sort of taken it as shorthand that automating things that save a
little time is _never_ worth it.

On the one hand, the comic is making a more subtle point (if you can save 5
minutes weekly, it makes sense to spend _several_ working days automating a
task). On the other hand, it's just a comic made for laughs. [On the gripping
hand](http://catb.org/jargon/html/O/on-the-gripping-hand.html), it leaves out a
critical time cost: task switching.

Say your deploy process is made mostly of duct tape and bailing wire, and
written by someone else so no one on your team deeply understands it. If you
spend time writing code to fix a bug but then the build breaks when you make
your PR now you have to do a bunch of context switching.

You have to stop thinking about your bug, find and/or load up context about the
build and how it works, figure out how to fix it, then if you've broken any
tests or get any substantive PR comments, you have to load your bug context back
up to make those changes. So the cost wasn't just the time it took to fix the
build itself, but all the context loading and unloading.

This holds for all the little things around our technical artifacts that we
ignore until they cause us pain and then only do enough work to make the pain go
away.


## Confidence in code.

Switching costs are a pretty immediate, tactical cost. I also think there's a
more long term cost we pay being responsible for things that are unreliable in
terms of stress or uncertainty or debugging. Say a service's database goes down
and this causes the service to get into a bad state that will need manual
intervention once the database is healthy again. That's just one more thing to
remember and do when you're already responding to a stressful incident. Spending
the time up front to ensure the service recovers on its own lifts that burden.

If you make a habit of taking care of details like this you end up shortening
the time it takes to find and fix bugs and incidents alike. Especially for
services, which inherently exist in the complex world that is a distributed
system, it can be very useful to have confidence around the sort of "basic"
things like resiliency and communication between services when there is a
problem with the interaction between more than one system, especially if owned
by more than one team.


## In practice.

Just to belabor the point some more, let's take another couple of short examples
of what this sort of thing might look like in practice.

Take a backend service publishing Kafka messages that other backend services
will consume when some business object is persisted in a database. It bears
asking, "what if the Kafka broker is down?". Is it OK to drop messages in that
case? Should you look into a dead-letter queue or similar tool? What guarantees
are you making about the consistency of your database and the messages you
produce? If you are mindful in approaching these sorts of topics you can decide
when to spend the time making it so that you won't have to go hunting down "I
saved this record, but it's not showing up in analytics" sorts of questions at
some unexpected future date.

Or consider a single page application powered by React. It bears asking, "what
if the user is on an iffy network?". Is it OK to have the site fail to work? Do
we need to show the user some concrete notification that things are weird? Are
retries good enough to cover the problem? Can we rely on local storage to store
state until things are more reliable? How valuable is the ability to mask
service outages from our users? If you begin with this kind of question, it can
save you a lot of time later compared to bolting offline support onto an
existing application.

These sorts of questions apply to a wide variety of common situations from the
afore-mentioned CD and CI examples to making synchronous HTTP requests between
services, the shape of our API request and response bodies, accessibility
concerns or dealing with a variety of viewport sizes from users.

The core value, here, is that the things that your systems rely on which are
outside your team's domain, which are "boring" and incidental to the business
domain should actually _be boring_. Strive for them never to page the on-call
engineer, never to be the source of lost or inconsistent data, never frustrate a
user. This may mean spending quite a bit of time on them up front, but the thing
you buy with that is not having to think about them again for a considerable
amount of time.

_Thanks to Dawn Hammond for sharing her front-end perspective on these topics._

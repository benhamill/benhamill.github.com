---
layout: post
title: "Individuals don't own things; teams do."
date: "2021-09-24 13:56:42 -0500"
---

_This post was originally written for the internal engineering blog at my
current employer, [Auctane](https://auctane.com/about-us/), and was published on
2021-07-01. It has been edited for clarity to an outside audience._

In mid-June 2021, one of our engineering leaders shared a link to an article
titled '[There is no "us" in
team](https://www.sicpers.info/2021/06/there-is-no-us-in-team/)'. It links to a
previous, related article by the same author, which I also recommend. They both
address a topic I've thought a lot about over the years: Teams that aren't
teams.


## Let's review.

You should really go read both
[other](https://www.sicpers.info/2021/06/there-is-no-us-in-team/)
[articles](https://www.sicpers.info/2021/03/one-person-per-task/), but in case
you have and it's been a while, let's review the high level assertion: The most
common way we map work-to-be-done onto software teams causes problems. The 1:1
locking of tasks:developers seems like it means all human resources are
maximally utilized at all times, but it creates information and ownership silos
within a team, which means people do skimmy, "LGTM" code reviews, work
priorities get mixed up with capacity, people develop fiefdoms within codebases,
etc. The assertion is that these problems all stem from treating each software
engineer as a lane of work, rather than a member of a holistic team.


## A maxim.

Assuming you're bought in on the above problem, what do we do about it? What
does it look like, in practice, to have solved this problem. For this, I've been
using this maxim:

> Individuals don't own things; teams do.

Maxims, however, are terrible and great. They're great because they're pithy and
can help you remember what they mean, but they're terrible because if you don't
already know what they mean they do nothing to explain it to you. So let's break
it down a bit.

First, let's read "own" as broadly as possible. This doesn't just mean "own" in
the sense of understanding a certain section of the codebase, say, or even the
broader ownership of operating something in production. It's meant to encompass
the _even broader_ ownership of a subdomain of the business. It means ownership
in a technical sense _and_ a business sense (and design, and security, and… it's
all-encompassing). It should be clear how monumental of a task this ownership
is.

So now we can turn to the rest of the maxim. With such a wide scope of
ownership, _of course_ no one individual can own all that. But all of those
things do need to be owned together or else you find things falling through the
cracks and not being taken care of. Which leads to the idea that a group of
people need to own all that together.

The word "together" is doing a lot of work in that previous sentence, so let's
examine that a bit more. If you arrive at a place where Alice owns some parts of
your system, Bob owns other parts and Chiwetel owns yet another part, etc. then
you've not changed anything. This "together" assumes that everyone will be
involved in and expected to know a bit about everything. Ideally this includes
topics like design, security, product goals, etc.

This means that you don't want teams where individuals are focused on what their
next task is, but where the whole team is focused on a single, larger objective
and discusses internally how best to break up the work to meet that objective.
One objective per team at a time also means that the order of work will be based
on dependency between work, not business priority (often a gut check) of wildly
differing tasks.


## What it looks like.

What does it look like when a team is truly working and owning things together?
What behaviors are evident? A lot of us have some similar habits because they're
so endemic in the industry. Here are some of the habits I've had to untrain (or
still struggle to untrain) in myself and what I've tried to replace them with on
teams that have achieved this kind of unity. There may be others. All of them
fall out of two overall goals: **seek shared understanding across the whole
team** and **focus the whole team on a single goal at a time**.

Previously I would focus on the tickets dropped into a sprint: what they meant,
how to do them, how hard they were, who would work them, how important they were
relative to each other. The replacement behavior is to question each item of
work you're presented with (whether tickets or not, whether in a sprint
framework or not). Drive conversations towards the team's current, single goal
and focus work on that goal. Now your work items should largely have a single
business priority because they all serve the same goal. Now everyone's work is
moving in the same direction, so everyone should be invested in what everyone
else is working on.

Another habit I try to avoid is a preference to work on things I'm already
familiar with, whether an area of a codebase or a type of work. Sometimes, we
need to play to our weaknesses. If we don't, we end up creating single points of
failure on every team where only one person is a subject matter expert on any
given part of the codebase. It's OK (even for really experienced engineers!) to
ask for help or to go slower than you're used to. Spreading the knowledge and
understanding across the team is more important for long term velocity than how
quickly this one ticket ships.

It's also very common to focus on shipping the current work item as fast as
possible. Instead, it's important to have a longer-term view of how the work
fits into the overall objective and the way the current objective fits into the
team's holistic ownership story. Slowing down to do something the right way
often saves time and headache later on, which is more important for a team's
productivity (and happiness in my experience). This may include slowing down to
_figure out_ what the right way is and having a team-wide discussion about it
before you can even approach the code. I like two sayings on this topic: "You
can save hours and hours of discussion with just a few short weeks of work," and
"Slow is smooth, and smooth is fast".

It can be tempting to avoid reviewing pull requests for things I don't
understand well or give cursory PRs to things I'm not involved with. If the
whole team is focused on a single goal at a time, you've now minimized the
number of PRs for things you're not involved with. The next part takes a certain
grit: Focus on this code you don't know well and how it changes. Ask questions
before making recommendations. I think "I don't understand what this is doing"
is a reasonable PR comment and an important one for the PRs-as-knowledge-sharing
benefit of PRs.

I try to avoid almost all 1:1 communication, honestly. Instead, more and more, I
prefer to address groups, rather than individuals. Internally, this might mean
that if Chiwetel was the last one to touch a module that I have questions about,
rather than DMing him, I'd ask in our team channel. This way Alice can at least
overhear if not weigh in with an opinion or further questions. Externally, this
means doing things like rotating which team member represents the team in
meetings. Ideally, every member of the team should have the knowledge needed to
report on the team's progress against their current goal (skill in communicating
it is a separate matter, of course).

In general, develop and express an opinion on what the team should be working
on. What things are most important. Ideally, you can build a sense of team
beyond just engineering and involve product folks in discussions of technical
debt that exists or how your deployment pipeline needs to improve. If you listen
to the needs of others and express your own, the team succeeds together
predictably.

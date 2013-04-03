---
layout: post
title: ! 'New Side Project: HeyGoVote'
tags:
- heygovote
- programming
- projects
- rails
- ruby
- twitter
status: publish
type: post
published: true
---
National elections, especially the Presidential elections, get a lot of attention in the US. People talk for months about who might campaign before people even announce their candidacy. There are news stories all over the place covering them. On the other hand, more local elections (for state reps or city councils, etc.) get a lot less coverage (because it's not CNN's job, for instance). Right after this most recent Presidential election, I realized that I hadn't voted at all since the previous Presidential election. In four years, I hadn't cast a ballot, and I thought I should probably become somewhat more involved in local politics. Or, if not involved, at least aware.

One of the buildings adjacent to the one where I work is a polling location. In Texas, that means I can vote there during early voting. A few months ago, I was cutting through that lobby and saw voting booths set up. "Oh!" I thought to myself, "I wonder what we're voting on today." I could have voted right then, but I had no idea what was on the ballot, what the various opinions and angles were, etc. It would have been totally uninformed and random, so I refrained, but I started thinking about how I could be forewarned about elections so I could do my own research.

I could start watching the evening news or following local (or state-level) political blogs or take the local paper. But that's a lot of overhead which I've already decided I don't want to deal with... and just to get one piece of data. So I thought I'd solve my own problem and the way I'd solve it (and solve it for people other than me, who think in a similar way on this issue) was with Twitter.

<h2>The Pitch</h2>
My initial concept was that I'd set up an app that would store election dates and just tweet them. I quickly realized that it's a little more complicated than that. If I just tweet on election day, I've just recreated the oh-what-are-we-voting-on-today problem. So I need to give some warning ahead of time. @<a href="https://twitter.com/jonloyens">JonLoyens</a> pointed out that a tweet can be easily missed, so I should use direct messages. The beauty, here, is that I can build a reminder service and not have to manage who wants reminders: It's all just based on who's following the twitter account.

So the idea is that you'll follow @heygovote and it will direct message all its followers to warn that an election is coming up. Simple enough and if you want to opt out, you unfollow. Easy.

<h2>Three Rules</h2>
I want to keep these three things foremost in my mind while I'm working on this thing:

  * No Bias - I don't want to help people decide how to vote, or influence their vote in any way. This is just about prodding people enough in advance that they can do their own research.
  * No Data - I don't want to know who the users are and I don't want them to have to trust me that I'm not selling information about them to some organization (related to the above, as well). So I don't want to have to know anything about the user other than their Twitter user name and I don't even plan on storing that, just asking Twitter who's following @heygovote.
  * No Bother - I don't want to hassle people. I want to remind people, not badger them. I also don't want to have to mess with the thing myself to keep it running; it should be fire-and-forget.

<h2>Trouble Scoping</h2>
It pretty quickly became evident that I needed to decide who my target was. I'd intended to target "Austin", but that doesn't really scale gracefully to the county, state and national levels. After talking to my brother, who works on campaigns and the like, I've decided I'm going to target Travis County. That catches most of Austin (more to the point, it catches where I live, since this is solving my problem) and also scales up nicely.

For voting purposes, Texas breaks counties up into voting precincts. All the election dates for all the precincts in a given county are the same, so one tweet (or, rather, direct message) will apply to everyone in Travis County. If, beyond comprehension, this becomes wildly popular and other places want HeyGoVote to cover them, then I'll deal with that as it occurs. My guess is that how elections are handled will be different enough from state to state that it would mean rebuilding the date-getting machinery for each, uh, constituency, as it were.

<h2>Where to Start</h2>
I haven't started coding on anything yet; all work to date has been design thinking and research on how I can get a hold of the information I need. I've sent some emails back and forth with the Travis County Tax Office (which decides election dates, oddly enough), who've been helpful. They don't seem to have a handy RSS feed of election dates that I can poll, so I'm still working out that side of things.

I will probably start on the reminder side of the application. If I design the DB schema intelligently, it can be very loosely coupled with the data gathering bit. Depending on how it goes, it also seems like the kind of thing that might make a useful Rails plug-in, too. So I might release that on it's own.

That being said, I expect to use Rails as the tool set. That might seem like overkill since nothing I've described has needed a web interface to it, but I have the idea that, after I get the reminder working, it might be nice to build a tool or two that would help people figure out where their polling location is (for folks who skip early voting), what's on the ballot for their precinct and things like that. If I can't find existing, non-partisan tools to link to for this, I might have to build my own.

Expect to hear more about this as I work on it. If you have any suggestions or questions, leave a comment.

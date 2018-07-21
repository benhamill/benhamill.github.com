---
layout: post
title: "The Slack Users' Code"
date: 2015-11-14 00:27:00 -0500
---
The bigger a [Slack](https://slack.com/) team gets, the more chaotic it can get.
These are my ideas about what makes a good Slack team and Slack citizen. These
are for _work Slack teams_. If you have a social Slack team set up, _you do you_
because the expectations, goals and social forces are likely to be very
different. Some of these may apply, but I haven't done as much thinking about
which ones.


## Yourself

Whether you work in a widely distributed business unit or just a very large
office, from time to time you'll find yourself interacting with people you don't
see in person much. These are people who do not know you by sight. These four
guidelines are to help those people have a chance of recognizing you from
 #watercooler when they meet you in person.

* You should set your first and last name in your
  [Slack Profile](https://my.slack.com/account/profile). If your name is
  "James", but everyone calls you "Jim", it's OK to put "Jim", but otherwise,
  you should put it as if you're turning in homework. Don't try to get cute.
* If your full name is First Middle Last, your user name should be firstlast,
  flast, fmlast or something similar. Change it in
  [the username settings](https://my.slack.com/account/settings#username).
* Your avatar should be your face. Ideally, it should be big enough that it
  takes up most of the avatar space. It should be easily recognizable at a small
  size.
* Put your job title _and your team name_ in the "What I Do" section of
  [your Slack Profile](https://my.slack.com/account/profile). For example, don't
  put "teh codes", but instead, "Software Engineer, Product X". If you're an
  administrator, consider using [Slack's custom profiles](http://slackhq.com/post/132116511325/customprofiles)
  to make a special space for this information.


## Channels

Channels, especially, are important to name in a consistent and predictable way.
You want channels to be discoverable and obvious so that people have a good
chance of finding the right channel for a question or discussion.


### General Guidelines

#### Rename the #general Channel

This one's just for administrators, really. Consider renaming #general to
something else. It's a special channel that everyone _must_ be in. No one can
`/leave` it. It's important for people to understand that talking at all in
 #general can be bothersome to a lot of folks, let alone using @here or @channel.
Renaming it to #everyone or #all-hands or something might make that more clear.
In a large enough organization, locking down this channel so that only
administrators can talk in it (and rarely at that) is a very, very good idea.

Corollary: "General" isn't a very descriptive word. You don't need to use it in
channel names. #product-general isn't really any more descriptive than #product.


#### Don't Create Private Channels

You might think no one else cares. If they don't care, they don't have to join
your channel. Private channels silo communication and stymie discoverability.
By default, you should make all channels public. If you have something truly
secret to discuss—something that even other employees of your company shouldn't
see—consider that you're technically sharing anything you type into Slack with
another company (Slack itself). If it's truly secret, consider sending PGP
signed emails or only talking face-to-face. Much of what you do probably isn't
actually that secret.  Flowdock has [a great post about the dangers of private channels](http://blog.flowdock.com/2014/04/30/beware-of-private-conversations/).
Also see [Zach Holman's fine writing about opt-in transparency](http://zachholman.com/posts/opt-in-transparency/).

It's not that creating a private channel is _never_ a good idea. It's that if
you're making a private channel, you should have a damn good reason.


#### Don't Create Extraneous Channels

You might think you're bothering people in another channel. For some kinds of
topics (see below) it might be appropriate to create a new channel, but in
general, you should wait until some sub-topic is routinely taking over a more
general channel before you create a new one. You don't want to get into a
situation where there are more channels than people in your Slack team.
Specifically, a team should not create a new channel for every individual
problem they want to discuss. Keep things in as general a channel as folks can
stand.


#### Use Dashes in Channel Names

Just for consistency. Do #austin-lunch-games not #austin_lunch_games or any
other ridiculousness.


#### Know What Kind of Channel You're Creating

There are two broad categories of channel: Work-Related and Off-Topic.
Work-Related channels are the ones that some team is expected to actively
monitor. Off-Topic channels are everything else.


#### Use @here and @channel Appropriately

There are two @mention aliases that notify a lot of people (let's just forget
that @everyone even exists). One, @channel, will notify everyone in the channel
where you use it. The other, @here, will notify only those people in the channel
who are online when you use it. _Both_ should be used sparingly, and @channel
more sparingly than @here. For example, you don't need to use either one to let
people know you're going to lunch. Either @mention someone specific who needs
that information, or just say it in the channel and trust people who need it to
pick it up.

This is especially important in managing communication with your immediate team.
Your team's channel may have a lot of guests lurking and listening. If you use
either @here or @channel to ensure the 5 other members of your team get a ping,
you'll also be pinging all your guests. If typing out your teammates' handles is
too onerous, you might consider setting up a [User Group](http://slackhq.com/post/132108708810/usergroups)
for your team.


#### Minimize Use of Bots & Other Toys

Programmers, especially, can become enamoured of chat bots and chat
integrations.  As a programmer, I feel this too. However, you have to carefully
balance this with the costs it can impose on real human communication. Slack is
a tool primarily for humans to communicate with other humans. If computers are
being too chatty in a channel, the humans will have a hard time getting a word
in edgewise. And a chat history tends to be a pretty poor radiator of
information.  Rather than having engineers initiate and control a deploy from a
channel, consider having a different tool that does all that, and have only
sparse or summary updates go to the team channel.

As for other toys, let us consider the cautionary tale of `/giphy`. The images
that [Giphy](http://giphy.com/) serves up in response to any given query are so
infrequently relevant to the query that it's a remarkable event when it happens.
I mean that literally: I will remark in Slack "Holy shit. Giphy didn't fail
miserably!" on the rare occasion.

Some people feel that a single gif has a damaging effect on conversation. I
_like_ a good animated gif, but people repeatedly typing `/giphy` over and over
at each other hoping for a relevant animation does not constitute human
interaction. This same principle applies to almost any fun integration or
@slackbot auto-response. They're very easy to abuse in a way that you don't
realize you're abusing them. No one should have to ask their coworkers to stop
playing a loud game of tag in the hallway next to the bullpen.


#### Archive Unused Channels

If a channel is no longer needed, a Slack administrator (at any time) or the
channel owner (if the channel is empty) should be able to archive it. Get rid of
that dead weight.


### Work-Related Guidelines

#### Name it After the Hosting Team

The channel structure should loosely mirror your company's org chart. Each
channel should be hosted by a manager and their team. These channels should be
named after the team. The Support team's channel should be #support and the
API sub-team within should be #support-api (thus ordered so they sort together
in the channel list).

These channels are _hosted_ by a team and belong to that team, but they should
still be public. If someone has a question about something owned by a team or
for a member of that team, this channel is the first place they should go.

Anyone is allowed to be in any channel, but members of a team are expected to
be actively in their team's channel most of the time. In fact, if each manager
hosts a Slack channel, everyone who reports up through them, even indirectly,
should probably at least passively monitor every channel up the chain of
command from them (imagine, e.g. a place where a VP level person can say
"The code for the all-hands dial-in is 9466." and reach the entire relevant
audience).

Caveat: While you're allowed to be in any channel, be aware of whose dinner
party you're attending. If you're not on the hosting team, be especially
vigilant for when your input is actively welcome and when it might be
counter-productive. For a good example, see [Holman's discussion of HR policy changes](http://zachholman.com/posts/opt-in-transparency/).


#### Name it After the Office

There's a much less common type of Work-Related channel, which is
location-based. These channels should be hosted by the head office manager for
an office and the people who work in the office should at least passively
monitor these channels. They are places mostly for the office manager to announce
things or to ask questions about the office. They are not Off-Topic channels,
but they are probably more relaxed than other Work-Related channels. They also
serve an ambassadorial purpose for people visiting the office from another
office.

These can actually be thought of as team channels (for the team of office
managers, even if there's just one), but they deserve to be called out because a
potentially large number of people would be regularly in them.  These channels
should be named descriptively. If you have offices that happen to have cool
names, you could get cute and name them after the office building/complex that
the host office manager is in charge of: #seaholm, #tide-point, etc. Elsewise,
you should use the city name: #austin, #baltimore, etc. The latter is probably
better for new folks, but weigh that against how often the offices are called by
the other name.


#### Set the Topic and Purpose

So that people know what the point of a channel is. Especially if the channel
seems very similar to another channel. For more general channels where the title
really is very clear, emoji can be a fun way to still have a topic without it
feeling super redundant.


#### Stay on Topic

Try to stay on topic. The fewer people in a channel, the less important this is,
but still. If people are off topic and annoying you, you can tell them to
":goto: #watercooler" (or whatever appropriate channel, and you should totally
[alias the :arrow_right: emoji as :goto:](https://my.slack.com/customize/emoji)).


#### Invite People to Your Channel

If the #some-product engineers are having a problem with deployment and they
think it might be a problem that the #infrastructure folks could help solve,
here's how I recommend they should handle it: Once they make that determination
(I assume they've been talking about the problem in #some-product for a while,
now), they should elect a single one of them to pop into #infrastructure.

That person should see if anyone else has already brought the problem up and, if
so, follow that conversation (possibly to another channel, as we'll see
shortly). If not, then they should say something like, "@infrateamlead: We're
having a problem with deployment over in #some-product. Does anyone have time to
come over and help us work it out?"

You ask the team lead because, as with any other task you want to hand to
another team, you don't jump the chain of command: you let the leader decide
who's interruptible or appropriate for the task. You mention what channel you've
been talking about the problem in so that the nominated helper knows where to
go, _as well as anyone else who wanders into #infrastructure with the same
problem_.

Then whoever it is from #infrastructure that gets assigned to help can join
 #some-product and see all the history of the discussion about the problem and
get caught up. And then, you know, hopefully help out in fixing the problem.
This protocol or something like it is especially important for heavily
interrupt-driven teams like Ops, IT, Internal Tools, etc.


### Off-Topic Guidelines

#### Name it After the Topic

Off-Topic channels still have topics, they're just not work-required topics.
This can be something very general like #games or very specific like
 #long-distance-trail-running (but remember: Don't Create Extraneous Channels).
Try to keep in mind the geographically distributed nature of your organization.
If you're making a channel to find people to play cards with at lunch, maybe put
the office or city name in the channel title, as well.


#### Name it After the Role

There is a special kind of off-topic channel that bears highlighting. You
probably want to have a channel for Software Engineers to hang out and talk
about their jobs. Or Engineering Managers or whatever. These are related to the
work that people are doing, but they're probably not required to be in the
channel in order to get their job done.

These are hubs for people to do frequent, low level professional development.
They should be grouped around _roles_ not _titles_. This is partly so that they
can span titles, which don't always map 1:1 with a role. Also, anyone who has an
interest or stake in the topic should feel welcome to join in the discussion.
Whether you call it #software-engineering or #programming is a bit of trivia
that each role-community can hash out for themselves, honestly.


### Outliers

> The code is more what you'd call "guidelines" than actual _rules_.<br>
—[Captain Hector Barbossa](https://en.wikipedia.org/wiki/Hector_Barbossa)

There will always be weird outliers. A cross-team task force will assemble and
need a channel to talk about their work in. Temporarily, a small group of people
will want to have a place to organize while at a conference. These kinds of
things are perfectly OK, as long as the bulk of the channels you have adhere to
the guidelines. And as soon as these outliers are no longer needed, you should
get rid of them. If a channel persists for a long time and doesn't fit within
the guidelines, that's an organizational smell (or a guideline smell and if you
think it is, [hit me up on Twitter](https://twitter.com/benhamill)) that might
need addressing.

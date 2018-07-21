---
layout: post
title: 'My Twitter Project: atreply'
---
I use <a href="http://twitterfox.net/">Twitterfox</a> to read and create tweets most of the time. I follow enough people that, when I open my browser for the first time for the day, more than 20 tweets have accumulated and, really, I don't want to go back and read all 60-odd or whatever that have accumulated overnight. Twenty, I should note, is just what Twitterfox picks up when it first turns on.

Occasionally, I'll come in and see the last few tweets in a conversation between two people I'm following (I only see @replies by others who are to people I'm also following). If it seems interesting enough, I'll go back and page through to see what they were talking about, reading in reverse order. Sort of like reading a chat log written by the guys that made <a href="http://www.imdb.com/title/tt0209144/">Memento</a>. It's not horrible, but neither is it ideal.

So I had an idea about it and I've started work. Twitter tracks what tweet (technically called a "Twitter status", apparently) any given tweet was a reply to. And, I figured, it would be relatively simple to, given a Twitter status ID, recursively follow the reply chain back and get the whole conversation. Turns out, I was right.

A proof of concept:

``` ruby
require 'rubygems'
require 'twitter'

class Reply
 attr_accessor :text, :author, :in_reply_to, :time, :atreply

 def initialize status_id
   status = Twitter::Client.new.status :get, status_id

   self.text = status.text
   self.author = if status.user.name then status.user.name else status.user.screen_name end
   self.time = status.created_at
   self.in_reply_to = status.in_reply_to_status_id
   self.atreply = Reply.new self.in_reply_to unless self.in_reply_to.nil?
 end

 def each_reply &amp;block
   reply_chain.each do |reply|
     yield reply
   end
 end

 def to_s
   self.author + ' - ' + self.time.to_s + "\n" + self.text
 end

 protected

 def reply_chain
   return [self] unless self.atreply

   self.atreply.reply_chain &lt;&lt; self
 end
end
```

This has a dependency on <a href="http://github.com/joshuamiller/twitter4r/tree/master">Joshuamiller's version of twitter4r</a>. My medium-term plan is to make a one-trick-website that will take an ID or twitter URL and give you the replies all pretty-like. Maybe make a bookmarklet for convenience's sake. I plan on using Rails, even though that's overkill because I figure it'll be a good learning experience on that front. Find it on <a href="http://github.com/BenHamill/atreply/tree/master">Github</a>.

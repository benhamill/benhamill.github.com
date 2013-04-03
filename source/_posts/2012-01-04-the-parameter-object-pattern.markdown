---
layout: post
title: The Parameter Object Pattern
tags:
- otherinbox
- programming
- ruby
- tips
status: publish
type: post
published: true
---
I have been paying more attention to software design patterns recently. It all started with my noticing the <a title="Falsiness and Null Objects" href="http://garbled.benhamill.com/2011/06/falsiness-and-null-objects/">Null Object pattern</a> and the <a title="Ruby Rogues podcast" href="http://rubyrogues.com/" target="_blank">Ruby Rogues</a> talking about <a title="Amazon Link" href="www.amazon.com/Smalltalk-Best-Practice-Patterns-Kent/dp/013476904X/" target="_blank">Smalltalk Best Practice Patterns</a>. My most recent discovery is of the <a title="Parameter Object pattern @ WikiWiki" href="http://c2.com/cgi/wiki?ParameterObject" target="_blank">Parameter Object pattern</a>. Thanks, by the way, to @<a title="Tim's Twitter page" href="http://twitter.com/timtyrrell" target="_blank">timtyrrell</a> for telling me the name of it.

As I've <a title="A Smarter has_many :through?" href="http://garbled.benhamill.com/2011/08/a-smarter-has_many-through/">mentioned before</a>, I've been spending some time <a title="OtherInbox" href="http://otherinbox.com/" target="_blank">at work</a> with a search engine. It works sort of like NewEgg's search in that you might type a string in ("desktop ram") and then narrow your search by clicking on links in a side bar (DDR3 or Corsair). The search engine in question powers a product called <a title="SubjectLin.es" href="http://subjectlin.es" target="_blank">SubjectLin.es</a> (it's in beta; you can sign up for a free account to try it out, if you like) where you can search for words that appear in an email subject and then further limit by, say, who sent it ("sender"), what kind of email it is (like coupon or receipt, we call it a "tag") or what business the sender was in ("market").

To date, I've rewritten the search engine a few times and refactored it a few times. It and one other area of the code base are, by far, the most complex bits of the application, so it has gotten it's fair share of attention. And it's gotten more recently when an exception brought our attention to something: We don't have one search engine... we have <em>four</em>. It's just that they're all in one class. Finding the email subjects that a user is interested in is not really related to finding the senders who sent them except that the user's parameters are the same. So I thought what we'd do is factor out our current search object into 4 smaller ones (subject, sender, tag and market) that each centers around the object it's pulling out of the DB. Besides code clarity, there are some other benefits we get from this modularity, but I don't want to get into that in this post.

So now we're faced with the fact that we'll have 4 objects we want to create that all depend on the same parameters to build themselves. Add to that the fact that we do some fancy Google-style keyword parsing (you can type "some key words sender:Company" and we pull out the company name) and the fact that if the user is interested in limiting by sender, we want to display things differently than if they aren't. Now, I thought, it looked as if we should have an object just about the user's search parameters and that each of our search engines should take only that as an argument.

This way, our view (or presenter... whatever) can ask the search parameter object if the user limited based on sender or not. We can build the one object and then pass it to each of the four engines. We can put all the keyword parsing logic into the search parameter object, too. When I had it, it seemed like such a good idea that it had to have been thought of before. And, lo, it is called the Parameter Object pattern. Or, anyway, I think this is a case of that pattern.

What do you think? Is this Parameter Object? Is it something else? Does my example make any sense at all (I honestly have no idea, since I'm so deep in the system; I tried to balance high-level with need-to-know)? Note: as of this writing, none of this is implemented yet. This is all just design work with @<a title="Macors's Twitter page" href="http://twitter.com/marcosacosta" target="_blank">marcosacosta</a>, so far. If anything interesting crops up while we're writing it, I'll try to remember to update this post.

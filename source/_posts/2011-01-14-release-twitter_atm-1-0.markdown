---
layout: post
title: ! 'Release: twitter_atm 1.0'
tags:
- code
- otherinbox
- programming
- projects
- ruby
status: publish
type: post
published: true
meta:
  _edit_last: '2'
---
Holy crap. Did I really start this blog in December 2008? That would make it more than 2 years old. Wow. Well, good on me, I guess. That feels sort of absurd. Anyway...
<h4>An Itch...</h4>
The other day at <a title="OtherInbox" href="http://www.otherinbox.com" target="_blank">work</a>, I had to get the OAuth credentials for a twitter account that our application would use to send programmatic tweets to. For those of you not familiar with OAuth, a brief description: The usual way you OAuth with Twitter is that you have a web page where a user clicks something indicating they'd like to OAuth you to their account. You then send your consumer key and secret off to Twitter to get a request token and, using that, you send the user off to a url over on twitter.com.

Once there, they sign in (or are already signed in) and click "Allow". Twitter then hits your callback url with some more tokens, which you use to make a final reply and then they respond with the access token and secret you'll need to do whatever it is you're doing with the user's account. If the user changes username or password, you're still authorized and if they want to revoke your access, they can without changing their username or password<em>. </em>Great! (If you're thinking, "What?!? Not great! That made no sense!" then maybe the image in <a title="OAuth Documentation on dev.twitter.com" href="http://dev.twitter.com/pages/auth#intro" target="_blank">this article</a> will help).

However, when your program is a desktop client or when you've only got one account you'll ever be tweeting from (or maybe a small handful), it's not really practical to build a web interface and a callback url to hit so that you can do the whole dance and get the tokens. So Twitter has an alternate path that replaces the redirects in the middle of the dance. Instead of redirecting users over to twitter.com, you show them the URL and they go there manually. When they click "Allow", they're given a PIN, which they then give back to you and you can then finish off the dance as if the PIN were the callback.
<h4>...Scratched</h4>
So at work, I hacked around in the console for a bit and eventually figured out how to work the PIN-based method, ran it for the account I wanted and then got the access token and secret for our account. But, I thought, I shouldn't have to hunt all around to figure out how it works (the documentation is almost all focused on the callback path). Heck, if I know my consumer key and secret and I own the account, I shouldn't need to know how it works at all. So, having figured out how it works already, I decided I'd write a little command to do it for me and publish it as a gem. Thus was born <a title="Github repo" href="https://github.com/benhamill/twitter_atm" target="_blank">twitter_atm</a>.
<h4>The Tool</h4>
As the README states, it's pretty simple. You invoke <code>twitter_atm get_creds</code> with your consumer key and secret as arguments, then it interactively gives you directions on how to finish out the process and spits out the access token and secret at the end. I do want to note, about the name, it's not about cash. It's about inputting a PIN and getting something in return. It's not very exciting to use, so I won't talk about it much. I'd rather move on to...
<h4>An Old God of Asgard</h4>
This was the first time I'd written a program with a command line interface, so I asked around a little about gems that were good at that and Jonathan Otto (of <a title="Dealzon main site." href="http://dealzon.com" target="_blank">Dealzon</a>) pointed me at <a title="thor gem on Github" href="https://github.com/wycats/thor" target="_blank">thor</a>. In short, thor seems awesome. It's got a nice DSL for describing the various subcommands of your application and it looks deep enough to handle something more complex than my purposes with twitter_atm.

As a brief example, consider <code>git pull --rebase origin master</code>. If you were writing something that would support this syntax in thor, it would look something like this (I made up the git commands inside off the top of my head, so it's a bit naive):

<pre lang="ruby" line="1">class Git < Thor
  desc "git pull", "Fetches and merges stuff into the current branch."
  method_options :rebase => :boolean
  def pull(remote, branch)
    `git fetch #{remote}`

    if options[:rebase]
      `git rebase #{remote}/#{branch}`
    else
      `git merge #{remote}/#{branch}`
    end
  end
end</pre>

You can also declare different types of options, default values, etc. Have a look at the fairly extensive readme and look at <a title="/bin/twitter_atm" href="https://github.com/benhamill/twitter_atm/blob/develop/bin/twitter_atm" target="_blank">how I used it in twitter_atm</a> if that helps. I quite recommend the gem and already have another project I'd like to use it on.
<h4>bundle gem twitter_atm</h4>
There's this little project--I don't know if you've heard of it--called <a title="Bundler website." href="http://gembundler.com/" target="_blank">bundler</a>. It was started by a <a title="Carl Lerche on Github" href="https://github.com/carllerche" target="_blank">couple of up-and-coming young programmers</a> <a title="Yehuda Katz on Github" href="https://github.com/wycats" target="_blank">who really might go somewhere some day</a>. Bundler is great for managing gems in a big project and it does this really impressive dependency resolution thing. But there's a lesser known command that I've fallen in love with: <code>bundle gem &lt;gem_name&gt;</code>. It just makes a skeleton for a gem project for you. Unlike <a title="Jeweler on Github" href="https://github.com/technicalpickles/jeweler" target="_blank">Jeweler</a>, it only gives you the bare minimum and really just gets out of your way. You manage your own version number and <a title="Yehuda on using .gemspec files &quot;correctly&quot;." href="http://yehudakatz.com/2010/04/02/using-gemspecs-as-intended/" target="_blank">write your own gemspec</a> (gasp!).

It has three handy rake tasks with obvious functions: <code>rake build</code>, <code>rake install</code> and <code>rake release</code>. Each of those for the most part just issue various <code>gem</code> commands. I basically like it because it builds you a little foundation and then doesn't really manage anything else for you. One thing that's important to note: The current version of bundler doesn't add <code>Gemfile.lock</code> to the <code>.gitignore</code> that it generates (but future versions will), and it is important that you do so. Yehuda has a <a title="Yehuda on Gemfile and .gemspec" href="http://yehudakatz.com/2010/12/16/clarifying-the-roles-of-the-gemspec-and-gemfile/" target="_blank">blog post</a> explaining why.

So, based on this experience, here's what <em>I</em> took away: Thor is good to use for making a CLI, <code>bundle gem</code> is good to use for making a gem and sometimes you can make something small and cool for yourself in one sitting which gives you a good feeling and is a well invested 5 hours.

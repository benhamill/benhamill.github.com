---
layout: post
title: Sudo on Windows XP... Sort of
---
I'm mostly posting this to document it to myself, since I always forget, but it might be helpful to any readers, as well. You generally don't want to be logging in to your computer as a user with admin privileges and a sane OS (like Mac OS or Linux), makes it a fairly painless experience. Windows, on the other hand, can be a real bear on the point. Specifically, Windows Explorer doesn't like to do the whole "Run as..." thing. I've discovered a wonderful little run line that will solve this problem.

In the run line (Windows key, then R or click Start menu then Run...) put in <code>runas /u:administrator "explorer.exe /separate"</code>. You'll want to replace <code>administrator</code> with an appropriate user name if that's not a valid one. A DOS prompt will appear asking for the password and away you go. This tip thanks to <a href="http://stackoverflow.com/questions/13805/opening-explorer-shell-with-admin-priveleges-on-xp-with-ie7-installed">Stack Overflow</a>.

I also found something else handy <a href="http://pcwizkid.blogspot.com/2008/03/create-shorcuts-of-hidden-commands-in.html">here</a>: You can input run-line commands into the "location" prompt when creating a shortcut from scratch in Windows. So if you don't want to type out all that /u:administrator stuff all day (or, well, probably not that frequently and you'd forget it), then you can right-click &gt; New &gt; shortcut and paste your command into the location. Call it whatever you want and then you've got a shortcut to an admin Windows Explorer right on your desktop.

I find this so much easier to deal with than any other solution when I need to muck with file permissions or any number of things in Windows. Helpful? Didn't work for you? Thoughts on the site design? Let me know in the comments.

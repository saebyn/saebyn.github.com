--- 
layout: post
title: Python, C++, and subversion oh my!
wordpress_id: 4
wordpress_url: http://johnweaver.zxdevelopment.com/2006/10/27/python-c-and-subversion-oh-my/
date: 2006-10-27 19:59:24 -07:00
---
I woke up this morning much earlier than I would have liked, but of course I checked my email and so forth. First bug of the day, the ftplib module in Python had been giving me problems for the past two days, and I was disappointed to hear that a small script that I had written that used it was still being difficult. It turns out that (and of course I should have expected this) the FTP class raises an EOFError exception when the server drops the connection. Of course, I can't keep the server from doing this, but I can at least get the script to behave properly. With a quick fix to catch the exception and retry that server later, I went back to sleep.

I woke up, a couple of hours later, feeling much more alert. I'm working on a C++ port of a 2d multiplayer overhead game, that was originally written in VB6. It turned out to be a much larger project than I thought it would be, but I still started chugging along on it again any ways. I was getting little pieces here and there done, but still much to do.

It's been about a year since I last setup a subversion repository, since I haven't worked on too many large projects recently. The last one I setup, I did for a small 3d game engine I was working on as a hobby project. A quick 'yum install subversion' on the chosen server, followed by adding a 'svn' user and a place for the repository, and away I went. I had to check the documentation a couple of times to be sure about things (An important lesson I learned along time ago about unix: If you are ever unsure about what a command or option does, check the manual!).  Overall, it was a decently productive day, interspersed with many IMs asking about VB6 (I don't mind answering questions about pretty much anything I know something about, as many of my clients eventually discover).

---
layout: post
title: Shadowbox and javascript controlled links
date: 2008-10-28 20:32:33 -07:00
---

Recently I have started using <a href="http://mjijackson.com">Michael J. I. Jackson</a><a href="http://mjijackson.com">'s</a> <a href="http://mjijackson.com/shadowbox/">Shadowbox.js</a> on some of my client's sites. On site in particular had an image gallery where you could click on a thumbnail to view a larger version of the image, and some had associated videos as well.

The goal there was to have a single link that would be updated to point to the correct flash video depending on the thumbnail being clicked (the link being hidden if no video was associated with that thumbnail). After a few frustrated minutes, I discovered that Shadowbox.setup() needs to be called after changing the link's href attribute so that the link will trigger the correct video. Since the initially the link didn't point to a video at all, I also passed {skipSetup: true} to Shadowbox.init().

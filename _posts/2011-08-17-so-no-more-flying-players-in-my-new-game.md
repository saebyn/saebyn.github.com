--- 
layout: post
title: So no more flying players in my new game
date: 2011-08-17
tags: game-development
---

I finally realized I could simply track when the player hit a horizontal
surface and set a 'onGround' flag, and then turn that off when you jump. then
each jump attempt checks the flag, if it's not set it won't let you jump.
Obviously, I tried a much more complicated method first. So no more flying
players in my new game! (at least unless I add jet pack or something)


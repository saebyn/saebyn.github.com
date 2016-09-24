--- 
layout: post
title: Recent Projects
date: 2011-10-04
tags: django twilio github
alias: [/post/11024404628/recent-projects]
---

I have found myself writing about my most recent projects (as of October 2011) and I thought I'd put all of this in one place. Here it goes:

I have about six years of experience doing web application development, and
have been programming since 1996 (first as a hobbyist, then professionally),
primarily in C++, Python, and Javascript.

I've been developing projects using Django since the 1.0 release (summer of
2008). My most recent Django projects:

  * A phone number verification system using Twilio, which generates and speaks a PIN to the end user over the phone. It was developed with Django 1.3 using the django_twilio app.
  * A data importing system, which matched existing records fetched via an external API, allowed the user to manually select the best match, edit the generated record, and review records to be imported. This project was also developed using Django 1.3 and jQuery for a simple auto-completed search function. I also used Celery (and the django-celery app) to deal with the task of processing the uploaded data in the background.
  * I am currently between iterations on [http://mathisasport.com/](http://mathisasport.com/), which is also developed with Django and jQuery. Most recently, I have developed several statistics gathering methods for the model managers in the app, using custom SQL (this site is using MySQL) as needed to allow the records returned in QuerySets to be sortable by the results.
  * A series of HTTP-based web services for an unreleased web service API. The API calls return results in JSON and were developed using Django's class-based views.
  * My most recent personal Django project is at [http://fanonic.net/](http://fanonic.net/). It's still in development, but somewhat usable at this stage, although there isn't much of any content there yet.

I also have lots of examples of my work open sourced at [http://github.com/saebyn/](http://github.com/saebyn/). I've recently pushed out some updates for the one Django project I have there, django-classifieds, in the django-1.3 branch.

The largest example of my HTML, CSS, and Javascript skills is at <del>[http://familysnap.saebyn.info](http://familysnap.saebyn.info)</del> (the original site was taken offline several years ago). I was responsible for about 60% of the front-end for that site, and a considerable amount of the backend as well (which is PHP, so I won't go into more detail about that).

You can find more about me on my [About page](http://saebyn.info/about/) and in my [online resume](http://saebyn.info/resume/).


Update (July 27th, 2012): My mirror of FamilySnap is no longer online.

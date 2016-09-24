--- 
layout: post
title: View Parameters in Reusable Django Apps
date: 2011-05-12
tags: django
alias: [/post/5424537791/view-parameters-in-reusable-django-apps]
---

I've read through [Eric Holscher's blog post about Reusable App Conventions in
Django](http://ericholscher.com/projects/django-conventions/app/) a few times,
and it got me thinking about problems I have had in the past with other
developer's apps.

When you write your views for a Django application you intend to release,
unless every view has exactly the same keyword arguments, please make sure to
add a **kwargs parameter to your views. This lets your users pass arguments to
your views with the include() in their URLconf, without getting errors from
the views that don't have that parameter listed. On my recent fan-fiction web
site, I had to work around this problem and it's really annoying to have to
make a local copy and edit it for what could be really trivial to address by
the reusable app creator.


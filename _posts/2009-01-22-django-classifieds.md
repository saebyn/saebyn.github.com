--- 
layout: post
title: Django-Classifieds
wordpress_id: 32
wordpress_url: http://johnweaver.zxdevelopment.com/?p=32
date: 2009-01-22 12:08:54 -08:00
---

Back in July, my company was contracted to work on a new classified ad site
related to the golf industry. A few weeks prior to this we had decided to
take the plunge and start using <a href="http://djangoproject.com/">Django</a> on projects whenever possible. After a
few emails back-and-forth about this project, I knew that Django would make a
good base for building it.

The key feature requested for the original site was to be able to support
several categories of postings, each with their own distinct fields. I used a
'Field' model to represent the different field types available for each ad
category. A 'FieldValue' model was used to store the value of a specific field,
and associate it with the 'Field' and 'Ad' via foreign keys.

Additional features include searching, attaching images to ads, PayPal checkout,
configurable pricing options, and custom templates for listing, viewing, and
editing each category of ad posting.

<del>You can see a <a href="http://django-classifieds.zxdevelopment.com">demo version of this software here</a>.</div>

I can release this as an open source application, so that others can use
it to build their own classified ad sites, but before I do I need to clean up
the code a bit more and get more documentation in place.

Edit: Please see the <a href="http://saebyn.info/2009/02/28/django-classified-ads/">follow up post</a>.

---
layout: post
title: "Fanonic Update"
date: 2012-07-26 13:28
comments: true
tags:
- django
- puppet
- celery
- fanonic
---

I'm an avid fan fiction reader, and one of my on-going projects is <a href="http://fanonic.net/">Fanonic.net - a fan fiction hosting site</a> that provides helpful features for readers to find the kind of stories they are looking for more easily. If you haven't heard of the term or come across fan fiction before, here's the <em>Wikipedia</em> defintition from the <a href="http://en.wikipedia.org/wiki/Fan_fiction">fan fiction</a> page:

> <em>Fan fiction</em> (alternatively referred to as <em>fanfiction</em>, <em>fanfic</em>, <em>FF</em>, or <em>fic</em>) is a broadly-defined term for fan labor regarding stories about characters or settings written by fans of the original work, rather than by the original creator. Works of fan fiction are rarely commissioned or authorized by the original work's owner, creator, or publisher; also, they are almost never professionally published. Because of this, many fan fictions written often contain a disclaimer stating that the creator of the work owns none of the characters. Fan fiction, therefore, is defined by being both related to its subject's canonical fictional universe and simultaneously existing outside the canon of that universe.

Fanonic is free for both readers and authors. It's main features, which I developed last year as part of its initial launch, are:

* the ability to search within the entire text of stories
* story tagging by users
* an activity stream that lets you see when your favorite authors add new stores or publish new chapters and when friends earn new achievement badges.

## What's Been Added

- <strong>Avatars</strong>: Your profile automatically uses your Gravitar image if it's set, and you can also upload an image as your Fanonic avatar.

- <strong>Badges</strong>: A few new achievement badges have been added

- <strong>Favorites</strong>: Stories can be favorited and show up on your list of favorites

- <strong>Fanfiction.net import</strong>: Import your stories from fanfiction.net into your Fanonic story list

- Lots of style improvements

## <a id="coming-next"></a>What's Coming Next

I'm using <a href="http://haystacksearch.org">Haystack</a> to provide story search. Fanonic is hosted on a single server and is using <em>Whoosh</em> to index the site's story data, but my intention is to switch to another indexer/backend for <em>Haystack</em> to allow for future scalability. <a href="http://www.elasticsearch.org/">Elastic Search</a> looks like the best direction to go right now because it looks easier to set up and administer than Solr and should be easier to scale to multiple search index servers than <em>Whoosh</em> or <em>Xapian</em>.

The site will be getting immediate notifications for each user's activity stream and author's story import status updates. I've set up a new repository on github for this subproject: <a href="https://github.com/saebyn/django-tsuchi">django-tsuchi</a>. I'm also looking into adding reader statistics for story authors, but I haven't decided on specifics at this point. Support for importing stories from other fan fiction repositories will be added gradually as well.

I'm getting the word out about Fanonic, and getting in touch with some fanfic authors about bringing their stories to the site. I'm hoping to work with both fanfic creators and readers to build a better experience for everyone.

## Technical Details

### Django, nginx, uwsgi, redis, memcached, postgresql, celery

- The new story importer uses <a href="http://celeryproject.org">Celery</a> to import the content in the in a separate worker process so users can continue using the site while their content is loaded into Fanonic. I'm using <a href="http://redis.io/">redis</a> to hold the queued tasks. Later on, I'm planning on using <em>redis</em> for some future features involving gathering reader statistics as well.

- <a href="http://www.djangoproject.com">Django</a> is a web framework written in <a href="http://www.python.org/">Python</a> that provides a lot of the basic funcionality that Fanonic builds upon. Django has a large and growing ecosystem of third-party pluggable apps. I've published one such app that is going to provide a new feature for the site, which I'm calling <a href="#coming-next">django-tsuchi</a>.

- <a href="http://nginx.org/">nginx</a> serves static content, such as Javascript and CSS files, and acts as a proxy to <a href="http://projects.unbit.it/uwsgi/">uwsgi</a>.

- <em>uwsgi</em> interfaces with the Fanonic <em>Python</em> code to handle incoming requests and return the app's responses.

- Fanonic's database is <a href="http://www.postgresql.org/">PostgreSQL</a>.

- Fanonic uses <a href="http://http://memcached.org/">memcached</a> to cache content generated by the backend.

### Puppet

<a href="http://puppetlabs.com/">Puppet</a> is a tool that allows system administrators to define how the servers they administer are configured, which programs are installed, which services should be running, and what user accounts should be present on them. I'm using <em>Puppet</em> to manage both my local development system and the production server that Fanonic runs. Because both systems have identical setups, I don't have to worry about differences in the server setup breaking the site - what works locally is much more likely to work on the production server.

- I found some helpful <em>Puppet</em> modules on <a href="http://github.com/">github</a>:

  + <a href="https://github.com/example42">example42</a> has quite a few <em>Puppet</em> modules on <em>github</em>. I'm using the <a href="https://github.com/example42/puppet-redis">redis module</a>, and <a href="https://github.com/saebyn/puppet-nginx/">my fork of their nginx module</a>. Many of their modules depend on puppi, which I've <a href="https://github.com/saebyn/puppi">forked</a> to handle some breakage because of the version of <em>Puppet</em> I'm using (2.7.17).

  + I'm using my <a href="https://github.com/saebyn/puppet-memcached/">fork of the memcached module</a> from <a href="https://github.com/danakim/">danakim</a> to set up memcached.

  + I'm using <a href="https://github.com/uggedal/puppet-module-postgresql/">the postgresql module</a> from <a href="https://github.com/uggedal/">uggedal</a> to set up PostgreSQL.

- My local machine has a <a href="http://vagrantup.com/">vagrant</a> box running which automatically loads up the <em>Puppet</em> configuration when it boots.

### Fabric

- I'm using <a href="http://www.fabfile.org/">Fabric</a> to handle code deployment

- Right now I'm using <em>Fabric</em> to push the <em>Puppet</em> manifests to the server and run <code>puppet apply</code> on them, rather than having a <em>puppetmaster</em>.

- I've taken the common tasks for this kind of set up into a <a href="https://github.com/saebyn/fanonic-fabtools"><em>Python</em> module of <em>Fabric</em> tasks</a>.
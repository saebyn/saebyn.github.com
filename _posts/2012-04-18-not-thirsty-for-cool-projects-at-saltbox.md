--- 
layout: post
title: Not thirsty for cool projects at Saltbox
date: 2012-04-18
tags: 
- saltbox
- aws
- swf
- thumbnailing
- rss
alias: [/post/21334020198/not-thirsty-for-cool-projects-at-saltbox]
---

I've been working on the [Saltbox team](http://saltbox.com/about.html) for
about four months now, and I've had the privilege both to work with some very
talented people and work on interesting projects, both on the frontend and the
backend. Here I'm going to summarize what Saltbox is, a little bit about our
talented founders and development team, and the projects I've been working on
over these last few months.

[Saltbox](http://saltbox.com), our sales and learning toolbox, is a web
application that makes it easy to communicate in-house knowledge among peers;
collect news and articles from the web via RSS or Atom feeds; and disseminate
knowledge, news, and quizzes from managers. Central to the application are
_channels_ which act as continous streams of small, easily-digested, content
for learners. Each company or team that sets up a Saltbox site can create
groups to organize their employees or team members, and these groups can then
be given access to channels suitable for that group. Learners can add RSS and
Atom feeds as channels, and these feeds will automatically update as new
content becomes available. Every user gets their own channel where they can
post their own content, and anyone else in the same group will see this peer
channel as available for subscription. Saltbox sites are mobile ready so that
teams on the go can get the company news and product information they need
when they need it.

John Delano and Ali Shahrazad have both been really great as non-technical
founders. They have brought to the table a solid vision of what Saltbox should
give to its users. It's clear that they both have real insight into what sales
teams out there are dealing with in terms of existing learning management
tools and the lack of good tooling for continuous, mobile, and social learning
in companies' sales forces today.

John and Ali put together our development team, Russell Duhon; Brian Gershon;
and myself, and it does them credit. Russell has brought some great ideas and
a strong academic background to the team. He rewrote almost all of our
database queries, ferreting out the original requirements and reimplementing
to reduce the number of queries and code duplication while also improving our
database performance. More recently, he has been leading our efforts in adding
caching to the backend to reduce database trips. Brian rebuilt the entire
Saltbox mobile web app using jQuery mobile (it's quite nice) and has been a
key part in the creation of unit tests and then refactoring of our Javascript
code (which is the majority of our codebase).

Both Russell and Brian have made working on this team a great experience by
both having strong opinions, identifying and communicating opportunities and
pitfalls in code reviews, and being willing to listen and learn from critism
and incorporate it in their work.

I've had a chance to work on several things at Saltbox that I am quite proud
of and have learned some interesting things from. One key piece of
functionality in Saltbox is the ability to add RSS or Atom feeds from the web.

Initially we fetched the feeds as needed, processed them, and sent them on to
the user's browser as needed - essentially proxying the RSS feed into our app.
This had a few serious downsides given our architecture: entries in the feeds
weren't favoritable or searchable, we couldn't easily keep track of read
entries, and proxying the data would, as the number of users on the app
increased, cause major performance issues and likely get the IP addresses of
our servers banned from major sites that were subscribed to frequently. A
different approach was needed.

Our key requirements were that feed content would be stored locally for
searchablity and favoriting, feed processing would occur in the background to
keep our web servers from having being preoccupied with long running feed
processing tasks, feeds would be refreshed depending on the frequency that new
content is available so that we can keep our fetching to a minimum while also
getting the latest content for our users, and Javascript and other undesirable
markup would be removed from the feed entries before display to the user. The
end result was an integration of our Python web backend determining which
feeds should be reloaded at a given time; a cronjob that polls our web backend
for the feeds and inserts tasks to fetch the feeds into [Amazon's Simple Queue
Service](http://aws.amazon.com/sqs/) (SQS); and a
NodeJS/[CoffeeScript](http://coffeescript.org) service that polls SQS for feed
tasks, processes their content, and posts the new content back to our web
backend.

Another project that spun off from the feed processing service was our
thumbnailing system. We find and extract a suitable image from each entry in
the feed content and generate a thumbnail to display in our app. We ran into
some difficulties performing thumbnailing reliably as part of the feed
processing task on NodeJS. This was because thumbnailing took significantly
more time than processing the feed itself and handling both tasks separately
while making sure that thumbnails still got connected to the correct feed
entry proved very difficult to implement. I built a scalable thumbnailing
generation service in Java using Amazon's [Simple Workflow
Service](http://aws.amazon.com/swf/) and Flow Framework (which is part of the
[Java SDK](http://aws.amazon.com/sdkforjava/)).

This service takes one or more URLs from the feed processing system via HTTP
POST to a round-robin DNS entry, creates a loading placeholder for the
thumbnail, fetches an image from each URL, filters these images based on a
"thumbnail specification", scales the selected image, and stores the final
thumbnail in S3 (another amazon service). Amazon's workflow service allows the
system to scale to an extent that I doubt we'll reach. The service is
currently fetching, analyzing, resizing, and storing about 12,000 thumbnail
images every 24 hours, on two servers that could likely handle several times
that amount. It's been pretty fun to build this system and see it actually
work. Very exciting!

This post has gotten quite long, so I will have to dig into more depth about
some of these things in future posts. I've had a great time as part of Saltbox
team and I am really thankful of the opportunity its been so far.


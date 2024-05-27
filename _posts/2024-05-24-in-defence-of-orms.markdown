---
layout: post
title:  In Defence of ORMs"
date:   TODO
categories: serious
---
Object Relational Mapping tools are an extremely contentious topic these days. They are extremely 

Today, all it takes is for someone to write on a forum or walk up on speaker stage, proclaim:

> Don't use an ORM - just write SQL

and what follows will be a standing ovation, virtual or not.

In my experience, your typical ORM hater is smart, productive and very influential in wherever they work. That is why it has taken me so long to figure out whether I disagree with them, or if I am simply too stupid to have come to the same conclusion.

It was not until I heard that at least one more influential programming influencer actually DO see them as valuable that I begin to let myself defend them intellectually.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">highly recommended even if you don&#39;t end up using it, lots of great &amp; interesting ideas<br><br>ActiveRecord remains my favorite ORM, very pragmatic, stays out of the way &amp; leverages OO in the right way<a href="https://t.co/OIIT10GDiw">https://t.co/OIIT10GDiw</a> <a href="https://t.co/DyeNU3Qml0">https://t.co/DyeNU3Qml0</a></p>&mdash; htmx.org / CEO of SEO (same thing) (@htmx_org) <a href="https://twitter.com/htmx_org/status/1719533526951846169?ref_src=twsrc%5Etfw">November 1, 2023</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## The criticism

The hatred for ORMs is not unfounded. I would never go so far as to say that anyone who says their application became simpler when they tossed it out in favour of just writing SQL strings is lying. Lots and lots of of especially Java shops have been burnt on the JPA/Hibernate stove, and you will hear them complaining about: 
- fighting weird, implicit `1+N` performance problems
- debugging unexpected flushing and cache invalidation behaviours
- never truly understanding the correct combination of annotations to get their many-to-many to work

It is natural that they start to gag when an online guide is suggesting you pick it up "to avoid having to learn or write SQL". But I feel like the popularized position of rejecting the entire concept of an ORM means throwing out the baby with the bathwater. I contend that the problem with ORMs is not the concept, but something else.

## The problem



Links:
- https://www.reddit.com/r/programming/comments/2cnw8x/comment/cjhcoc7/
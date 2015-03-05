---
layout: post
title: JPAContainer does not update relationship
---

Problem is that JPAContainer does not update relationship of entities.
<!--more-->
JPAContainer have a page length (default 15) and cache rate (default 2.0) options. First it is how much entities will be loaded after container creating. When you write a JPAContainerFactory.make(...), 15 entities load from database. Cache rate it is how much entities will be additionally cached (15 * 2.0 - 15 = 15 will be cached additionaly, and overall 30 entities loaded from database).

But I have a problem with it. We have entity and it has fields with relationship (many-to-many, many-to-one and so on). When JPAContainer creating the first time, it is cached some entities (pageLength + cache). And problem is that entity in cache does not update their relationship. During full life of application (until it's restarted).

For example: we have two types Person and Address. Person has many Address, Address belongs to only one Person. In database there are 16 entities Person type. Every entity loading when container created. But if you try to change set of Address of sixth Person. Nothing will be shown in interface (but in database everything will be ok).

This code helps me, but it's bad influence on the performance of application.
I am do not using auto commit, so this a little different than in most projects.
<pre class="lang:java decode:true ">JPAContainer newContainer = JPAContainerFactory.makeBatchable(requestedClass, "persistence");
newContainer.setAutoCommit(false);
newContainer.setBuffered(true);

newContainer.getEntityProvider().getEntityManager().getEntityManagerFactory().getCache().evict(requestedClass);
</pre>
The last line is solution, it is just erase cache.

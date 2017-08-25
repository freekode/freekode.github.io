---
layout: post
title: Today's Plan Addition to API Documentation
---

I'm using Today's Plan as a planning software and realy happy with it.
It costs, but flexible calendar, which is one of the most important thing for me, really nice.

They have [public API](http://api.todaysplan.com.au/), but unfortunately docs describes not all REST API which they have.
In this article I try to fix that. Here I will add rest endpoints which are not exist in documentation, but they are available.

### Wellness

`GET https://whats.todaysplan.com.au/rest/users/day/rating/{userId}/{fieldName}/{rating}/{year}/{dayOfTheYear}`

Example

`https://whats.todaysplan.com.au/rest/users/day/rating/7185324/diet/0/2017/234`

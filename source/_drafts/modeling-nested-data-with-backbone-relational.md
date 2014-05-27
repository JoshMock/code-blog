title: 'Modeling nested data with Backbone-relational'
comments: false
date: 2014-05-26 14:36:22
tags:
- backbone.js
- javascript
- backbone-relational
---
One of the first major pitfalls JavaScript developers run into after they've warmed up to Backbone.js is that it has very weak support for nested data structures. This can be especially frustrating when trying to build a Backbone application using a predetermined JSON structure that can't, or shouldn't be changed.

A developer's first instinct might be to write some map/reduce logic to flatten the data on load. This works well enough for read-only applications, but as soon as that data needs to get saved back out in the original format, efforts have to be doubled to put the data back.

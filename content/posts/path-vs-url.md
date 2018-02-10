---
title: Path vs URL
date: 2016-09-01T20:21:43+01:00
---

Today's post will be shorter than usual.

One day I started wondering what's the difference between Rails _some\_route\_path_ and _some\_route\_url_. Because at first, it looked like both returned the same address. After some research and help from Arkency, I learned that the difference is in fact quite significant.

Path helper provides a path that is relative to the root of a site. And should always be used if you provide a link inside your application. In that case, domain part is redundant.

URL, on the other hand, provides an absolute path, including protocol and server name. And should be used when you create a link for use outside your application. For example, when you send an email to confirm password absolute path is required.

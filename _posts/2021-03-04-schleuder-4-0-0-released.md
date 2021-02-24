---
layout: post
title:  "Schleuder 4.0.0 released"
date: "2021-03-04"
---

This release brings one new feature, some bug fixes, and several dependency changes.

Please note that this release does not contain the breaking changes to the HTTP API which were planned for it originally.

Highlights:

* Mandatory blank line to separate keywords from email content. Arguments for keywords are now looked for in the following lines, too, until a blank line (or the next keyword) is found.
* Update ActiveRecord to version 6.1.
* Require Ruby version >= 2.5.
* Support Ruby 3.0.
* Require GnuPG version >= 2.2.
* Drop the deprecated `X-LISTNAME`. Please only use `X-LIST-NAME` from now on.
* Downcase all email addresses when subscribing.
* Require fingerprints as arguments to `X-DELETE-KEY`, no fuzzy matching of strings anymore.

Please see the [user-relevant changes]({{ "schleuder/docs/changes.html" | absolute_url }}) and the [changelog](https://0xacab.org/schleuder/schleuder/blob/master/CHANGELOG.md#400-2021-03-04) for details.

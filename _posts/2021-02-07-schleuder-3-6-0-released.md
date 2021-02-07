---
layout: post
title:  "Schleuder 3.6.0 released"
date: "2021-02-07"
---

This release ships new features and bug fixes:

* The list options are enhanced, via:
  * `set_reply_to_to_sender` sets the `Reply-To`-header of emails sent to subscriptions to the value of the `Reply-To`- or the `From`-header of the incoming email.
  * `munged_from` makes the `From`'s "display name" of emails sent to subscriptions to include the sender's email address of the incoming mail (e.g. `"me@example.org via list@example.net" <list@example.net>`).
* The detection of bounce messages is massively improved.
* Mail addresses of subscriptions are properly validated.

Please see the [user-relevant changes]({{ "schleuder/docs/changes.html" | absolute_url }}) and the [changelog](https://0xacab.org/schleuder/schleuder/blob/master/CHANGELOG.md#360-2021-02-FIXME) for details.

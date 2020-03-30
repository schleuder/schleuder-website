---
layout: post
title:  "Schleuder 3.5.0 released"
date: "2020-03-30"
---

This release ships new features, bug fixes and various UX-related improvements:

* The list options are enhanced, via:
  * an option to include their public keys in the headers of outgoing emails (conforming with Autocrypt, [https://autocrypt.org/](https://autocrypt.org)),
  * an option to control whether subscribers get a copy of mail they sent themselves,
* pseudoheaders are wrapped at 78 characters and a visual separator is added between the pseudoheaders block and the rest of the body,
* messages sent by Jenkins or sudo are not recognized as automated,
* the output of `x-resend` and friends is more precise,
* attaching the public key of a list works if using Thunderbird,
* better detection of the encoding of mails, which should reduce the amount of problematic mails causing errors,
* incoming mails encrypted to an absent key, using symmetric encryption or containing PGP-garbage are handled in a more graceful manner,
* notification mails sent to admins contain a List-Id header, which should help with filtering such messages.

Please see the [user-relevant changes]({{ "schleuder/docs/changes.html" | absolute_url }}) and the [changelog](https://0xacab.org/schleuder/schleuder/blob/master/CHANGELOG.md#350-2020-03-30) for details.

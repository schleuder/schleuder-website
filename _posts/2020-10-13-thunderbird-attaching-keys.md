---
layout: post
title:  "Beware of Thunderbird 78 attaching keys"
date: "2020-10-13"
---

This is a warning about Thunderbird version 78 and later versions:

Please be aware that since version 78, Thunderbird by default attaches your public key to every outgoing, signed email. With normal emails this is not a problem.
With emails to a schleuder-list that contain X-RESEND it's different though: it reveals the public key of the actual sender, which probably leaks information about your email address and maybe name to the recipients.

To prevent that, please de-select `Attach My Public Key` from the `Options`-menu _in each X-RESEND email_.

As of now there is no option to generally disable attaching your key. [It has already been wished for](https://bugzilla.mozilla.org/show_bug.cgi?id=1654950), though.


---
title: Changes from Schleuder 3.5 to 3.6
---

{: .note}
This list describes changes that users or list-admins of Schleuder should be aware of. For a more technical changelog, please see the [repository](https://0xacab.org/schleuder/schleuder).

{% include docs-head.md version=3.6 %}

User-relevant changes in version 3.6 compared to version 3.5:

* The list options are enhanced, via:
  * `set_reply_to_to_sender` sets the `Reply-To`-header of emails sent to subscriptions to the value of the `Reply-To`- or the `From`-header of the incoming email.
  * `munged_from` makes the `From`'s "display name" of emails sent to subscriptions to include the sender's email address of the incoming mail (e.g. `"me@example.org via list@example.net" <list@example.net>`).
* The detection of bounce messages is massively improved.
* Mail addresses of subscriptions are properly validated.

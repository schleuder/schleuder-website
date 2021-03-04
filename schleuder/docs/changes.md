---
title: Changes from Schleuder 3.6 to 4.0
---

{: .note}
This list describes changes that users or list-admins of Schleuder should be aware of. For a more technical changelog, please see the [repository](https://0xacab.org/schleuder/schleuder).

{% include docs-head.md version=4.0 %}

User-relevant changes in version 4.0 compared to version 3.6:

* It is now mandatory to use a blank line to separate keywords from email content. This change allows looking for keyword arguments in the following lines, too. If you omit the blank line, the start of your email content will be interpreted as keyword argument (which probably will result in an error). There is no need to use blank lines between multiple keywords.
* Please only use `X-LIST-NAME` from now on, `X-LISTNAME` has been removed.
* `X-DELETE-KEY` may only be given fingerprints in order to identify keys. No fuzzy matching of strings happens anymore in order to avoid accidental destructive actions.


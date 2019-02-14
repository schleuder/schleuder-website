---
title: Changes from Schleuder 3.3 to 3.4
---

{: .note}
This list describes changes that users or list-admins of Schleuder should be aware of. For a more technical changelog, please see the [repository](https://0xacab.org/schleuder/schleuder).

{% include docs-head.md version=3.4 %}

User-relevant changes in version 3.4 compared to version 3.3:

* HTML is stripped from multipart/alternative messages if they contain keywords. This is to prevent those keywords from being visible to recipients of resend-messages, and thus leaking confidential information. Filtering the keywords from the HTML is not solidly feasible, so removing the HTML-part entirely is the only option. Generally we recommend to not use HTML with Email at all, as it brings a lot of problems and insecurities.

{% include feedback.md %}

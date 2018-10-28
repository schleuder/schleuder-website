---
title: Changes from Schleuder 3.2 to 3.3
---

{: .note}
This list describes changes that users or list-admins of Schleuder should be aware of. For a more technical changelog, please see the [repository](https://0xacab.org/schleuder/schleuder).

User-relevant changes in version 3.3 compared to version 3.2:

* Handle missing arguments for several keywords and reply with a helpful error-message.
* Send replies to keyword-usage and notices to admins regardless of the delivery-flag of their subscription.
* `X-UNSUBSCRIBE` will refuse to unsubscribe the last admin of a list.
* Handle "protected subjects" in a way that Thunderbird/Enigmail recognize.
* `X-SET-FINGERPRINT` will not anymore allow setting an empty fingerprint.
* To remove a fingerprint from a subscription one can use the new keyword `X-UNSET-FINGERPRINT`.
* The output of the keywords `X-ADD-KEY` and `X-DELETE-KEY` now also show the "oneline"-format to represent keys.
* In the response to `X-ADD-KEY`, differentiate between "newly imported" and "updated" keys.
* Parse keywords up to the first line detected as mail content.
* Extend the pseudoheaders configuration option to support `sig` and `enc` as configurable and sortable fields.

{% include feedback.md %}

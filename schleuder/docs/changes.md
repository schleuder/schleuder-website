---
title: Changes from Schleuder 3.2 to 3.3
---

{: .note}
This list describes changes that users or list-admins of Schleuder should be aware of. For a more technical changelog, please see the [repository](https://0xacab.org/schleuder/schleuder).

{% include docs-head.md version=3.3 %}

User-relevant changes in version 3.3 compared to version 3.2:

* List-admins now receive replies to keyword-usage and notices regardless of the delivery-flag of their subscription.
* Keywords are now only looked for in the beginning of the message, up to the first line detected as mail content. Later content will not be interpreted or touched.
* `X-UNSUBSCRIBE` will refuse to unsubscribe the last admin of a list.
* `X-SET-FINGERPRINT` will not anymore allow setting an empty fingerprint. To remove a fingerprint from a subscription one can use the new keyword `X-UNSET-FINGERPRINT`.
* Missing arguments for several keywords are better handled and replied to with a helpful error-message.
* In the response to `X-ADD-KEY`, differentiate between "newly imported" and "updated" keys.
* In the response to `X-ADD-KEY` and `X-DELETE-KEY`, more information on each key than just the fingerprint is shown.
* In the pseudoheaders, `sig` and `enc` are now configurable. They can be reordered in or removed from the list-option `headers_to_meta`.
* Schleuder now handles "protected subjects" in a way that Thunderbird/Enigmail recognize.

{% include feedback.md %}

---
title: Changes from Schleuder 3.1 to 3.2
---

{: .note}
This list describes changes that users or list-admins of Schleuder should be aware of. For a more technical changelog, please see the [repository](https://0xacab.org/schleuder/schleuder).

User-relevant changes in version 3.2 compared to version 3.1:

* To improve consistency with the English language, `X-LISTNAME` has been renamed to `X-LIST-NAME`. `X-LISTNAME` still works, but will be deprecated, therefore please use `X-LIST-NAME` from now on.
* Lists can have an internal-footer now. It's appended to each message sent to a subscriber (if it's not blank).
* New keywords: `X-RESEND-UNENCRYPTED` and `X-RESEND-CC-UNENCRYPTED`. With them you can enforce that resent-email will be sent in the clear, regardless of whether we would find a key for the recipient or not.
* The short representation of GnuPG keys became more human-friendly. Besides the fingerprint we now show the email-address of the first UID, the generation-date, and (if present) the expiration-date.
* The resend-keywords now check the given arguments to be valid email-addresses, and block resending if any one is found invalid.
* `X-LIST-KEYS` does not require an argument anymore.
* `X-SUBSCRIBE` now handles the combination of space-separated fingerprint and additional arguments (admin-flag, delivery-enabled-flag) correctly.
* `X-RESEND` now respects the encoding the mail was sent with.
* Fixed broken encoding of certain character-sequences in encrypted+signed messages.


{% include feedback.md %}

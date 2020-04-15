---
title: Changes from Schleuder 3.4 to 3.5
---

{: .note}
This list describes changes that users or list-admins of Schleuder should be aware of. For a more technical changelog, please see the [repository](https://0xacab.org/schleuder/schleuder).

{% include docs-head.md version=3.5 %}

User-relevant changes in version 3.5 compared to version 3.4:

* The list options are enhanced, via:
  * an option to include their public keys in the headers of outgoing emails (conforming with Autocrypt, [https://autocrypt.org/](https://autocrypt.org)),
  * an option to control whether subscribers get a copy of mail they sent themselves.
* Pseudoheaders are wrapped at 78 characters.
* A visual separator is added to the end of the pseudoheaders block. This should help users of Apple Mail, which jams this block and the body together. Hopefully, this change makes it easier to dinstiguish both parts from each other.
* Messages sent by Jenkins or sudo are not recognized anymore as automated messages.
* The output of `x-resend` and friends is improved and more precise:
  * Tell how many keys are in the keyring and how many are usable.
  * Make it more clear what happens when resending an encrypted email fails (due to missing or too many matching keys), but falling back to unencrypted resend is allowed.
  * Be more explicit that resending to other CC recipients has been aborted.
* Attaching the public key of a list works, if using `x-attach-listkey` via Thunderbird. Before the output was garbled and hardly usable.
* The detection of the encoding of incoming mails is improved. This should reduce the amount of mails which were problematic in this regard, and lead to errors.
* Incoming mails encrypted to an absent key, using symmetric encryption or containing PGP-garbage are handled in a more graceful manner: No exception is thrown, admins aren't notified (and annoyed), instead the sender is informed how to do better.
* Notification mails sent to admins contain a List-Id header. This should help with filtering such messages, which wasn't easy to do in a reliable way.
* `x-add-key` is able to handle mails with attached, quoted-printable encoded keys. Such mails might be produced by Thunderbird.

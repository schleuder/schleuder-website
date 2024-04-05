---
title: Documentation for list-admins
---

{% include docs-head.md version=4.0 %}

{% include_relative _list_usage_basics.md %}


#### Subscription and key management

These keywords must be send to `foo-request@hostname`. They are used to get information about the list, its subscribers and keys, or to change that information.

**x-list-subscriptions**
: List all subscriptions.

**x-subscribe:** person@example.org 0x12345678DEADBEEF12345678DEADBEEF12345678 false true
: Subscribe the given email address and assign it the given OpenPGP fingerprint. The last two arguments can be omitted and will default to "false true": do not give the given email address administration permissions and enable email delivery.

**x-unsubscribe:** person@example.org
: Unsubscribe the given address. It is not possible to unsubscribe the last admin of a list.

**x-set-fingerprint:** 0x12345678DEADBEEF12345678DEADBEEF12345678
: Assign the key with the given fingerprint to your subscription. It is not possible to set an empty fingerprint. To unset a fingerprint use `x-unset-fingerprint`.

**x-set-fingerprint:** person@example.org 0x12345678DEADBEEF12345678DEADBEEF12345678
: Assign the key with the given fingerprint to the given subscription. This variant of this command may only be used by list-admins. To unset a fingerprint use `x-unset-fingerprint`.

**x-unset-fingerprint:** person@example.org
: Remove the fingerprint associated with a given subscription.

**x-list-keys**
: Lists all public keys known to the list. To see only keys that match a given string, use `x-list-keys: something`.

**x-add-key:**
: Import the email's attachment(s) or the rest of the email-body into the list’s keyring. To paste your public key into the body of the mail, please export it in _"ASCII-armored"_ text format (e.g. `gpg --armor --export 0xAE0DBF5A92A5ADE49481AB6F8A3171EF366150CE`). Please make sure you put a blank line between the keyword(s) and the key material. If you send the key as an **attachment** of the email, **both** _"ASCII-armored"_ and binary format are supported.

**x-delete-key:** 0x12345678DEADBEEF12345678DEADBEEF12345678
: Delete the key with the given fingerprint from the list's keyring.

**x-get-key:** 0x12345678DEADBEEF12345678DEADBEEF12345678
: Export the key with the given fingerprint from the list's keyring.

**x-fetch-key:** 0x12345678DEADBEEF12345678DEADBEEF12345678
: Fetch the key with the given fingerprint from a keyserver and import it into the list's keyring. (This works only if a keyserver has been configured by the provider.)

This keyword must be send to the normal list-address: `foo@hostname`.

**x-attach-listkey:**
: Attaches the public key of the list. This might be useful for example after the setup of a new list, to distribute the public key of the list to all subscribers.


#### Other

These must also be sent to the request-address: <foo-request@hostname>.

**x-get-logfile:**
: Sends the logfile of the list.

**x-get-version:**
: Returns the version of schleuder.

**x-sign-this:**
: Sign the remaining contents of the email body or the attachments with the list's key. Use this e.g. to provide GnuPG-signatures for things you are publishing.

{% include participate.md %}

---
title: Documentation for list-admins
---

{% include docs-head.md version=3.2 %}

{% include_relative _list_usage_basics.md %}


#### Subscription and key management

These keywords must be send to `foo-request@hostname`. They are used to get information about the list, its subscribers and keys, or to change that information.

x-list-subscriptions
: List all subscriptions.

x-subscribe: person@example.org 0x12345678DEADBEEF12345678DEADBEEF12345678
: Subscribe the given address and assign it the given OpenPGP-fingerprint.

x-unsubscribe: person@example.org
: Unsubscribe the given address.

x-set-fingerprint: 0x12345678DEADBEEF12345678DEADBEEF12345678
: Assign the key with the given fingerprint to your subscription.

x-set-fingerprint: person@example.org 0x12345678DEADBEEF12345678DEADBEEF12345678
: Assign the key with the given fingerprint to the given subscription. This variant of this command may only be used by list-admins.


x-list-keys
: Lists all public keys known to the list. To see only keys that match a given string, use `x-list-keys: something`.

x-add-key:
: Import the attachments or the rest of the email-body into the list's keyring. Only ascii-armored keys are supported.

x-delete-key: 0x12345678DEADBEEF12345678DEADBEEF12345678
: Delete the key with the given fingerprint from the list's keyring.

x-get-key: 0x12345678DEADBEEF12345678DEADBEEF12345678
: Export the key with the given fingerprint from the list's keyring.

x-fetch-key: 0x12345678DEADBEEF12345678DEADBEEF12345678
: Fetch the key with the given fingerprint from a keyserver and import it into the list's keyring. (This works only if a keyserver has been configured by the provider.)

This keyword must be send to the normal list-address: `foo@hostname`.

x-attach-listkey:
: Attaches the public key of the list. Probably most useful in combination with x-resend.


#### Other

These must also be sent to the request-address: <foo-request@hostname>.

x-get-logfile:
: Sends the logfile of the list.

x-get-version:
: Returns the version of schleuder.

x-sign-this:
: Sign the remaining contents of the email body or the attachments with the list's key. Use this e.g. to provide GnuPG-signatures for things you are publishing.

{% include participate.md %}

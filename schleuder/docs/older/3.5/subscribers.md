---
title: Documentation for subscribers
---

{% include docs-head.md version=3.5 %}

{% include_relative _list_usage_basics.md %}

#### Subscription and key management

The following keywords must be send to the request address of the list: `foo-request@hostname`.

x-set-fingerprint: 0x12345678DEADBEEF12345678DEADBEEF12345678
: Assign the key with the given fingerprint to your subscription. It is not possible to set an empty fingerprint. To unset your fingerprint use `x-unset-fingerprint`.

x-unset-fingerprint: person@example.org
: Remove the fingerprint associated with your subscription.

#### Resending

The resending-keywords must be included in messages sent to the normal list-address: `foo@hostname`.

**x-attach-listkey:**
: Attachs the public key of the list. Probably most useful in combination with x-resend.

**x-resend:** someone@example.org
: Send the message to the given address, encrypted if possible, otherwise in the clear.

**x-resend-encrypted-only:** someone@example.org
: Send the message to the given address only if it could be encrypted. Can be abbreviated to `x-resend-enc`.

**x-resend-unencrypted:** someone@example.org
: Send the message to the given address without encrypting it. You can use this keyword to make schleuder skip looking for a matching key for this address and enforce sending the email out in the clear.

**x-resend-cc:** someone@example.org anotherperson@example.org
: Send one message to all of the given addresses in Cc, so they get to know of each other (encrypted if possible, otherwise in the clear).

**x-resend-cc-encrypted-only:** someone@example.org
: Send one message to all of the given addresses in Cc, so they get to know of each other, only if it could be encrypted to all of those addresses. Can be abbreviated to `x-resend-cc-enc.`

**x-resend-cc-unencrypted:** someone@example.org
: Send one unencrypted message to all of the given addresses in Cc, so they get to know of each other. We skip looking for any key and will just send out the email in the clear.


### Contact list-owner

Write to `foo-owner@hostname` to contact the list-owner(s) even if you don't know who they are. Use the list's key to encrypt the email!


{% include participate.md %}

---
title: Documentation for subscribers
---

{% include docs-head.md version=4.0 %}

{% include_relative _list_usage_basics.md %}

&nbsp;  


#### Subscription and key management

The following keywords must be send to the request address of the list: `foo-request@hostname`.  
Include `x-list-name: foo@hostname` with every command you send.

**x-add-key:**  
: Import the email's attachment(s) or the rest of the email-body into the list’s keyring. To paste your public key into the body of the mail, please export it in _"ASCII-armored"_ text format (e.g. `gpg --armor --export 0xAE0DBF5A92A5ADE49481AB6F8A3171EF366150CE`). If you send the key as an **attachment** of the email, **both** _"ASCII-armored"_ and binary format are supported. 


**x-set-fingerprint:** 0x12345678DEADBEEF12345678DEADBEEF12345678
: Assign the key with the given fingerprint to your subscription. It is not possible to set an empty fingerprint. To unset your fingerprint use `x-unset-fingerprint`.

**x-unset-fingerprint:** person@example.org
: Remove the fingerprint associated with your subscription.

&nbsp;  


#### Example: Update your key of a list subscription

**All commands to switch to a new key must be signed with the current old key.**

##### 1. Submit the new key to the keyring of the list

```
x-list-name: foo@hostname
x-add-key

-----BEGIN PGP PUBLIC KEY BLOCK-----

ASak6ezpIZkSZ/ql7UOiOIxi7dAWg4YwFB+yrkN+aUi9Io+No1Y0Rjz+/pUIvGx7
KbyhUQjE6wvGJKDqWyLQVoyB+R0ZV3k6lQFqa7TETXCoGuU8CRM4XcynU7MNgGFQ
...
mDMEXuYDuxYJKwYBBAHaRw8BAQdAqP98Ao=
=32SG
-----END PGP PUBLIC KEY BLOCK-----
```

A successful answer should look something like this:
> This key was newly added:
> 0x12345678DEADBEEF12345678DEADBEEF12345678 youraccount@yourmail.net 2019-05-23 [expires: 2023-05-23] 

##### 2. Verify that the uploaded key made it into the keyring

```
x-list-name: foo@hostname
x-list-keys
```
The answer will be a list of all keys in the keyring of the list.
Make sure, your **new** key is in the keyring now.

##### 3. Change your subscription to use the new key

```
x-list-name: foo@hostname
x-set-fingerprint: 0x12345678DEADBEEF12345678DEADBEEF12345678
```

A successful answer should look something like this:
> Fingerprint for youraccount@yourmail.net set to 12345678DEADBEEF12345678DEADBEEF12345678.

From now on, all mails will be encrypted to the **new** key and all your mails must be signed with this key.

##### One caveat: Disabled commands 

To further improve security and confidentiality, list-admins can manually **disable certain commands**
like `x-add-key` for mere subscribers of the list. With these commands disabled for you,
you have to rely on the list-admin to handle key management. 

&nbsp;  

#### Resending

The resending-keywords must be included in messages sent to the normal list-address: `foo@hostname`.

**x-attach-listkey:**
: Attaches the public key of the list. Probably most useful in combination with x-resend.

**x-resend:** someone@example.org
: Send the message to the given address, encrypted if possible, otherwise in the clear.

**x-resend-encrypted-only:** someone@example.org
: Send the message to the given address only if it could be encrypted. Can be abbreviated to `x-resend-enc`.

**x-resend-unencrypted:** someone@example.org
: Send the message to the given address without encrypting it. You can use this keyword to make schleuder skip looking for a matching key for this address and enforce sending the email out in the clear.

**x-resend-cc:** someone@example.org anotherperson@example.org
: Send one message to all of the given addresses in Cc, so they get to know of each other (encrypted if possible, otherwise in the clear).

**x-resend-cc-encrypted-only:** someone@example.org anotherperson@example.org
: Send one message to all of the given addresses in Cc, so they get to know of each other, only if it could be encrypted to all of those addresses. Can be abbreviated to `x-resend-cc-enc.`

**x-resend-cc-unencrypted:** someone@example.org anotherperson@example.org
: Send one unencrypted message to all of the given addresses in Cc, so they get to know of each other. We skip looking for any key and will just send out the email in the clear.


### Contact list-owner

Write to `foo-owner@hostname` to contact the list-owner(s) even if you don't know who they are. Use the list's key to encrypt the email!


{% include participate.md %}

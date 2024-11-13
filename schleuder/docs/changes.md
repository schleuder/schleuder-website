---
title: Changes from Schleuder 4.0 to 5.0
---

{: .note}
This list describes changes that users or list-admins of Schleuder should be aware of. For a more technical changelog, please see the [repository](https://0xacab.org/schleuder/schleuder).

{% include docs-head.md version=5.0 %}

User-relevant changes in version 5.0 compared to version 4.0:

* Schleuder by default doesn't lookup keys on a traditional HKP keyserver anymore. Admins can still set the config option to a keyserver of choice and Schleuder will use it.
* Schleuder now fetches keys also and primarily from a VKS keyserver (the default is `keys.openpgp.org`). A traditional HKP keyserver is only queried if the VKS server didn't have a key (and if it is configured).
* Schleuder now knows how to import a key from an incoming email: if the email is encrypted or signed, and if the list doesn't know a key matching the sender's address (`From` header), then Schleuder will check if such a matching key is present in the email's attachments or its Autocrypt-headers. This hopefully is helpful in case a list receives emails from a lot of different people with fluctuating key usage. The feature is configurable per list, and is off by default.
* After adding keys, the users now directly see details about the added keys. This is meant to help in situations in which people accidentally upload expired keys.

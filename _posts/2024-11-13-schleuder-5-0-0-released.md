---
layout: post
title:  "Schleuder 5.0.0 released"
date: "2024-11-13"
---

This release brings some improvements and bug fixes, but breaks usage only by changing technical dependencies and dropping the default setting of one config option:

* Schleuder by default doesn't lookup keys on a traditional HKP keyserver anymore. The HKP keyserver network is basically dead and should be avoided. You can still set the config option to a keyserver of your choice and Schleuder will use it. We encourage to do so only for private keyservers, though.
* Schleuder now fetches keys also and primarily from a VKS keyserver (the default is `keys.openpgp.org`). A tranditional HKP keyserver is only queried if the VKS server didn't have a key (and if it is configured).
* Schleuder doesn't use `dirmngr` anymore but communicates with keyservers using own code. For this it newly requires `libcurl` and the rubygem "typhoeus".
* Schleuder now knows how to import a key from an incoming email: if the email is encrypted or signed, and if the list doesn't know a key matching the sender's address (`From` header), then Schleuder will check if such a matching key is present in the email's attachments or its Autocrypt-headers. This hopefully is helpful in case a list receives emails from a lot of different people with fluctuating key usage. The feature is configurable per list, and is off by default.
* After uploading keys, the users now directly see details about the uploaded keys. This is meant to help in situations in which people accidentally upload expired keys.
* The oldest supported Ruby version is now 3.1.

Please see the [changelog](https://0xacab.org/schleuder/schleuder/blob/main/CHANGELOG.md#500--2024-11-13) for further details.

We dedicate this release to René Paulokat. He helped to kickstart this project, and his influence is still present in the code base to this day. We will keep this project alive – as long as it matters, just as René would have done. May he rest in peace. 🖤

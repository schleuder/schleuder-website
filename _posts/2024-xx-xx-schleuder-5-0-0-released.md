---
layout: post
title:  "Schleuder 5.0.0 released"
date: "2024-XX-XX"
---

This release brings some improvements, but breaks usage only by changing technical dependencies and dropping the default setting of one config option:
* Schleuder now fetches OpenPGP-keys from (different types of) keyservers "manually", without using `dirmngr`. For this it newly requires the rubygem "typhoeus" and `libcurl`.
* Schleuder by default doesn't lookup keys on keyserver.ubuntu.com anymore. The HKP keyserver network is basically dead and should be avoided. You can still set the config option to a keyserver of your choice and Schleuder will use it. We encourage to do so only for private keyservers, though.
* Schleuder now knows how to import a key from an incoming email: if the email is encrypted or signed, and if the list doesn't know a key matching the sender's address (`From` header), then Schleuder will check if such a matching key is present in the email's attachments or its Autocrypt-headers. This hopefully is helpful in case a list receives emails from a lot of different people with fluctuating key usage. The feature is configurable per list, and is off by default.
* TODO: ...

Please see the [changelog](https://0xacab.org/schleuder/schleuder/blob/main/CHANGELOG.md#500-2024-XX-XX) for details.

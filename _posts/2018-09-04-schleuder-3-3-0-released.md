---
layout: post
title:  "Schleuder 3.3.0 released!"
date: "2018-09-04"
---

This release contains a few bug fixes and improvements: Missing arguments to keywords are now properly handled, and also more helpful error messages are provided; "Protected Headers" now are handled in a way that Thunderbird/Enigmail recognize; we also introduced a new keyword to remove a fingerprint from a subscription, and protected X-SET-FINGERPRINT better against accidental mis-use. Additionally the pseudoheaders now fully depend on the 'headers_to_meta' configuration-option, including 'sig' and 'enc'; in effect you can now also have a fully empty pseudoheaders-block, which might come in handy for newsletters. Thanks to all contributors! Packages for Debian and CentOS will be released soon. For all details please see the [changelog](https://0xacab.org/schleuder/schleuder/blob/master/CHANGELOG.md#330-2018-09-04).
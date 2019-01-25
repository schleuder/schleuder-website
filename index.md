---
layout: home
title: Schleuder
subtitle: A gpg-enabled mailing list manager with remailing-capabilities.
---

{::options parse_block_html="true" /}

Schleuder is a group's email-gateway: subscribers can exchange encrypted emails among themselves, receive emails from non-subscribers and send emails to non-subscribers via the list. [How it works](schleuder/docs/concept.html)

We give our time and knowledge to build and maintain this project in order to help people with their daily private communication and in the struggle for their personal emancipation, social and economic justice and political freedom. [Why we do it](MISSION_STATEMENT.html)


<div class='block smallblock'>
### Schleuder

Documentation:

* [for subscribers](schleuder/docs/subscribers.html)
* [for list-admins](schleuder/docs/list-admins.html)
* [for server-admins](schleuder/docs/server-admins.html)
</div>

<div class='block smallblock'>
### Projects

These projects also belong to the schleuder-family:

* [schleuder-web](schleuder-web)
* [schleuder-cli](schleuder-cli)
</div>


<div class='block' id='news'>
### News

<span class='date'>2019-01-24</span>: **Meet Schleuder devs in Frankfurt/Main**. For everyone located in or near Frankfurt/Main, Germany (or willing to travel): On Friday, Feb 1st, the Schleuder development team will meet in a restaurant in Frankfurt, and would be pleased to have a chat with you! We have no schedule, just time to sit around, discuss ideas, ask questions, and have some drinks or food. Anyone is welcome, be they technically minded, politically interested or simply curious! *Friday, 2019-02-01, from 7pm (local time), at Heck Meck* (Friesengasse 19, Frankfurt am Main, Germany).

<span class='date'>2018-10-22</span>: **Meet Schleuder devs in Hamburg**. For everyone located in or near Hamburg, Germany (or willing to travel): Next Friday evening the Schleuder development team will meet in a bar in Hamburg, and would be pleased to have a chat with you! There is no program, just time to sit around, discuss ideas, ask questions, and have some drinks or food. Friday, 2018-10-26, from 7pm (UTC+2), at Feldstern (Sternstrasse 2, Hamburg, Germany).

<span class='date'>2018-09-04</span>: **Schleuder 3.3.0 released!** This release contains a few bug fixes and improvements: Missing arguments to keywords are now properly handled, and also more helpful error messages are provided; "Protected Headers" now are handled in a way that Thunderbird/Enigmail recognize; we also introduced a new keyword to remove a fingerprint from a subscription, and protected X-SET-FINGERPRINT better against accidental mis-use. Additionally the pseudoheaders now fully depend on the 'headers_to_meta' configuration-option, including 'sig' and 'enc'; in effect you can now also have a fully empty pseudoheaders-block, which might come in handy for newsletters. Thanks to all contributors! Packages for Debian and CentOS will be released soon. For all details please see the [changelog](https://0xacab.org/schleuder/schleuder/blob/master/CHANGELOG.md#330-2018-09-04).

<span class='date'>2018-05-14</span>: **Schleuder 3.2.3 released!** This release fixes some bugs, e.g. with emails sent by Thunderbird/Enigmail with "protected subjects" (they are not leaked anymore and keywords can be found again). Also error messages are converted into human readable text now, instead of giving their class-name. And we moved the project to our own domain, schleuder.org! For all details please see the [changelog](https://0xacab.org/schleuder/schleuder/blob/master/CHANGELOG.md#323-2018-05-14). Packages for Debian and CentOS will follow as soon as possible. *This release is **not** related to "efail" (a disclosure of vulnerabilities in email-programs that handle encrypted HTML-messages).*


*Older news can be found in the [archive of the announce-mailinglist](https://lists.nadir.org/pipermail/schleuder-announce).*
</div>

{% include footer.md project="schleuder" %}

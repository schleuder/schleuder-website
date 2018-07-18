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

<span class='date'>2018-07-18</span>: **Vulnerability in dependency of schleuder-web.** Anyone running schleuder-web should update the gem sprockets by running "bundle update sprockets" as soon as possible. (See [CVE-2018-3760](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-3760) for details.)

<span class='date'>2018-05-14</span>: **Schleuder 3.2.3 released!** This release fixes some bugs, e.g. with emails sent by Thunderbird/Enigmail with "protected subjects" (they are not leaked anymore and keywords can be found again). Also error messages are converted into human readable text now, instead of giving their class-name. And we moved the project to our own domain, schleuder.org! For all details please see the [changelog](https://0xacab.org/schleuder/schleuder/blob/master/CHANGELOG.md#323-2018-05-14). Packages for Debian and CentOS will follow as soon as possible. *This release is **not** related to "efail" (a disclosure of vulnerabilities in email-programs that handle encrypted HTML-messages).*

<span class='date'>2018-03-28</span>: **Vulnerability in dependencies of schleuder-web.** Anyone running schleuder-web should update the gems "loofah" and "rails-html-sanitizer" by running "bundle update loofah rails-html-sanitizer" as soon as possible. (See [CVE-2018-8048](https://github.com/flavorjones/loofah/issues/144) and [CVE-2018-3741](https://hackerone.com/reports/328270) for details.)


*Older news can be found in the [archive of the announce-mailinglist](https://lists.nadir.org/pipermail/schleuder-announce).*
</div>

{% include footer.md project="schleuder" %}

---
layout: home
title: Schleuder
subtitle: A gpg-enabled mailing list manager with remailing-capabilities.
---

{::options parse_block_html="true" /}

Schleuder is a group's email-gateway: subscribers can exchange encrypted emails among themselves, receive emails from non-subscribers and send emails to non-subscribers via the list. [How it works](schleuder/docs/concept.html)

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

<span class='date'>2018-03-28</span>: **Vulnerability in dependencies of schleuder-web.** Anyone running schleuder-web should update the gems "loofah" and "rails-html-sanitizer" by running "bundle update loofah rails-html-sanitizer" as soon as possible. (See [CVE-2018-8048](https://github.com/flavorjones/loofah/issues/144) and [CVE-2018-3741](https://hackerone.com/reports/328270) for details.)

<span class='date'>2018-02-19</span>: **Linux-packages for Schleuder 3.2.2 available.** For Debian (stretch-backports) and CentOS (EL 7) there are now packages of Schleuder version 3.2.2 available to easily install and upgrade it. Please see the [installation instructions](https://schleuder.org/docs/#installation) for details on how to use the packages. For details about version 3.2.2 please read the [changelog](https://0xacab.org/schleuder/schleuder/blob/master/CHANGELOG.md#322-2018-02-06).

<span class='date'>2018-02-06</span>: **Schleuder 3.2.2 released!** This release fixes some minor bugs, e.g. with lingering dirmngr-processes, parsing keywords from big messages, and OpenPGP-keys with non-ASCII-characters. It also pins the "mail"-library to version 2.6, because 2.7 seems to have problems. For all details please see the [changelog](https://0xacab.org/schleuder/schleuder/blob/master/CHANGELOG.md#322-2018-02-06).

*Older news can be found in the [archive of the announce-mailinglist](https://lists.nadir.org/pipermail/schleuder-announce).*
</div>

{% include footer.md project="schleuder" %}

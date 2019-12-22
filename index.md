---
layout: home
title: Schleuder
subtitle: A gpg-enabled mailing list manager with remailing-capabilities.
---

{::options parse_block_html="true" /}

Schleuder is a group's email-gateway: subscribers can exchange encrypted emails among themselves, receive emails from non-subscribers and send emails to non-subscribers via the list. [How it works](schleuder/docs/concept.html)

We give our time and knowledge to build and maintain this project in order to help people with their daily private communication and in the struggle for their personal emancipation, social and economic justice and political freedom. [Why we do it](MISSION_STATEMENT.html)

<div class='row'>
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
* [schleuder-gitlab-ticketing](schleuder-gitlab-ticketing)
</div>
</div>


<div class='block' id='news'>
### News

<span class='date'>2019-12-22</span>: **Meet Schleuder devs in Berlin**. For everyone located in or near Berlin, Germany (or willing to travel): On Friday, Jan 3rd, the Schleuder development team will meet in a restaurant in Berlin, and would be pleased to have a chat with you! We have no schedule, just time to sit around, discuss ideas, ask questions, and have some drinks or food. Anyone is welcome, be they technically minded, politically interested or simply curious! *Friday, 2020-01-03, from 7pm (local time), at Kuchen Kaiser* (Oranienplatz 11-13, Berlin, Germany).

<span class='date'>2019-09-16</span>: **Schleuder 3.4.1 released!** This release fixes two problems with the handling of emails containing "protected headers", and it drops third-party signatures on OpenPGP keys when importing to prevent signature flooding (if your version of GnuPG is recent enough). For all details please see the [changelog](https://0xacab.org/schleuder/schleuder/blob/master/CHANGELOG.md#341-2019-09-16).

<span class='date'>2019-02-21</span>: **Schleuder Gitlab Ticketing 1.0.0 released!** Since Schleuder 3.3.0 it is possible to extend the schleuder filters through externally provided filters. A project that is using this mechanism is the [schleuder-gitlab-ticketing](schleuder-gitlab-ticketing) project, that provides tracking functionality of your schleuder messaging within a gitlab issue tracker. This is especially built and helpful for schleuder lists being used as helpdesk or project gateway. Read more about the functionality on the projects summary page: [schleuder-gitlab-ticketing](schleuder-gitlab-ticketing).

<span class='date'>2019-02-14</span>: **Schleuder 3.4.0 released!** This release ships an important privacy fix: To not leak keywords (for example via resent messages), HTML is now stripped from multipart/alternative messages if they contain keywords. For all details please see the [changelog](https://0xacab.org/schleuder/schleuder/blob/master/CHANGELOG.md#340-2019-02-14).

<span class='date'>2019-01-24</span>: **Meet Schleuder devs in Frankfurt/Main**. For everyone located in or near Frankfurt/Main, Germany (or willing to travel): On Friday, Feb 1st, the Schleuder development team will meet in a restaurant in Frankfurt, and would be pleased to have a chat with you! We have no schedule, just time to sit around, discuss ideas, ask questions, and have some drinks or food. Anyone is welcome, be they technically minded, politically interested or simply curious! *Friday, 2019-02-01, from 7pm (local time), at Heck Meck* (Friesengasse 19, Frankfurt am Main, Germany).


*Older news can be found in the [archive of the announce-mailinglist](https://lists.nadir.org/pipermail/schleuder-announce).*
</div>

{% include footer.md project="schleuder" %}

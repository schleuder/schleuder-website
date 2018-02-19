---
layout: page
title: Schleuder
subtitle: A gpg-enabled mailinglist with remailing-capabilities.
---

Schleuder is a gpg-enabled mailing list manager with resending-capabilities. Subscribers can communicate encrypted (and pseudonymously) among themselves, receive emails from non-subscribers and send emails to non-subscribers via the list.

To **read** the documentation see [docs](docs/).

To **install** the current version see [installation](docs/#installation).

To **participate** in the development use the [issue tracker](https://0xacab.org/schleuder/schleuder/issues). Please take note of our [Code of Conduct](https://0xacab.org/schleuder/schleuder/blob/master/CODE_OF_CONDUCT.md).

To **contact** us read [contact](contact.html).

To **be notified** of news about Schleuder subscribe to [schleuder-announce](https://lists.nadir.org/mailman/listinfo/schleuder-announce).

## News

2018-02-19: **Linux-packages for Schleuder 3.2.2 available.** For Debian (stretch-backports) and CentOS (EL 7) there are now packages of Schleuder version 3.2.2 available to easily install and upgrade it. Please see the [installation instructions](https://schleuder.nadir.org/docs/#installation) for details on how to use the packages. For details about version 3.2.2 please read the [changelog](https://0xacab.org/schleuder/schleuder/blob/master/CHANGELOG.md#322-2018-02-06).

2018-02-06: **Schleuder 3.2.2 released!** This release fixes some minor bugs, e.g. with lingering dirmngr-processes, parsing keywords from big messages, and OpenPGP-keys with non-ASCII-characters. It also pins the "mail"-library to version 2.6, because 2.7 seems to have problems. For all details please see the [changelog](https://0xacab.org/schleuder/schleuder/blob/master/CHANGELOG.md#322-2018-02-06).

2017-10-24: **Schleuder 3.2.1 released!** This release adds an explicit dependency on ruby-gpgme >= 2.0.13, which includes a fix for a bug people have run into. Please see the [changelog of schleuder](https://0xacab.org/schleuder/schleuder/blob/master/CHANGELOG.md#321-2017-10-24) for details.

2017-10-23: **Schleuder 3.2 released!** This release adds two new features: An internal footer to be appended to each mail sent to a subcribed address, and an option to use an OS-wide defined keyserver (useful on Debian 9). On top, two new keywords: `X-RESEND-UNENCRYPTED` and `X-RESEND-CC-UNENCRYPTED` will send out mails in the clear, regardless of whether relevant keys exist. Additionally, some bugs were fixed. Don't forget to **run `schleuder install`** after the update to apply a necessary database migration. Please see the [changelog of schleuder](https://0xacab.org/schleuder/schleuder/blob/master/CHANGELOG.md#320-2017-10-23) for details.


Older news can be found in the [archive of the announce-mailinglist](https://lists.nadir.org/pipermail/schleuder-announce/).

---
layout: page
title: Schleuder
subtitle: A gpg-enabled mailinglist with remailing-capabilities.
---

Schleuder is a gpg-enabled mailing list manager with resending-capabilities. Subscribers can communicate encrypted (and pseudonymously) among themselves, receive emails from non-subscribers and send emails to non-subscribers via the list.

To **read** the documentation see [docs](docs/).

To **install** the current version see [the README](https://0xacab.org/schleuder/schleuder/blob/master/README.md).

To **participate** in the development use the [issue tracker](https://0xacab.org/schleuder/schleuder/issues). Please take note of our [Code of Conduct](https://0xacab.org/schleuder/schleuder/blob/master/CODE_OF_CONDUCT.md).

To **contact** us read [contact](contact.html).

To **be notified** of news about Schleuder subscribe to [schleuder-announce](https://lists.nadir.org/mailman/listinfo/schleuder-announce).

## News

2017-10-23: **Schleuder 3.2 released!** This release adds two new features: An internal footer to be appended to each mail sent to a subcribed address, and an option to use an OS-wide defined keyserver (useful on Debian 9). On top, two new keywords: `X-RESEND-UNENCRYPTED` and `X-RESEND-CC-UNENCRYPTED` will send out mails in the clear, regardless of whether relevant keys exist. Additionally, some bugs were fixed. Don't forget to **run `schleuder install`** after the update to apply a necessary database migration. Please see the [changelog of schleuder](https://0xacab.org/schleuder/schleuder/blob/master/CHANGELOG.md#320-2017-10-23) for details.

2017-07-24: **Schleuder-cli 0.1.0 released!** This release adds a new sub-command (`lists send-list-key-to-subscriptions`), and changes one other. Please see the [changelog of schleuder-cli](https://0xacab.org/schleuder/schleuder-cli/blob/master/CHANGELOG.md#010-2017-07-21) for details.

2017-07-13: **Schleuder 3.1.2 released!** Several bugs were fixed. Please see the [changelog of schleuder](https://0xacab.org/schleuder/schleuder/blob/master/CHANGELOG.md#312-2017-07-13) for details.


Older news can be found in the [archive of the announce-mailinglist](https://lists.nadir.org/pipermail/schleuder-announce/).

##  Legacy: version 2

For the legacy (old) version of Schleuder see [schleuder2.nadir.org](https://schleuder2.nadir.org/).


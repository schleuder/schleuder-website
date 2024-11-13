---
title: Concept
---

{% include docs-head.md version=5.0 %}

Schleuder is an OpenPGP-enabled mailing list manager with resending capabilities. This document tries to explain what that means.


## An email hub for groups

Schleuder enables subscribers to communicate encryptedly and pseudonymously. It takes care of all de- and encryption, stripping of headers, formatting conversions, etc.

But Schleuder can also receive and send emails to non-subscribers, which makes it a useful email hub for groups.

Incoming emails from non-subscribers ("outside people") are (re)encrypted and distributed among the subscribers, which can communicate internally as well as answer to the outside person via the list.

The outside person only sees an email address with a public key, but has no information about who is behind it.

Here's a simple picture of a message that is sent to a non-subscriber ("Zacharias"). It illustrates how Alice, Bob, Claire and David are invisible to Zacharias (click image to enlarge it).

[![resent-schema](schleuder-schema-resend-small.png 'Click to enlarge')](schleuder-schema-resend.png)

Additionally, it is possible to combine a list being used as a helpdesk to be combined with a Gitlab issue tracker. To learn more about that, have a look at the [project description](../../schleuder-gitlab-ticketing/).


### A helpful bot

Additionally, Schleuder can send out its own public key upon request by email. Anyone sending a message to `listname-sendkey@hostname` will receive a reply that contains the public key of that list.

And Schleuder can receive administrative commands by email. E.g. it is possible by email to add an OpenPGP-key to a list in order to reply encryptedly to an outside person.


## Technical details

### A "wanted machine-in-the-middle"

Basically Schleuder is a "wanted machine-in-the-middle".

Each list has its own key pair. Schleuder decrypts every incoming email and verifies its signature with the keys from the list's keyring. Then Schleuder loops over the list of subscribers, creates for each a stripped down copy of the message, encrypts it with the subscriber's key and signs it with its own key, and sends it out.

Schleuder inserts some lines of metadata into the top of the email, containing a (configurable) copy of some of the original headers and the result of the decryption and verification of the incoming email. Here's an example:


    From: Bob <bob@example.net>
    To: team@schleuder.org
    Date: Tue, 6 Apr 2010 17:28:46 +0200
    Enc: encrypted
    Sig: Good signature from 12345678DEADBEEF Bob <bob@example.net>

The consequence of this approach is that you need to *really trust* the provider that runs Schleuder: they could store and decrypt all emails that pass the lists, if they wanted.


### Internal workings

Schleuder behaves like an email-filter: it reads email from standard-input, and reports errors to standard-error. If all goes well Schleuder closes the initial connection to the Mail Transport Agent (MTA) only after it sent out all outgoing emails.

In case of an error the MTA includes Schleuder's error message into a bounce-email that is sent back to the sender.

The keyrings for each list are standard GnuPG keyrings and sit in the filesystem under `$lists_dir/$hostname/$listname/` ($lists_dir is read from `schleuder.yml`, see [Configuration](#configuration)). They can be used manually using gpg2. Please be careful to maintain proper file permissions if you touch the files.

In the list-directory there's also a list specific log-file (might be missing if the log-level is high and no error occurred yet).

Other logging is sent to syslog. Where that ends up depends on the operating system and the system administration.

All other list-related data is stored in the SQL-database. Most data is unserialized, only some values are JSON-encoded.

---
layout: docs
title: Documentation
---

{: .note}
This document refers to **Schleuder v3.** To read about Schleuder v2 please go to the [old site](https://schleuder2.nadir.org/documentation.html).

{: .note}
Disclaimer: this is work in progress. To suggest improvements see [Feedback](#feedback).

* toc
{:toc}

## Introduction

Schleuder is an email hub for groups. Subscribers can communicate encryptedly and pseudonymously among themselves, receive emails from non-subscribers and send emails to non-subscribers via the list. Schleuder takes care of all de- and encryption, stripping of headers, formatting conversions, etc. Further Schleuder can send out its own public key and receive administrative commands by email.

### A "wanted man-in-the-middle"

Basically Schleuder is a "wanted man-in-the-middle".

Each list has its own keypair. Schleuder decrypts every incoming email and verifies its signature with the keys from the list's keyring. Then Schleuder loops over the list of subscribers, creates for each a stripped down copy of the message, encrypts it with the subscriber's key and signs it with its own key, and sends it out.

Schleuder inserts some lines of metadata into the top of the email, containing a (configurable) copy of some of the original headers and the result of the decryption and verification of the incoming email. Here's an example:


    From: Bob <bob@example.net>
    To: schleuder@nadir.org
    Date: Tue, 6 Apr 2010 17:28:46 +0200
    Enc: encrypted
    Sig: Good signature from 12345678DEADBEEF Bob <bob@example.net>

The consequence of this approach is that you need to *really trust* the provider that runs Schleuder: they could store and decrypt all emails that pass the lists, if they wanted.


### An email hub for groups

Schleuder can receive and send emails to non-subscribers, which makes it a useful email hub for groups.

Incoming emails from non-subscribers are (re)encrypted and distributed among the subscribers, which can communicate internally as well as answer to the outside person via the list.

The outside person only sees an email address with a public key, but has no information about who is behind it.

Here's a simple picture of a message that is sent to a non-subscriber ("Zacharias"). It illustrates how Alice, Bob, Claire and David are invisible to Zacharias (click image to enlarge it).

[![resent-schema](schleuder-schema-resend-small.png 'Click to enlarge')](schleuder-schema-resend.png)


### Technical details

Schleuder behaves like an email-filter: it reads email from standard-input, and reports errors to standard-error. If all goes well Schleuder closes the initial connection to the Mail Transport Agent (MTA) only after it sent out all outgoing emails.

In case of an error the MTA includes Schleuder's error message into a bounce-email that is sent back to the sender.

The keyrings for each list are standard GnuPG keyrings and sit in the filesystem under `$lists_dir/$hostname/$listname/` ($lists_dir is read from `schleuder.yml`, see [Configuration](#configuration)). They can be used manually using gpg2. Please be careful to maintain proper file permissions if you touch the files.

In the list-directory there's also a list specific log-file (might be missing if the log-level is high and no error occurred yet).

Other logging is sent to syslog. Where that ends up depends on the operating system and the system administration.

All other list-related data is stored in the SQL-database. Most data is unserialized, only some values are JSON-encoded.

[↑](#top "Go to top of page")
{: .linktotop}

## Setup

### Installation

For install instructions please see the [README](https://git.codecoop.org/schleuder/schleuder3/blob/master/README.md) of Schleuder.

As suggested there you should also install [SchleuderConf](https://git.codecoop.org/schleuder/schleuder-conf), which is a command line tool to manage Schleuder lists.


### Configuration

Schleuder reads its **basic settings** from a file that it by default expects at `/etc/schleuder/schleuder.yml`. To make Schleuder read a different file set the environment variable `SCHLEUDER_CONFIG` to the path to your file when running schleuder. E.g.:

    SCHLEUDER_CONFIG=/usr/local/etc/schleuder.yml /path/to/bin/schleuder ...

For explanations of the possible settings read the default config file (also [available in the repository](https://git.codecoop.org/schleuder/schleuder3/blob/master/etc/schleuder.yml)).

The **default settings for new lists** are read from another config file. By default Schleuder looks at `/etc/schleuder/list-defaults.yml`. To make Schleuder read a different file set the environment variable `SCHLEUDER_LIST_DEFAULTS` analogous to above. The possible settings are explained in the default config file, which is [also available in the repository](https://git.codecoop.org/schleuder/schleuder3/blob/master/etc/list-defaults.yml).


### Hook into Mail Transport Agent

In "work"-mode, Schleuder expects the list's email-address as second argument (first one is "work") and the incoming email on standard-input.

To enable Schleuder to receive emails, your Mail Transport Agent must be configured accordingly. How to do this with Postfix is documented in detail below.

#### Postfix

To hook Schleuder into Postfix adapt these lines and add them to `master.cf`:

    schleuder  unix  -       n       n       -       -       pipe
      flags=DRhu user=schleuder argv=/path/to/bin/schleuder work ${recipient}

Also adapt these lines and add them to `main.cf`:

    transport_maps = hash:/etc/postfix/transport
    local_recipient_maps = proxy:unix:passwd.byname $alias_maps $transport_maps
    schleuder_destination_recipient_limit = 1

Then you have to configure which addresses should be connected to Schleuder via `transport`. If you have a whole (sub)domain dedicated to Schleuder you can just list it like this:

    @sub.example.org          schleuder:

To configure each list individually adapt the following lines for each list and add them to `transport`:

    listname@example.org          schleuder:
    listname-request@example.org  schleuder:
    listname-owner@example.org    schleuder:
    listname-bounces@example.org  schleuder:
    listname-sendkey@example.org  schleuder:


### Schleuder API

The Schleuder API is provided by `schleuder-api-daemon`. Configuration clients (Webschleuder, SchleuderConf) use it to access information about lists, subscriptions, and keys. As you probably want to at least use SchleuderConf from localhost, setting up schleuder-api-daemon is useful even without remote clients.

{: .note}
Schleuder does **not** use schleuder-api-daemon to process emails.

To run `schleuder-api-daemon`, depending on the type of operating system and the setup you are using, you can either start the systemd-unit-file:

    systemctl start schleuder-api-daemon

Or you can run it manually in a shell:

    schleuder-api-daemon

{: .note}
Please take care to run `schleuder-api-daemon` as the user that owns your the directory of schleuder lists (by default `/var/schleuder/lists`) to avoid running into file permission problems!

#### Transport encryption

By default schleuder-api-daemon **listens only to localhost** and does not authenticate requests. To enable it listen to other addresses, you have to provide transport encryption (TLS) certificates.

You can **generate a new certificate** by executing:

    schleuder cert generate

If the file systems permissions allow it, Schleuder will write the certificate and the key directly into the correct files (paths are read from the configuration file). Otherwise you might have to move them. Please read the output of the above command for possible instructions.

In case you **already have a suitable certificate** you can use that, too. Its hostnames do not matter. Just copy it to the paths specified in the configuration file, or change those paths.

{: .note}
In order to verify the connection, each client needs to know the fingerprint of the API-certificate. Use secure channels to transport this information!



#### Authentication

The Schleuder API uses API-keys to authenticate clients — if transport encryption is enabled *(and only if).*

You can generate new API-keys by executing:

    schleuder new_api_key

To enable the client to connect, their API-key must be added to the section `valid_api_keys` in Schleuder's configuration file.

{: .note}
Provide each client with their own API-key, and use secure channels to transport this information!

{: .note}
The is **no authorization of clients,** yet. Each client is allowed every action. So be wary who to give an API-key to. Webschleuder does its own authorization, but SchleuderConf does not!



[↑](#top "Go to top of page")
{: .linktotop}

## Managing a list

To create and manage lists you have two options: Webschleuder and SchleuderConf.

Both require a running `schleuder-api-daemon`. Please see [the previous section](#schleuder-api) on how to set that up.


### Webschleuder

To create lists with Webschleuder log in as `root@localhost`. Managing lists is allowed to each list-admin.

### SchleuderConf

To use SchleuderConf please see the output of

    schleuder-conf help


[↑](#top "Go to top of page")
{: .linktotop}

## Using a list


Everything you send to `listname@hostname` will be send to all subscribers, but they will see only certain headers and the body of your email. The selection of these headers can be configured for each list individually by the list-admins.

### Getting a list's public key

Each Schleuder-list replies with its public key to any email sent to `listname-sendkey@hostname`. E.g. to receive the key for our contact address write an email to `schleuder-sendkey@nadir.org`.

### Special keywords

Schleuder knows some special keywords that trigger different behaviour. You can e.g. subscribe someone, or resend an email to a non-subscriber using keywords. See a list of available keywords below.

Keywords require that:

* they start the line and begin with "x-",
* they are written into the beginning of the *first text-part* of the email (usually that's just the normal body of the email),
* possible arguments must be written *on the same line* as the keyword (exceptions are mentioned in the descriptions below),
* the email must be *encrypted and signed* by a list-member's key.

Keywords can be repeated within one email at will.
Letter case doesn't matter.

There are two types of keywords: those to enhance messages sent over the list ("list-keywords"), and those to request something from Schleuder ("request-keywords").


#### Resending

The resending-keywords must be included in messages sent to the normal list-address: `listname@hostname`.

x-resend: someone@example.org
: Send the message to the given address, encrypted if possible, otherwise in the clear.

x-resend-encrypted-only: someone@example.org
: Send the message to the given address only if it could be encrypted.


#### Subscription and key management

These keywords  must be send to `listname-request@hostname`. They are used to get information about the list, its subscribers and keys, or to change that information.

x-list-subscriptions
: List all subscriptions.

x-subscribe: person@example.org 0x12345678DEADBEEF12345678DEADBEEF12345678
: Subscribe the given address and assign it the given OpenPGP-fingerprint.

x-unsubscribe: person@example.org
: Unsubscribe the given address.

x-set-fingerprint: 0x12345678DEADBEEF12345678DEADBEEF12345678
: Assign the key with the given fingerprint to your subscription.

x-set-fingerprint: person@example.org  0x12345678DEADBEEF12345678DEADBEEF12345678
: Assign the key with the given fingerprint to the given subscription. This variant of this command may only be used by list-admins.


x-list-keys:
: Lists all public keys known to the list.

x-add-key:
: Import the rest of the email-body into the list's keyring. Write the ascii-armored key-block below this keyword. — *Note:* This command reads the whole body of the email. If you want to use other keywords within the same email, *this must be the last one!*

x-delete-key: 0x12345678DEADBEEF12345678DEADBEEF12345678
: Delete the key with the given fingerprint from the list's keyring.

x-get-key: 0x12345678DEADBEEF12345678DEADBEEF12345678
: Export the key with the given fingerprint from the list's keyring.

x-fetch-key: 0x12345678DEADBEEF12345678DEADBEEF12345678
: Fetch the key with the given fingerprint from a keyserver and import it into the list's keyring. (This works only if a keyserver has been configured by the provider.)


#### Other

These must also be sent to the request-address: <listname-request@hostname>.

x-sign-this:
: Sign the remaining contents of the email body or the attachments with the list's key. Use this e.g. to provide GnuPG-signatures for things you are publishing. — *Note:* This command reads the whole body of the email. If you want to use other keywords within the same email, *this must be the last one!*


### Contact list-owner

Write to `listname-owner@hostname` to contact the list-owner(s) even if you don't know who they are. Use the list's key to encrypt the email!


[↑](#top "Go to top of page")
{: .linktotop}

## Maintenance

{: .note}
Please take care to have the following commands run by the user that owns your the directory of schleuder lists (by default `/var/schleuder/lists`) to avoid running into file permission problems!


Schleuder can **check all keys** that are present in the list's keyrings for (upcoming) expiration dates, revocation, or other reasons for not being usable.

Call this command weekly from cron to automate the check and have the results sent to the respective list-admins:

    schleuder check_keys

To only check the keys of one particular list, run:

    schleuder check_keys list@hostname


Schleuder can also **refresh all keys** in the same manner:

    schleuder refresh_keys

To run only for one particular list:

    schleuder refresh_keys list@hostname

Alternatively to using Schleuder for refreshing keys you could set up more sophisticated tools like [parcimonie](https://gaffer.ptitcanardnoir.org/intrigeri/code/parcimonie/), [parcimonie.sh](https://github.com/EtiennePerot/parcimonie.sh) or [GPG maintenance](https://github.com/ilf/gpg-maintenance/).


[↑](#top "Go to top of page")
{: .linktotop}


## Feedback

To suggest changes to these documents or if you found any errors in them, please [contact us](mailto:schleuder@nadir.org) or use the [issue tracker](https://git.codecoop.org/schleuder/schleuder3/issues).

[↑](#top "Go to top of page")
{: .linktotop}


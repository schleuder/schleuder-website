---
layout: docs
title: Documentation
---

{: .note}
This document refers to **Schleuder version 3.2** To read about older versions of Schleuder please see the [older docs](older/).

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

For install instructions please see the [README](https://0xacab.org/schleuder/schleuder/blob/master/README.md) of Schleuder.

As suggested there you should also install [schleuder-cli](https://0xacab.org/schleuder/schleuder-cli), which is a command line tool to manage Schleuder lists.


### Configuration

Schleuder reads its **basic settings** from a file that it by default expects at `/etc/schleuder/schleuder.yml`. To make Schleuder read a different file set the environment variable `SCHLEUDER_CONFIG` to the path to your file when running schleuder. E.g.:

    SCHLEUDER_CONFIG=/usr/local/etc/schleuder.yml /path/to/bin/schleuder ...

For explanations of the possible settings read the default config file (also [available in the repository](https://0xacab.org/schleuder/schleuder/blob/master/etc/schleuder.yml)).

The **default settings for new lists** are read from another config file. By default Schleuder looks at `/etc/schleuder/list-defaults.yml`. To make Schleuder read a different file set the environment variable `SCHLEUDER_LIST_DEFAULTS` analogous to above. The possible settings are explained in the default config file, which is [also available in the repository](https://0xacab.org/schleuder/schleuder/blob/master/etc/list-defaults.yml).

Once a list is created, it is not affected by these configuration files any more. Existing lists have their configuration stored in the database. The settings in the database can be shown and set via the schleuder API, available through schleuder-web or schleuder-cli. Run `schleuder-cli lists help` and `schleuder-cli lists list-options` for more information on the latter.

### Hook into Mail Transport Agent

In "work"-mode, Schleuder expects the list's email-address as second argument (first one is "work") and the incoming email on standard-input.

To enable Schleuder to receive emails, your Mail Transport Agent must be configured accordingly. How to do this with Postfix is documented in detail below.

#### Postfix

{: .note}
This section describes only those parts of a Postfix-setup that are relevant to Schleuder. We assume that you have a sensible and tested Postfix-setup already.

Firstly, to hook Schleuder into Postfix adapt these lines (path and maybe user) and add them to `master.cf`:

    schleuder  unix  -       n       n       -       -       pipe
      flags=DRhu user=schleuder argv=/path/to/bin/schleuder work ${recipient}

Then you have to chose how postfix should decide if a message should be delivered to Schleuder. There are two options:

1. Configure it for each list individually. That's the way to go if you don't run many lists, or use the respective domain also for a varying number of email accounts or aliases.
2. Dedicate a whole domain to Schleuder. That's the way to go if you run more lists than email accounts or aliases on that domain.


**To configure each list individually,** add these lines to `main.cf`:

    schleuder_destination_recipient_limit = 1
    transport_maps = hash:/etc/postfix/transport_schleuder

Now adapt the following lines for each list and add them to `/etc/postfix/transport_schleuder`:

    listname@example.org          schleuder:
    listname-request@example.org  schleuder:
    listname-owner@example.org    schleuder:
    listname-bounce@example.org   schleuder:
    listname-sendkey@example.org  schleuder:

Afterwards run `postmap /etc/postfix/transport_schleuder` and restart postfix. Remember to repeat this also for newly created lists later.


**To dedicate a whole domain to Schleuder,** add these lines to `main.cf`:

    schleuder_destination_recipient_limit = 1
    virtual_mailbox_domains = lists.example.org
    virtual_transport       = schleuder
    virtual_alias_maps      = hash:/etc/postfix/virtual_aliases
    virtual_mailbox_maps    = sqlite:/etc/postfix/schleuder_sqlite.cf

Then adapt and add at least the following exceptions from the All-to-Schleuder-rule to `/etc/postfix/virtual_aliases`:

    postmaster@lists.example.org    root@anotherdomain
    abuse@lists.example.org         root@anotherdomain
    MAILER-DAEMON@lists.example.org root@anotherdomain
    root@lists.example.org          root@anotherdomain

Afterwards run `postmap /etc/postfix/virtual_aliases`.

From now on each Schleuder-list will instantly be reachable by email once it was created.

#### Exim

{: .note}
This section describes only those parts of a Exim-setup that are relevant to Schleuder. We assume that you have a sensible and tested Exim-setup already.

As with any exim email routing, we need to configure essentially a router that accepts and directs a mail to a transport, which knows how to hand-over an email to schleuder.

Within the `begin routers` section of your `exim.conf` you can add the following router:

    mlschleuder:
      driver = accept
      condition = ${lookup {$local_part@$domain}lsearch{/etc/exim/schleuder-lists} {yes}{no}}
      local_part_suffix_optional
      local_part_suffix = +* : -bounce : -sendkey : -request : -owner
      transport = mlschleuder_transport_local
      no_more

`/etc/exim/schleuder-lists` is a simple textfile containing one list-address per line. You can for example create it by executing `schleuder-cli lists list > /etc/exim/schleuder-lists` after creating or deleting any lists. In more advanced setups you might have different conditions depending on how you manage the inventory of your schleuder lists and decide to accept a mail for a list.

Within the `begin transports` section of your `exim.conf` you then configure the transport:

    mlschleuder_transport_local:
       driver = pipe
       user = schleuder
       group = schleuder
       # schleuders generates nice log messages for some of the problems
       return_fail_output = true
       home_directory = /var/lib/schleuder/lists/$domain/$local_part
       command = "/usr/bin/schleuder work $local_part$local_part_suffix@$domain"
       message_size_limit = 10M

Please note that we keep the `$local_part_suffix` when handing the mail over to schleuder, so schleuder can e.g. detect bounces or sendkey emails properly.

Restart exim and you have your working schleuder+exim setup.

Remember to repeat dumping the list of schleuder-lists to `/etc/exim/schleuder-lists` also for newly created lists later.

### Schleuder API

The Schleuder API is provided by `schleuder-api-daemon`. Configuration clients (schleuder-web, schleuder-cli) use it to access information about lists, subscriptions, and keys. As you probably want to at least use schleuder-cli from localhost, setting up schleuder-api-daemon is useful even without remote clients.

{: .note}
Schleuder does **not** use schleuder-api-daemon to process emails. You can stop schleuder-api-daemon at any time without breaking the email flow.

To run `schleuder-api-daemon`, depending on the type of operating system and the setup you are using, you can either start the systemd-unit-file:

    systemctl start schleuder-api-daemon

Or you can run it manually in a shell:

    schleuder-api-daemon

{: .note}
Please take care to run `schleuder-api-daemon` as the user that owns the directory of schleuder lists (by default `/var/lib/schleuder/lists`) to avoid running into file permission problems!

#### Transport encryption

schleuder-api-daemon uses transport encyrption (TLS) for all connections. The required TLS-certifcates should have been generated during the setup (`schleuder install`). You can generate new ones at any time by executing:

    schleuder cert generate

If the file systems permissions allow it, Schleuder will write the certificate and the key directly into the correct files (paths are read from the configuration file). Otherwise you might have to move them. Please read the output of the above command for possible instructions.

In case you already have a suitable certificate you can use that, too. Its hostnames do not matter. Just copy it to the paths specified in the configuration file, or change those paths.

In order to verify the connection, each client needs to know the fingerprint of the API-certificate. The fingerprint will be shown when generating the certificates. Later you can always have it show again by executing this:

    schleuder cert fingerprint

{: .note}
Use secure channels to transport this information!



#### Authentication

The Schleuder API uses API-keys to authenticate clients.

You can generate new API-keys by executing:

    schleuder new_api_key

To enable the client to connect, their API-key must be added to the section `valid_api_keys` in Schleuder's configuration file.

{: .note}
Provide each client with their own API-key, and use secure channels to transport this information!

{: .note}
There is **no authorization of clients,** yet. Each client is allowed every action. So be wary who to give an API-key to. schleuder-web does its own authorization, but schleuder-cli does not!



[↑](#top "Go to top of page")
{: .linktotop}

## Managing a list

To create and manage lists you have two options: schleuder-web and schleuder-cli.

Both require a running `schleuder-api-daemon`. Please see [the previous section](#schleuder-api) on how to set that up.


### schleuder-web

To create lists with schleuder-web log in as `root@localhost`. Managing lists is allowed to each list-admin.

### schleuder-cli

To use schleuder-cli please see the output of

    schleuder-cli help


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


#### Security

x-list-name: someone@example.org
: **You must always provide this keyword once per email.** Without it, no other keyword will be considered but you will receive an error message. It helps to mitigate replay-attacks.


#### Resending

The resending-keywords must be included in messages sent to the normal list-address: `listname@hostname`.

x-resend: someone@example.org
: Send the message to the given address, encrypted if possible, otherwise in the clear.

x-resend-encrypted-only: someone@example.org
: Send the message to the given address only if it could be encrypted. Can be abbreviated to `x-resend-enc`.

x-resend-unencrypted: someone@example.org
: Send the message to the given address without encrypting it. You can use this keyword to make schleuder skip looking for a matching key for this address and enforce sending the email out in the clear.

x-resend-cc: someone@example.org anotherperson@example.org
: Send one message to all of the given addresses in Cc, so they get to know of each other (encrypted if possible, otherwise in the clear).

x-resend-cc-encrypted-only: someone@example.org
: Send one message to all of the given addresses in Cc, so they get to know of each other, only if it could be encrypted to all of those addresses. Can be abbreviated to `x-resend-cc-enc.`

x-resend-cc-unencrypted: someone@example.org
: Send one unencrypted message to all of the given addresses in Cc, so they get to know of each other. We skip looking for any key and will just send out the email in the clear.

#### Subscription and key management

These keywords must be send to `listname-request@hostname`. They are used to get information about the list, its subscribers and keys, or to change that information.

x-list-subscriptions
: List all subscriptions.

x-subscribe: person@example.org 0x12345678DEADBEEF12345678DEADBEEF12345678
: Subscribe the given address and assign it the given OpenPGP-fingerprint.

x-unsubscribe: person@example.org
: Unsubscribe the given address.

x-set-fingerprint: 0x12345678DEADBEEF12345678DEADBEEF12345678
: Assign the key with the given fingerprint to your subscription.

x-set-fingerprint: person@example.org 0x12345678DEADBEEF12345678DEADBEEF12345678
: Assign the key with the given fingerprint to the given subscription. This variant of this command may only be used by list-admins.


x-list-keys:
: Lists all public keys known to the list.

x-add-key:
: Import the attachments or the rest of the email-body into the list's keyring. Only ascii-armored keys are supported.

x-delete-key: 0x12345678DEADBEEF12345678DEADBEEF12345678
: Delete the key with the given fingerprint from the list's keyring.

x-get-key: 0x12345678DEADBEEF12345678DEADBEEF12345678
: Export the key with the given fingerprint from the list's keyring.

x-fetch-key: 0x12345678DEADBEEF12345678DEADBEEF12345678
: Fetch the key with the given fingerprint from a keyserver and import it into the list's keyring. (This works only if a keyserver has been configured by the provider.)

This keyword must be send to the normal list-address: `listname@hostname`.

x-attach-listkey:
: Attachs the public key of the list. Probably most useful in combination with x-resend.


#### Other

These must also be sent to the request-address: <listname-request@hostname>.

x-get-logfile:
: Sends the logfile of the list.

x-get-version:
: Returns the version of schleuder.

x-sign-this:
: Sign the remaining contents of the email body or the attachments with the list's key. Use this e.g. to provide GnuPG-signatures for things you are publishing.


### Contact list-owner

Write to `listname-owner@hostname` to contact the list-owner(s) even if you don't know who they are. Use the list's key to encrypt the email!


[↑](#top "Go to top of page")
{: .linktotop}

## Maintenance

{: .note}
Please take care to have the following commands run by the user that owns your the directory of schleuder lists (by default `/var/lib/schleuder/lists`) to avoid running into file permission problems!


Schleuder can **check all keys** that are present in the list's keyrings for (upcoming) expiration dates, revocation, or other reasons for not being usable.

Call this command weekly from cron to automate the check and have the results sent to the respective list-admins:

    schleuder check_keys


Schleuder can also **refresh all keys** in the same manner. Each key of each list will be refreshed from a keyserver one by one. If you're using gpg 2.1, it's possible to configure a TOR onion service to be used as keyserver! See [the config](https://0xacab.org/schleuder/schleuder/blob/master/etc/schleuder.yml) for an example.


Call this command weekly from cron to automate the check and have the results sent to the respective list-admins:

    schleuder refresh_keys



[↑](#top "Go to top of page")
{: .linktotop}

## Changes to the previous version of Schleuder

{: .note}
This list describes changes that users or list-admins of Schleuder should be aware of. For a more technical changelog, please see the [repository](https://0xacab.org/schleuder/schleuder).

User-relevant changes in version 3.2 compared to version 3.1:

* To improve consistency with the English language, `X-LISTNAME` has been renamed to `X-LIST-NAME`. `X-LISTNAME` still works, but will be deprecated, therefore please use `X-LIST-NAME` from now on.
* Lists can have an internal-footer now. It's appended to each message sent to a subscriber (if it's not blank).
* New keywords: `X-RESEND-UNENCRYPTED` and `X-RESEND-CC-UNENCRYPTED`. With them you can enforce that resent-email will be sent in the clear, regardless of whether we would find a key for the recipient or not.
* The short representation of GnuPG keys became more human-friendly. Besides the fingerprint we now show the email-address of the first UID, the generation-date, and (if present) the expiration-date.
* The resend-keywords now check the given arguments to be valid email-addresses, and block resending if any one is found invalid.
* `X-LIST-KEYS` does not require an argument anymore.
* `X-SUBSCRIBE` now handles the combination of space-separated fingerprint and additional arguments (admin-flag, delivery-enabled-flag) correctly.
* `X-RESEND` now respects the encoding the mail was sent with.
* Fixed broken encoding of certain character-sequences in encrypted+signed messages.


[↑](#top "Go to top of page")
{: .linktotop}

## Feedback

To suggest changes to these documents or if you found any errors in them, please use the [issue tracker](https://0xacab.org/schleuder/schleuder-website/issues) (or [contact us by email](mailto:schleuder@nadir.org)).

[↑](#top "Go to top of page")
{: .linktotop}


## Changes to the documentation

We use a public git-repository to maintain these documents, thus you can [browse the list of changes](https://0xacab.org/schleuder/schleuder-website/commits/master).

[↑](#top "Go to top of page")
{: .linktotop}

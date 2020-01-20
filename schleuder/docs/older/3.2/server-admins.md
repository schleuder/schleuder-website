---
title: Documentation for server-admins
---

{% include docs-head.md version=3.2 %}


## Setup Schleuder

### Installation

You can install schleuder either from Linux distribution packages or rubygems. Currently there are supported distribution packages for Debian ("stretch-backports" and "buster") and CentOS 7. If you use one of the directly supported platforms, you should choose the packages over the gems.

{: .note}
Don't use the packages provided by Ubuntu in all releases up to and including 17.10, they are severely outdated. On Ubuntu 18.04 only use the package if it has at least version 3.2.2.

Besides schleuder you should also install at least one of [schleuder-cli](https://0xacab.org/schleuder/schleuder-cli) (the command line tool to manage Schleuder lists), and [schleuder-web](https://0xacab.org/schleuder/schleuder-web) (the web interface to manage and maintain Schleuder lists).

Additionally we recommend running an entropy source such as `haveged`. This ensures Schleuder won't be blocked by lacking entropy, which otherwise might happen e.g. during key generation.

#### Debian

{: .note}
All steps need root privileges.

We maintain schleuder and schleuder-cli in "stretch-backports" and "buster". (For production usage we recommend Debian "stretch".)

First, add the backports to your APT sources list directory:

    echo "deb http://deb.debian.org/debian stretch-backports main" > /etc/apt/sources.list.d/stretch-backports.list

After this, update the APT package cache:

    apt-get update

Finally, install schleuder and schleuder-cli via:

    apt-get install -t stretch-backports schleuder schleuder-cli

The package will finalize the setup of Schleuder, too.


#### CentOS

For CentOS 7 there is a maintained [copr-repository](https://copr.fedorainfracloud.org/coprs/schleuder/schleuder/) using [Software Collections](https://www.softwarecollections.org/en/).

{: .note}
All steps need root privileges

Install the repository & the SCL repository

    yum install centos-release-scl
    curl -o /etc/yum.repos.d/schleuder-schleuder-epel-7.repo https://copr.fedorainfracloud.org/coprs/schleuder/schleuder/repo/epel-7/schleuder-schleuder-epel-7.repo

Now you are ready to install schleuder and schleuder-cli

    yum install schleuder schleuder-cli

Afterwards run `schleuder install` to finalize the setup of Schleuder. This creates necessary directories, copies example configs, etc. If you see errors about missing write permissions please follow the advice given.

{: .note}
The copr-repository also provides you with a package for [schleuder-web](https://0xacab.org/schleuder/schleuder-web/). Please read the documentation of schleuder-web on how to get it up and running.


#### From Ruby-Gem

For instructions on how to install from rubygems please see the [README](https://0xacab.org/schleuder/schleuder/blob/master/README.md#installing-schleuder) of Schleuder.


#### Puppet

There is a [schleuder puppet module](https://0xacab.org/schleuder/puppet-schleuder) to automate the installation and configuration of schleuder, schleuder-cli, and schleuder-web, and the creation and deletion of schleuder lists. Have a look at its [README](https://0xacab.org/schleuder/puppet-schleuder/blob/master/README.md) on how to use it. Currently it works for CentOS 7, but we would like to make it work for Debian as well - help would be highly appreciated.


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

    foo@example.org          schleuder:
    foo-request@example.org  schleuder:
    foo-owner@example.org    schleuder:
    foo-bounce@example.org   schleuder:
    foo-sendkey@example.org  schleuder:

Afterwards run `postmap /etc/postfix/transport_schleuder` and restart postfix. Remember to repeat this also for newly created lists later.


Another way to tell postfix which domain and list can be piped to schleuder is to get that information out of the sqlite database. A requirement for that is the postfix-sqlite package, which isn't in the standard repositories of CentOS, but Debian.

**To dedicate a whole domain to Schleuder,** add these lines to `main.cf`:

    schleuder_destination_recipient_limit = 1
    virtual_mailbox_domains = sqlite:/etc/postfix/schleuder_domain_sqlite.cf
    virtual_transport       = schleuder
    virtual_alias_maps      = hash:/etc/postfix/virtual_aliases
    virtual_mailbox_maps    = sqlite:/etc/postfix/schleuder_list_sqlite.cf

Then adapt and add at least the following exceptions from the All-to-Schleuder-rule to `/etc/postfix/virtual_aliases`:

    postmaster@lists.example.org    root@anotherdomain
    abuse@lists.example.org         root@anotherdomain
    MAILER-DAEMON@lists.example.org root@anotherdomain
    root@lists.example.org          root@anotherdomain

Afterwards run `postmap /etc/postfix/virtual_aliases`.

The file `schleuder_domain_sqlite.cf` can ask the schleuder sqlite database (this will delegate the whole domain to schleuder):

    dbpath = /var/lib/schleuder/db.sqlite
    query = select distinct substr(email, instr(email, '@') + 1) from lists
            where email like '%%%s'

And the file `schleuder_list_sqlite.cf` can also get the information from the schleuder sqlite database:

    dbpath = /var/lib/schleuder/db.sqlite
    query = select 'present' from lists
              where email = '%s'
              or    email = replace('%s', '-bounce@', '@')
              or    email = replace('%s', '-owner@', '@')
              or    email = replace('%s', '-request@', '@')
              or    email = replace('%s', '-sendkey@', '@')

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

## Managing a list

To create and manage lists you have two options: schleuder-web and schleuder-cli.

Both require a running `schleuder-api-daemon`. Please see [the previous section](#schleuder-api) on how to set that up.


### schleuder-web

To create lists with schleuder-web log in as `root@localhost`. Managing lists is allowed to each list-admin.

### schleuder-cli

To use schleuder-cli please see the output of

    schleuder-cli help

## Maintenance

{: .note}
Please take care to have the following commands run by the user that owns the directory of schleuder lists (by default `/var/lib/schleuder/lists`) to avoid running into file permission problems!


Schleuder can **check all keys** that are present in the list's keyrings for (upcoming) expiration dates, revocation, or other reasons for not being usable.

Call this command weekly from cron to automate the check and have the results sent to the respective list-admins:

    schleuder check_keys


Schleuder can also **refresh all keys** in the same manner. Each key of each list will be refreshed from a keyserver one by one. If you're using gpg 2.1, it's possible to configure a TOR onion service to be used as keyserver! See [the config](https://0xacab.org/schleuder/schleuder/blob/master/etc/schleuder.yml) for an example.


Call this command weekly from cron to automate the check and have the results sent to the respective list-admins:

    schleuder refresh_keys

The available packages for Debian and CentOS both install a weekly cron job that check and refresh keys. Listadmins will be notified about issues with or changes to their keyring.

An additional maintenance command is available that allows you to pin subscriptions to their best matching key. If there is no key assigned, schleuder will try to select a key from the list's keyring that distinctly matches the subscription's email address.

This feature should be used with care. It's easy for a malicious (or inexperienced) person to inject additional user-IDs into the list's keyring. This can lead to situations in which people suddenly receive emails that are encrypted to a key they don't own.

You should better not run this command automatedly, and you should always examine the output closely to check for unintended consequences.

    schleuder pin_keys


{% include participate.md %}


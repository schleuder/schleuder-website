---
layout: post
title:  "Schleuder 5.0.1 released"
date: "2025-06-08"
---

This release ships some bug fixes:

* Fixed code that rescues from failed message delivery to subscribers.
* Since ActiveRecord >= 7.1, the SQLite3 connection adapter relies on `Write-Ahead Logging` (`WAL`), by default, which significantly enhances read and write performance. Such performance gains aren't of concern in Schleuder environments. Besides, Postfix only supports the `delete` journal mode, which Schleuder now enforces.

Please see the [changelog](https://0xacab.org/schleuder/schleuder/blob/main/CHANGELOG.md#501--2025-06-08) for further details.

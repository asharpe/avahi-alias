Avahi-Alias
===========

There are currently no configuration options to set up CNAME mDNS entries with
avahi-daemon.  These scripts are an adaption of the script at
http://www.avahi.org/wiki/Examples/PythonPublishAlias with the following
modification:

* Turn the script into a daemon via Upstart.
* Reads CNAMES from flat files in `/etc/avahi/alias.d`.

Requirements
============

`avahi-alias` requires `python-avahi`. The installation below assumes Upstart
and has been tested on Ubuntu 12.04.

Installation using Upstart
==========================

1. Copy `avahi-alias` to `/usr/sbin/avahi-alias`.
1. Copy `avahi-alias.upstart` to `/etc/init/avahi-alias.conf`

Usage
=====

The script reads all the files in `/etc/avahi/alias.d`. Each non-blank line in
each file is interpreted as a CNAME to register. The names must include the
domain (e.g., `.local`).

Example:

		# mkdir -p /etc/avahi/alias.d
		# echo app1.local >> /etc/avahi/alias.d/apps
		# echo app2.local >> /etc/avahi/alias.d/apps
		# echo db.local >> /etc/avahi/alias.d/services
		# service avahi-alias restart

Your machine now has three has CNAMEs: "app1.local", "app2.local", and "db.local".

Motivation
==========

This script is to inject the equivalent of domain CNAME resource records into
local DNS-SD records for entries.  This allows multiple hostname to resolve to
the current host.  The need for this is because avahi.hosts will automatically
filter out name collisions, and there's currently no means for adding these
values through existing service configuration.

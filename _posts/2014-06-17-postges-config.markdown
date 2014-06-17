---
layout: post
title: "Chef - Server Provisioning Software"
date: 2014-06-17 14:37:00
categories: RailsDeploy
---

I'm trying to run capistrano to deploy a code base. When I run capistrano, it fails to install gem pg because of the following
{% highlight bash %}
	rake aborted!
DEBUG [] 	PG::ConnectionBad: FATAL:  no pg_hba.conf entry for host "0.0.0.0", user "postgres", database "slice", SSL on
DEBUG [] 	FATAL:  no pg_hba.conf entry for host "0.0.0.0", user "postgres", database "slice", SSL off
DEBUG [] 	/home/deploy/slice/shared/bundle/ruby/2.1.0/gems/activerecord-4.0.3/lib/active_record/connection_adapters/postgresql_adapter.rb:831:in `initialize'
DEBUG [] 	/home/deploy/slice/shared/bundle/ruby/2.1.0/gems/activerecord-4.0.3/lib/active_record/connection_adapters/postgresql_adapter.rb:831:in `new'
DEBUG [] 	/home/deploy/slice/shared/bundle/ruby/2.1.0/gems/activerecord-4.0.3/lib/active_record/connection_adapters/postgresql_adapter.rb:831:in `connect'
DEBUG [] 	/home/deploy/slice/shared/bundle/ruby/2.1.0/gems/activerecord-4.0.3/lib/active_record/connection_adapters/postgresql_adapter.rb:548:in `initialize'
DEBUG [] 	/home/deploy/slice/shared/bundle/ruby/2.1.0/gems/activerecord-4.0.3/lib/active_record/connection_adapters/postgresql_adapter.rb:41:in `new'
DEBUG [] 	/home/deploy/slice/shared/bundle/ruby/2.1.0/gems/activerecord-4.0.3/lib/active_record/connection_adapters/postgresql_adapter.rb:41:in `postgresql_connection'
DEBUG [] 	/home/deploy/slice/shared/bundle/ruby/2.1.0/gems/activerecord-4.0.3/lib/active_record/connection_adapters/abstract/connection_pool.rb:440:in `new_connection'
DEBUG [] 	/home/deploy/slice/shared/bundle/ruby/2.1.0/gems/activerecord-4.0.3/lib/active_record/connection_adapters/abstract/connection_pool.rb:450:in `checkout_new_connection'
DEBUG [] 	/home/deploy/slice/shared/bundle/ruby/2.1.0/gems/activerecord-4.0.3/lib/active_record/connection_adapters/abstract/connection_pool.rb:421:in `acquire_connection'
DEBUG [] 	/home/deploy/slice/shared/bundle/ruby/2.1.0/gems/activerecord-4.0.3/lib/active_record/connection_adapters/abstract/connection_pool.rb:356:in `block in checkout'
DEBUG [] 	/home/deploy/slice/shared/bundle/ruby/2.1.0/gems/activerecord-4.0.3/lib/active_record/connection_adapters/abstract/connection_pool.rb:355:in `checkout'
DEBUG [] 	/home/deploy/slice/shared/bundle/ruby/2.1.0/gems/activerecord-4.0.3/lib/active_record/connection_adapters/abstract/connection_pool.rb:265:in `block in connection'
DEBUG [] 	/home/deploy/slice/shared/bundle/ruby/2.1.0/gems/activerecord-4.0.3/lib/active_record/connection_adapters/abstract/connection_pool.rb:264:in `connection'
DEBUG [] 	/home/deploy/slice/shared/bundle/ruby/2.1.0/gems/activerecord-4.0.3/lib/active_record/connection_adapters/abstract/connection_pool.rb:546:in `retrieve_connection'
DEBUG [] 	/home/deploy/slice/shared/bundle/ruby/2.1.0/gems/activerecord-4.0.3/lib/active_record/connection_handling.rb:79:in `retrieve_connection'
DEBUG [] 	/home/deploy/slice/shared/bundle/ruby/2.1.0/gems/activerecord-4.0.3/lib/active_record/connection_handling.rb:53:in `connection'
DEBUG [] 	/home/deploy/slice/shared/bundle/ruby/2.1.0/gems/activerecord-4.0.3/lib/active_record/migration.rb:863:in `initialize'
DEBUG [] 	/home/deploy/slice/shared/bundle/ruby/2.1.0/gems/activerecord-4.0.3/lib/active_record/migration.rb:764:in `new'
DEBUG [] 	/home/deploy/slice/shared/bundle/ruby/2.1.0/gems/activerecord-4.0.3/lib/active_record/migration.rb:764:in `up'
DEBUG [] 	/home/deploy/slice/shared/bundle/ruby/2.1.0/gems/activerecord-4.0.3/lib/active_record/migration.rb:742:in `migrate'
DEBUG [] 	/home/deploy/slice/shared/bundle/ruby/2.1.0/gems/activerecord-4.0.3/lib/active_record/railties/databases.rake:42:in `block (2 levels) in <top (required)>'
DEBUG [] 	Tasks: TOP => db:migrate
DEBUG [] 	(See full trace by running task with --trace)
cap aborted!
SSHKit::Command::Failed: rake exit status: 1
rake stdout: Nothing written
rake stderr: rake aborted!
PG::ConnectionBad: FATAL:  no pg_hba.conf entry for host "0.0.0.0", user "postgres", database "slice", SSL on
FATAL:  no pg_hba.conf entry for host "0.0.0.0", user "postgres", database "slice", SSL off
{% endhighlight %}

The pg_hba.conf file is generated when pg is is installed. The only problem is that I don't want postgres installed on that server. I want the application to communicate with another server that hosts the postgres database so that I can have multiple application servers accessing the same database. So, since I'm not installing postgres, the config file is not being created. I could run `gem install pg --without-pg_config`, but then I wouldn't be able to run capistrano. The [pg_hba][pg_hba] file is described as a configuration file for client authentication. At first I thought this file needed to be created on the application side, but the application side actually is looking for this file on the postgres server. So, this file needs to be on the postgres server at /etc/postgresql/VERSION/main/pg_hba.conf
{% highlight ruby %}
# PostgreSQL Client Authentication Configuration File
# ===================================================
#
# Refer to the "Client Authentication" section in the PostgreSQL
# documentation for a complete description of this file.  A short
# synopsis follows.
#
# This file controls: which hosts are allowed to connect, how clients
# are authenticated, which PostgreSQL user names they can use, which
# databases they can access.  Records take one of these forms:
#
# local      DATABASE  USER  METHOD  [OPTIONS]
# host       DATABASE  USER  ADDRESS  METHOD  [OPTIONS]
# hostssl    DATABASE  USER  ADDRESS  METHOD  [OPTIONS]
# hostnossl  DATABASE  USER  ADDRESS  METHOD  [OPTIONS]
#
# (The uppercase items must be replaced by actual values.)
#
# The first field is the connection type: "local" is a Unix-domain
# socket, "host" is either a plain or SSL-encrypted TCP/IP socket,
# "hostssl" is an SSL-encrypted TCP/IP socket, and "hostnossl" is a
# plain TCP/IP socket.
#
# DATABASE can be "all", "sameuser", "samerole", "replication", a
# database name, or a comma-separated list thereof. The "all"
# keyword does not match "replication". Access to replication
# must be enabled in a separate record (see example below).
#
# USER can be "all", a user name, a group name prefixed with "+", or a
# comma-separated list thereof.  In both the DATABASE and USER fields
# you can also write a file name prefixed with "@" to include names
# from a separate file.
#
# ADDRESS specifies the set of hosts the record matches.  It can be a
# host name, or it is made up of an IP address and a CIDR mask that is
# an integer (between 0 and 32 (IPv4) or 128 (IPv6) inclusive) that
# specifies the number of significant bits in the mask.  A host name
# that starts with a dot (.) matches a suffix of the actual host name.
# Alternatively, you can write an IP address and netmask in separate
# columns to specify the set of hosts.  Instead of a CIDR-address, you
# can write "samehost" to match any of the server's own IP addresses,
# or "samenet" to match any address in any subnet that the server is
# directly connected to.
#
# METHOD can be "trust", "reject", "md5", "password", "gss", "sspi",
# "krb5", "ident", "peer", "pam", "ldap", "radius" or "cert".  Note that
# "password" sends passwords in clear text; "md5" is preferred since
# it sends encrypted passwords.
#
# OPTIONS are a set of options for the authentication in the format
# NAME=VALUE.  The available options depend on the different
# authentication methods -- refer to the "Client Authentication"
# section in the documentation for a list of which options are
# available for which authentication methods.
#
# Database and user names containing spaces, commas, quotes and other
# special characters must be quoted.  Quoting one of the keywords
# "all", "sameuser", "samerole" or "replication" makes the name lose
# its special character, and just match a database or username with
# that name.
#
# This file is read on server startup and when the postmaster receives
# a SIGHUP signal.  If you edit the file on a running system, you have
# to SIGHUP the postmaster for the changes to take effect.  You can
# use "pg_ctl reload" to do that.

# Put your actual configuration here
# ----------------------------------
#
# If you want to allow non-local connections, you need to add more
# "host" records.  In that case you will also need to make PostgreSQL
# listen on a non-local interface via the listen_addresses
# configuration parameter, or via the -i or -h command line switches.




# DO NOT DISABLE!
# If you change this first entry you will need to make sure that the
# database superuser can access the database using some other method.
# Noninteractive access to all databases is required during automatic
# maintenance (custom daily cronjobs, replication, and similar tasks).
#
# Database administrative login by Unix domain socket
local   all             postgres                                peer

# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     peer
# IPv4 local connections:
host    all             all             APPLICATION_IP/32            md5
# IPv6 local connections:
host    all             all             ::1/128                 md5
# Allow replication connections from localhost, by a user with the
# replication privilege.
#local   replication     postgres                                peer
#host    replication     postgres        127.0.0.1/32            md5
#host    replication     postgres        ::1/128                 md5
{% endhighlight %}
The important line is the APPLICATION_IP that tells your postgres server where it will accept traffic from. Most of the other lines are just comments. [Here][pg_hba_configure] is another for customizing that configuration file.


[pg_hba]: http://www.postgresql.org/docs/8.3/static/auth-pg-hba-conf.html
[pg_hba_config]: http://stackoverflow.com/questions/1406025/no-pg-hba-conf-entry-for-host

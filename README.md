README for merlin-dashboard
===========================
This dashboard for Nagios using the Merlin backend was originally made by Morten Bekkelund <bekkelund@gmail.com> and made available at http://dingleberry.me/2010/04/our-new-dashboard/ and later at https://github.com/mortis1337/nagios-dashboard where several people forked it.

Meanwhile, I and a number of other people had been hacking on it in private, fixing bugs and adding features, but not publishing the additions.

This is, finally, a fork of Morten's code with the additional patches. I aim to continue maintaining this, possibly adding patches from other people's forks.

Authors
=======
Original project by Morten Bekkelund <bekkelund@gmail.com>.

This project maintained by Mattias Bergsten <mattias.bergsten@op5.com>.

Patches from the following list of fine people and companies:

* Jonas Drange Gr�n�s <jonas@drange.net>
* IPNett AS <http://www.ipnett.no>
* Peter Andersson <peter@it-slav.net>
* Advance AB <http://www.advance.se>
* Mattias Bergsten <mattias.bergsten@op5.com>
* John Carehag <john.carehag@westbahr.com>
* Anders K Lindgren <anders.k.lindgren@ericsson.com>
* Misiu Pajor <misiu.pajor@op5.com>
* Marius Boeru <mboeru@gmail.com>

Some Livestatus socket code borrowed from the mklivestatus-slave project by Lars Michelsen at <http://git.larsmichelsen.com/git/?p=mklivestatus-slave.git>.

License
=======
Morten's code says "GPL" but unspecified version. I'm assuming GPLv2.

Requirements
============
* Webserver with PHP5 and MySQL support
* Nagios, a version that works with the Merlin version you're running
* Merlin, anything > 1.0.0
* socat for tcp to unix socket

Installation for Merlin version 1.x
===================================
Put the files in a directory on your webserver. Edit merlin.php and change the server, username and password to fit your environment. Symlink nagios.php to index.php to save typing.

Installation for Merlin version 2.x
===================================
In Merlin 2.0, saving status data to the database is turned off by default. If you do not mind the performance penalty of writing status data to the database, you can turn it on again by adding "track_current = yes" and an import program to your merlin.conf, and then following the above instructions for version 1.x.

In Monitor 6, we instead use Livestatus to access status data, because it is significantly faster. To use the Livestatus version, put the files on your monitoring server (where Livestatus runs), then edit merlin2.php and set the path to the Livestatus socket. Symlink livestatus.php to index.php to save typing.

Multi instance monitoring was added so you can create local unix sockets using socat and monitor using livestatus through these sockets. This was tested with both Nagios and Icinga
For multi instances edit merlin2.php and add instance in monitoring_instances array like so:
```php
$monitoring_instances = array(
        # socat unix-listen:/tmp/sockets/socatmonlive,reuseaddr,fork,mode=777 TCP-CONNECT:XXX.XXX.XXX.XXX:6557 &
        array(  "name"  =>      "SOCAT-MON",
                "livestatuspath"        =>      "/tmp/sockets/socatmonlive"
        ),
        array( "name"  =>      "LOCALMON",
               "livestatuspath"        =>      "/usr/local/icinga/var/rw/live",
        )
);
```

Before adding new instances, create socat unix socket with the commented command

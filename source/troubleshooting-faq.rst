.. figure:: images/white-logo.png
   :alt: SlamData Logo


Troubleshooting FAQ
===================


Section 1 - Configuration
-------------------------


1.1 Configuration File Locations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Upon initial launch, SlamData will not have a configuration file. Once a
valid database mount has been configured, a file will be created and
used to store those mount points. See below for the location of that
configuration file.Unless specified on the command line, SlamData will
look for its configuration in the following locations by default:

Windows:

::

    %HOMEDIR%\AppData\Local\quasar\quasar-config.json

Mac OS X:

::

    $HOME/Library/Application Support/quasar/quasar-config.json

Linux:

::

    $HOME/.config/quasar/quasar-config.json

Note: Do not modify this file by hand unless you have first made a
backup. If you modify this file while SlamData is running, your changes
may be overwritten.


1.1.1 Configuration File Differences
''''''''''''''''''''''''''''''''''''

SlamData Community Edition fully relies on the quasar-config.json
configuration file to store all data for the product, including
server configuration, mountings, views, and more.

SlamData Advanced Edition instead relies on a PostgreSQL or
Java H2 database to store the same metadata.  Additionally it stores
security information for users, groups, permission, actions and token.

If there is no metadata source when SlamData Advanced Edition
starts it will instead use the quasar-config.json file.


1.2 Log File Locations
~~~~~~~~~~~~~~~~~~~~~~

SlamData has a single log file whose location depends upon the OS.
Replace ``version`` below with the actual version number that is
running.

Windows:

::

    C:\Program Files (x86)\slamdata <version>/slamdata-<version>.log

Linux:

::

    $HOME/slamdata<version>/slamdata-<version>.log

Mac OS X:

::

    /Applications/SlamData <version>.app/Contents/java/app/slamdata-<version>.log


Section 2 - Running SlamData
----------------------------


2.1 SlamData Won't Start
~~~~~~~~~~~~~~~~~~~~~~~~

Follow the steps below to ensure all known issues have been addressed.

1. Some versions of SlamData require more than one CPU core before
   launching.  This is a known bug in some older versions.  Try
   increasing the number of cores in your virtual machine and
   restarting.

2. In older versions of SlamData an invalid database mount may cause SlamData
   not to start.  An invalid database mount could be a database that was
   previously available but no longer is, credentials may have changed, port
   number change, or any other configuration change which does not allow
   previously validated configurations to successfully connect.


2.2 Accessing SlamData
~~~~~~~~~~~~~~~~~~~~~~

The default SlamData URL is ``http://servername:20223/slamdata/``


2.3 How do I see which version I'm running?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

SlamData's version should be displayed in the browser title bar or
tab title.

The version of the Quasar analytics backend engine can be obtained
by browsing to ``http://servername:20223/server/info``


2.4 Running SlamData in the Cloud
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When running SlamData with a hosting provider, including Amazon EC2, the
most common error encountered is a security policy misconfiguration.
SlamData will need to connect to your database over the same port that a
standard database client does.

Keep in mind that the database server and the SlamData server do not
need to run on the same system, though there is no problem with that
either. Follow this simple checklist to ensure network problems are
minimized:

Verify the security policy for the database server is:

-  accepting incoming connections from the SlamData server IP address
-  accepting incoming connections on the correct port

If you are still unable to connect to your hosted database:

-  Verify you can connect with a standard database client from any
   system.
-  Next connect with a standard database client from the same system
   SlamData is running on.

Section 3 - Performance
-----------------------


3.1 General Considerations
~~~~~~~~~~~~~~~~~~~~~~~~~~


3.2 Advanced Edition Considerations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


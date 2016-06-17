
![SlamData Logo](/images/white-logo.png)

# Troubleshooting FAQ

---

<a name="configuration-file-locations"></a>

## Configuration File Locations

Upon initial launch, SlamData will not have a configuration file.  Once a valid database mount has been configured, a file will be created and used to store those mount points.  See below for the location of that
configuration file.Unless specified on the command line, SlamData will look for its configuration in the following locations by default:

Windows:

    %HOMEDIR%\AppData\Local\quasar\quasar-config.json

Mac OS X:

    $HOME/Library/Application Support/quasar/quasar-config.json

Linux:

    $HOME/.config/quasar/quasar-config.json

Note: Do not modify this file by hand unless you have first made a backup.  If you modify this file while SlamData is running, your changes may be overwritten.

---

<a name="log-file-locations"></a>

## Log File Locations

SlamData has a single log file whose location depends upon the OS.  Replace `version` below with the actual version number that is running.

Windows:

    C:\Program Files (x86)\slamdata <version>/slamdata-<version>.log

Linux:

    $HOME/slamdata<version>/slamdata-<version>.log

Mac OS X:

    /Applications/SlamData <version>.app/Contents/java/app/slamdata-<version>.log

---


<a name="slamdata-wont-start"></a>

## SlamData Won't Start

This is typically caused by an invalid database mount.  Either the database is not currently available, or a previously configured mount is no longer valid.  Locate the [configuration file](#installing-config-file-locations) and if necessary modify the file by hand, or if you wish to erase all previously configured mounts, you may simply rename or delete the file and restart SlamData.


---

<a name="slamdata-url"></a>

## SlamData URL

When SlamData starts on Windows and Mac OS X, a browser window (or new tab) should open with the appropriate URL based on the port defined in the configuration file. Unless the port has been modified, the URL will likely be as follows, replacing servername with the actual host name:

```
http://servername:20223/slamdata/index.html
```

---

<a name="slamdata-version"></a>

## How do I see which version I'm running?

You can simply look at your title bar in your browser window.  The title of the web page should include it.

---

<a name="slamdata-on-aws"></a>

## SlamData on Amazon EC2 / Microsoft Azure, etc.

When running SlamData with a hosting provider, including Amazon EC2, the most common error encountered is a security policy misconfiguration.  SlamData will need to connect to your database over the same port that a standard database client does.

Keep in mind that the database server and the SlamData server do not need to run on the same system, though there is no problem with that either.  Follow this simple checklist to ensure network problems are minimized:

Verify the security policy for the database server is:

 * accepting incoming connections from the SlamData server IP address
 * accepting incoming connections on the correct port

If you are still unable to connect to your hosted database:

 * Verify you can connect with a standard database client from any system.
 * Next connect with a standard database client from the same system SlamData is running on.


---

<a name="slamdata-notebook-wont-delete"></a>

## SlamData Notebook Won't Delete

When Notebooks in version 2.3.3 or earlier were renamed or deleted, sometimes the view entries associated were not updated in the configuration file.

In version 2.4 and newer, this is no longer an issue.  It is a good idea to review your configuration file, removing any now-defunct entries relating to views associated with renamed or deleted notebooks.

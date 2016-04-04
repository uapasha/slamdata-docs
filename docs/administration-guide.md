
![SlamData Logo](/images/white-logo.png)

# Administration Guide - Community Edition

This Administration Guide can assist with installing and configuring SlamData.  For information on how to use SlamData from a user perspective see the [SlamData User Guide](users-guide.md)

---

<a name="prerequisites"></a> 

## Prerequisites

* 2 GB available memory
* 300 MB disk space
* Credentials to one or more databases
* Latest version of Chrome, Firefox, Safari or Internet Explorer
* Java 1.8 if using Linux *(Java Runtime is included with OS X and Windows installers)*
* We recommend the latest version of [SlamData Community Edition](http://slamdata.com/get-slamdata/download-slamdata-community-edition/)

---

<a name="limitations"></a> 

## Limitations

* When using MongoDB, version 2.6 or newer is required

---

<a name="installing-slamdata"></a> 

## Installing SlamData

SlamData may be installed on a workstation or a server.  Workstation installation is typically best for Administrator or Business Analyst work and only needs to run when something must be configured or changed.  Server installation is best for providing services for a larger group of users (or SaaS environment) and should remain running at all times for end user access.

### Mac OS X

SlamData has been tested and runs on OS X versions 10.10 and newer.  It may run on older versions but has not been tested and is not yet supported.

1. [Download SlamData](http://slamdata.com/get-slamdata/download-slamdata-community-edition/)
2. Open the downloaded disk image (.dmg file)
3. Double-click the installer. You may need to adjust 'Security & Privacy' settings to allow downloaded apps from Anywhere.
4. Complete the installation setup program

### Linux

SlamData has been tested and runs on Ubuntu version 14.04 and newer and CentOS verion 7 and newer.  It may run on different Linux distributions and versions but has not been tested and is not yet supported.

1. [Download SlamData](http://slamdata.com/get-slamdata/download-slamdata-community-edition/)
2. Ensure you have the [Java 8 JRE or Java 8 JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) installed.
3. Make the installer executable: ```chmod +x slamdata_unix_version.sh```
4. Run the installer, providing answers to the prompts:```./slamdata_unix_version.sh```


### Windows

SlamData has been tested and runs on Windows 7 Desktop and newer and Windows Server 2008 and newer.  It may run on older versions but has not been tested and is not yet supported.

1. [Download SlamData](http://slamdata.com/get-slamdata/download-slamdata-community-edition/)
2. Run the installer
3. Complete the installation setup program.

---

<a name="launching-slamdata"></a> 

## Launching SlamData

SlamData is comprised of a frontend interface and a backend analytics engine.  Launching the SlamData application will start both.

### Mac OS X

1. Open the Applications folder
2. Double-click on the SlamData icon

A new browser window or tab will open displaying the SlamData interface. The SlamData icon will appear in the OS X dock. As with other dock applications the SlamData icon may be right-clicked and the application terminated.

### Linux

1. Change directory to the location of the SlamData executable: ```cd SlamData-version```
2. Execute the SlamData executable:  ```./SlamData```

Some Linux systems may not launch a browser automatically. If this is the case, open a browser and point it to the following URL: <a href="http://localhost:20223/slamdata">http://localhost:20223/slamdata</a>

 
### Windows

1. Open the Start menu
2. Click on the newly installed SlamData icon or use the app search bar and type ```slamdata``` and press return to launch it. Select appropriate network security settings if prompted.

---

<a name="connecting-to-a-database"></a> 

## Connecting to a Database

Connecting to a database is the first step to analyzing data.  SlamData does not provide a database to connect to.  As more databases are supported by SlamData they will be listed below.

### MongoDB

Note: 
> Only MongoDB versions 2.6 and newer are supported by SlamData.

To connect to MongoDB click on the Mount ![Mount Icon](/images/icon-mount.png) icon in the upper right.

A mount dialog will be presented:

![SlamData Mount Dialog](/images/screenshots/mount-dialog-start.png)

Enter a name for the database mount.  This name is used in the SlamData UI as well as SQL² query paths.  Use a name that makes sense for the environment.  For instance if this database were hosted on Amazon AWS/EC2 it might be named ```aws``` or ```aws-1```.

Select **MongoDB** as the mount type.  Other mount types will be discussed later.  Once a Mount type is selected additional fields will appear in the dialog based on the mount type selected.

Use the following table to assist in providing example values for the remaining fields:

| Field    | Example        |
|----------|----------------|
| Host     | db.example.com |
| Port     | 27017          |
| Username | joe            |
| Password | \*\*\*\*\*\*\* |
| Database | joesdb         |
| Settings |                |

An example form might look like this:

![SlamData MongoDB Dialog](/images/screenshots/mount-mongodb.png)

**Note**
> When using MongoDB the database field value should be the database the username and password will authenticate against. This value will depend on which database the user was created in; as such it could be ```admin```, the name of the user or something completely different.

Click **Mount** to mount the database in SlamData.

### Several Mounts

After mounting several databases the SlamData UI might look like the following image.  In this image there are three separate mounts named ```aws```, ```kirk``` and ```macbook```, the last likely representing a locally mounted database.

![SlamData Multiple Mounts](/images/screenshots/mount-all-mounts.png)

**Note**:
> Keep in mind that with <a href="http://slamdata.com/get-slamdata/" target=_blank>SlamData Advanced</a> you can limit which databases, collections and charts can be seen based on OAuth authentication and the SlamData authorization model.

<a name="advanced-configuration"></a> 

## Advanced Configuration

### Configuration File

The SlamData configuration file allows an administrator to change settings such as the port number SlamData listens on, the mounts available and more.  The location of the configuration file depends upon the operating system being used.

| Operating System        | Configuration File Location                                 |
| ------------------------|-------------------------------------------------------------|
| Apple OS X              | $HOME/Library/Application Support/quasar/quasar-config.json |
| Microsoft Windows       | %HOMEDIR%\AppData\Local\quasar\quasar-config.json           |
| Linux (various vendors) | $HOME/.config/quasar/quasar-config.json                     |

SlamData Advanced edition has an extended format of the configuration file which is not covered in this document. Please refer to the SlamData Advanced Administration Guide that you were given after purchase of the Advanced edition.

An example configuration file might appear like this:
```json
{
  "server": {
    "port": 8080
  },
  "mountings": {
    "/aws/": {
      "mongodb": {
        "connectionUri": "mongodb://myUser:myPass@aws-box.example.com:27017/admin"
      }
    },
    "/kirk/": {
      "mongodb": {
        "connectionUri": "mongodb://myUser2:myPass@win-box.example.com:27017/admin?ssl=true"
      }
    },
    "/macbook/": {
      "mongodb": {
        "connectionUri": "mongodb://localhost:27017"
      }
    }
  }
}
```

### Mount Options

The mount dialog will display the appropriate fields based on the mount type selected.  For each database type that SlamData supports a section below explains the options available.


#### MongoDB

For MongoDB the values listed in the <a href="https://docs.mongodb.org/manual/reference/connection-string/#connection-options" target=_blank>Connection Options</a> on the MongoDB web site are supported. As of MongoDB 2.6 these options are listed below.

| Options          | Example | Description                                                                                    |
|------------------|---------|------------------------------------------------------------------------------------------------|
| ssl              | true    | Enable SSL encryption                                                                          |
| connectTimeoutMS | 15000   | The time in milliseconds to attempt a connection before timing out                             |
| socketTimeoutMS  | 10000   | The time in milliseconds to attempt a send or receive on a socket before the attempt times out |


#### SQL² View

SQL² Views are covered in detail in the SlamData Users Guide.

#### Other Databases

Support for other relational and NoSQL databases is coming in 2016.

### Enabling SSL

If you have trouble following the steps below you may also view our [SSL tutorial video](https://www.youtube.com/watch?v=FWdAMyZnOMM).

If a database connection supports SSL encryption, which is to say encryption between a client and server such as SlamData and the database, additional configuration is necessary.

The backend engine of SlamData is written in [Scala](http://www.scala-lang.org/) and executes within a Java Virtual Machine (JVM).  To enable SSL encryption several options must be passed to the JVM when running SlamData.  SlamData simplifies this by allowing these options to be listed in a text file that the SlamData launcher will reference when executed.  The file location for each operating system is listed below:

| Operating System        | File Location                                               |
| ------------------------|-------------------------------------------------------------|
| Apple OS X              | /Applications/SlamData-version.app/Contents/vmoptions.txt   |
| Microsoft Windows       | C:\Programs Files (x86)\slamdata-version\SlamData.vmoptions |
| Linux (various vendors) | $HOME/slamdata-version/SlamData.vmoptions                   |

There are four important options that must be passed to the JVM at startup to enable SSL. These options point the JVM to a Java Trust Store and a Java Key Store.  Adding certificates and keys to these files are not covered in this guide and it is assumed your security or network administrator has provided you with the appropriate information.

| JVM Optiosn                      | Purpose                                               |
|----------------------------------|-------------------------------------------------------|
| javax.net.ssl.trustStore         | The location of the encrypted trust store file        |
| javax.net.ssl.keyStore           | The location of the encrypted key store file          |
| javax.net.ssl.trustStorePassword | The password required to decrypt the trust store file |
| javax.net.ssl.keyStorePassword   | The password required to decryp the key store file    |

The example contents of the file may look something like this:

```
-Xms1g
-Xmx4g
-Djavax.net.ssl.trustStore=/Users/me/ssl/truststore.jks
-Djavax.net.ssl.keyStore=/Users/me/ssl/keystore.jks
-Djavax.net.ssl.trustStorePassword=mySecretPassword
-Djavax.net.ssl.keyStorePassword=mySecretPassword
```

The first two lines specify that 1GB and 4GB of physical memory should be reserved as a minimum and maximum memory usage guideline, respectively.

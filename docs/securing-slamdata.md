![SlamData Logo](/images/white-logo.png)

# Securing SlamData Community Edition

<a name="slamdata-introduction"></a>

## Introduction

This guide can assist with configuring NGINX (or other proxy service) and SlamData to limit access based on HTTP authentication and URL paths.

**Note:**
> SlamData Advanced includes LDAP/OAuth authentication, fine grained authorization and user auditing - all of the security you need for peace of mind in the enterprise.

---

SlamData is an all-in-one NoSQL visual analytics system.  You can get started right away by simply downloading the software and running it on your local system, or install it on a dedicated server.  There is no need to follow this guide if you do not need to limit access to SlamData.  If you need to restrict access to the SlamData application to certain users, then read on!

Watching our YouTube <a href="https://www.youtube.com/watch?v=ZCzv8WCVvRM" target="_blank">video</a> (~24 minutes) may help as well. The video is based off the instructions in this guide.

There are several methods of restricting access to SlamData.  This Quick Guide focuses on configuring SlamData to run under Nginx providing basic http authorization, then forwarding requests to the backend either on the same host or a separate host.

This quick guide does not give detailed instructions on how to setup firewall rules or configure applications other than SlamData and Nginx.

---

<a name="slamdata-assumptions"></a>

## Assumptions

For the example in this quick guide, we'll continue with the following
assumptions:

* 192.168.138.220: IP address of MongoDB host, already running on port 27017
* 192.168.138.210: IP address of Backend host
* 192.168.138.200: IP address of Nginx/SlamData UI host
* If network security policy dictates, then network communication between hosts is
  restricted via ipchains or other firewall mechanism.  This allows
  simpler Backend and Nginx configuration files.
* The reader has at least a basic understanding of JSON formatting and the Linux OS
* Running on Ubuntu 14.04 or similar.  If using RedHat, use 'yum' rather than 'apt-get' for software management.
* You have version 2.2.1 or newer of SlamData source code.

If your IP addresses differ, change as appropriate.

---

<a name="slamdata-architecture-overview"></a>

## Architecture Overview

![Architecture Overview](/images/restrict-slamdata.png)

As can be noted from the above diagram, the network communications path is straight forward:

1. A request comes into SlamData running under Nginx.

2. Nginx authenticates the user via standard http auth.

3. After appropriate authentication, Nginx allows the user into the SlamData application, which communicates directly to the Backend's web API.

4. The backend runs the appropriate tasks on the data source, MongoDB in this case, and returns the results back upstream, through Nginx to the client.

For increased security, this configuration assumes the reader has setup appropriate network or OS-level restrictions that will deny access to requests other than the system upstream from it.  For instance, the MongoDB server should only allow requests to port 27017 from IP 192.168.138.210 (the Backend host).  Similarly, the Backend should only allow requests to port 8080 from IP 192.168.138.200 (the SlamData / Nginx host).  This setup is not covered in this guide and is left to the reader to implement.

---

<a name="slamdata-setting-up-the-backend"></a>

## Setting Up The Backend

The first step is to download the Backend and build the source on the system that will
be hosting it.

<a name="slamdata-download-and-build-the-backend"></a>

### Download and Build the Backend

    $ git clone https://github.com/slamdata/quasar
    $ cd quasar
    $ ./sbt test
    $ ./sbt 'project web' oneJar
    $ ./sbt 'project core' oneJar

These commands will download the latest source code, run the test suite,
and build both the Web and Core projects jar files.

<a name="slamdata-configure"></a>

### Configure

Configure the Backend server to connect to MongoDB.  Assuming there is a MongoDB
databased called 'testdb' running on 192.168.138.220, your configuration file may
be called quasar-config.json and look like this:

    {
      "mountings": {
          "/local": {
              "mongodb": {
                  "connectionUri": "mongodb://192.168.138.220/testdb"
              }
          }
      },
      "server": { "port": 8080 }
    }

For further details on the format of the configuration file, please see the
Backend [Configuration](http://slamdata.com/documentation/back-end-manual/#configuration) documentation.


<a name="slamdata-testing-the-backend"></a>

### Testing the Backend

<a name="slamdata-start-the-core-jar-file"></a>

#### Start the Core jar file

From the Backend server, run the core jar file to verify you have connectivity and your mounting is correct:

    java -jar ~/quasar/core/target/scala-2.11/core_2.11-2.2.1-SNAPSHOT-one-jar.jar ~/quasar-config.json

Once launched, a [REPL](https://en.wikipedia.org/wiki/Read–eval–print_loop) console will appear representing a virtual file system where each MongoDB database is a directory, and each database directory contains one or more MongoDB collections.

Notice how the the OS-like file system commands and SQL commands are executed directly after the $ prompt:

```
💪 $ ls
local@
💪 $ cd local
💪 $ ls
local/
testdb/
💪 $ cd testdb
💪 $ ls
coll1
💪 $ select * from coll1;
Mongo
db.coll1.find();


Query time: 0.0s
 name    | age   | gender  | minor  |
---------|-------|---------|--------|
 Johnny  |  42.0 | male    |  false |
 Jenny   |  27.0 | female  |  false |
 Deb     |  33.0 | female  |  false |
 Billy   |  15.0 | male    |   true |
```

<a name="slamdata-start-the-web-jar-file"></a>

#### Start the Web jar file

Once you have verified proper connectivity between the Backend and MongoDB, stop the Core jar file and now start the Web jar file with a slightly different syntax to point to the configuration file:

```
java -jar ~/quasar/web/target/scala-2.11/web_2.11-2.2.1-SNAPSHOT-one-jar.jar -c ~/quasar-config.json
```

Congratulations!  You now have two of the three necessary systems up and running for this configuration.

<a name="slamdata-install-prerequisites"></a>

### Install Pre-requisites

You should now be working on the system that will be hosting Nginx and SlamData. If you do not already have npm or node installed there, do so first.

On Linux:

    $ sudo apt-get install npm
    $ sudo apt-get install nodejs-legacy

On Mac:

    $ brew install npm
    $ brew install nodejs

Once npm is installed, utilize it to install [Bower](http://bower.io/),
[Gulp](http://gulpjs.com/) and [PureScript](http://www.purescript.org/):

    $ npm install bower -g
    $ npm install gulp -g
    $ npm install purescript -g

<a name="slamdata-download-and-build-slamdata"></a>

### Download and Build SlamData

Now that npm, node, bower, gulp and PureScript are installed, download the SlamData source and compile:

    $ git clone https://github.com/slamdata/slamdata
    $ cd slamdata
    $ bower install
    $ npm install
    $ gulp

When gulp completes, you should have a full application under the 'public' directory:

```
[/Users/me/slamdata]$ ls public
css           fonts         img           index.html    js            notebook.html
```

It may be safest to copy this directory and place it under a directory with separate permissions for a more secure environment.  Whatever directory you use will be considered your web root directory in future steps.

<a name="slamdata-installing-nginx"></a>

---

## Installing Nginx

### OS X
On OS X systems, consider using [HomeBrew](http://brew.sh/) to install Nginx:

    $ brew install nginx

#### Redhat / CentOS
On RedHat or CentOS systems:

    $ sudo yum install nginx

#### Ubuntu / Debian
On Ubuntu or Debian systems:

    $ sudo apt-get install nginx

<a name="slamdata-configuring-nginx"></a>

### Configuring Nginx

There are two main reasons we'll modify the Nginx configuration file in this guide:

1. To force user authentication, thus restricting access to known individuals

2. To redirect queries to the Backend engine on another host, thus limiting the
   access path to the Backend API to individuals authenticated with Nginx.

This example will use the 'default' Nginx site configuration. Nginx has many configuration files and is a versatile tool, please contact your Nginx application administrator if you have questions, or visit the Nginx web site.

If Nginx is already running, stop it:

    sudo service nginx stop

<a name="slamdata-setting-up-authentication"></a>

### Setting up Authentication

To allow http authentication, we'll need to create a file which stores the names and passwords of allowed individuals.  To do this, use the apache2-utils package which provides those tools:

    sudo apt-get install apache2-utils

Now create the htpasswd file we'll use to store the encrypted data:

    sudo htpasswd -c /etc/nginx/.htpasswd exampleuser

Replace 'exampleuser' with a real username.  You'll then be prompted for a password and verification password.

This creates the file /etc/nginx/.htpasswd which we will reference in the Nginx configuration file below.

<a name="slamdata-nginx-configuration-file"></a>

### Nginx Configuration File

Assuming the Nginx site configuration file is located at /etc/nginx/sites-available/default, replace the contents with the following code, making adjustments where necessary of course:

    server {
      listen 80 default_server;
      listen [::]:80 default_server ipv6only=on;

      server_name your_server_name;

      client_max_body_size 1024M;

      location / {
        auth_basic "Restricted";
        auth_basic_user_file /etc/nginx/.htpasswd;
        root /slamdata/public;
        try_files $uri $uri/ @backend;
      }

      location @backend {
        proxy_set_header X-Forwarder-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_redirect off;
        proxy_pass http://192.168.138.210:8080;
      }
    }

Pay close attention to the 'root' directive above.  Replace the value with the web root directory referenced earlier. The configuration above ensures the following important actions:

1. Nginx runs on port 80

2. Enables http authentication (lines beginning with 'auth_basic')

3. Nginx acts as a reverse proxy to the Backend service. This can either be
   on a remote host (as indicated above) or your the same host as Nginx.

Save the configuration and start or restart Nginx:

    sudo service nginx restart

If your network firewall is setup properly, the Backend should be shielded from all requests except those coming directly from the Nginx host.  Additionally all requests coming from the Nginx host should only originate from authenticated users via http authentication. You may test the SlamData / Nginx HTTP authorization by going to the URL:

```http://192.168.138.200```

You should be immediately prompted for a username and password.  Upon successful authorization you should see the SlamData UI:

![After Login](/images/after-login.png)

Note that you are sending requests to Nginx (IP .200), which authenticates a username and then sends the request to the Backend (IP .210), then returns the results directly to
the browser.  The browser itself is not redirected as that would defeat the purpose of securing with Nginx.


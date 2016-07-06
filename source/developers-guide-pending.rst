.. figure:: /images/white-logo.png
   :alt: SlamData Logo


Developers Guide - Pending
==========================

This Developer's Guide will assist the developer who is unfamiliar with
SlamData to install, configure, customize and embed a complete solution
from start fo finish.

For information on how to use SlamData from a user perspective
see the `SlamData Administration Guide <administration-guide.html>`__

For information on how to use SlamData from a user perspective
see the `SlamData Users Guide <users-guide.html>`__  (not implemented yet)


.. attention:: SlamData Advanced Features

  Throughout this guide there are references to functionality available
  only in SlamData Advanced Edition.  Sections that apply only to SlamData
  Advanced Edition will be called out with the Murray (MRA)
  icon. |Murray-Small|


Section 1 - Installing and Running SlamData
-------------------------------------------

Purpose
~~~~~~~

The purpose of this Developer's Guide is to walk a software developer
through SlamData from installation through completed project.  The goal
is to provide a step-by-step process that a developer can follow,
including sample data, that is repeatable with other data sets and
environments.


Introduction
~~~~~~~~~~~~

SlamData is both an Open Source Software project and a commercially
available Visual Analytics platform for multidimensional data (including
two-dimensional RDBMS data).  SlamData provides the ability to query
all of your data, in any form, in any location with a single solution.
This is achieved with some of the following features of SlamData:

- Patented multidimensional relational technology, allowing SlamData to
  communicate with any data source in any data format. This includes not
  only legacy two-dimensional data such as RDBMS in rows and columns,
  but also deeply nested, semistructured data such as JSON and XML.

- Ability to understand schema dynamically, resulting in absolutely no
  need to map field types from one technology to another.  This also allows
  SlamData to use both field values **and** the schema as data.  This is
  not possible with other NoSQL -> relational solutions.

- A fully generalized database backend technology, providing a reliable
  and ANSI compatible superset of SQL called SQL² that runs on top of any
  supported data source.  There is no need to learn yet another proprietary
  database query language.

- Fully embeddable solution that merges seamlessly with your own applications
  providing a consistent look and feel while providing significant and
  immediate value out of the box.

- Easy to use search capabilities for non-technical users.  Search for a
  key word, value or any other data type without knowing where it is or
  in which format.

- Visually appealing charts (eCharts_ from Baidu) that can be customized
  and natively understand nested data.

- Ability to secure data in a multi-tenant environment through OpenID Connect
  and OAuth 2.0.


Assumptions
~~~~~~~~~~~

This guide was written with the following assumptions in mind.  The reader
is a developer that:

- Has a basic to moderate understanding of SQL
- Has a basic to moderate understanding of JSON
- Has a basic to moderate understanding of HTML web applications
- Can perform basic navigation of the MongoDB database via the **mongo** shell
- Has appropriate permissions to install relevant software


Requirements
~~~~~~~~~~~~

For SlamData to run in an optimal environment see the
`Minimum System Requirements <administration-guide.html#minimum-system-requirements>`__
section.

.. attention:: Windows Developers

  This Developer's Guide includes example code in several sections in addition to
  shell scripts or command line utilities.  While this guide can be followed
  by most Mac OS and Linux developers, Microsoft Windows developers will have to
  implement similar functionality through other means such as DOS shell scripts.


Installation
~~~~~~~~~~~~

Instructions for installing SlamData Community Edition can be found
`here <administration-guide.html#obtaining-slamdata>`__

SlamData Advanced Edition is delivered with an automated installer.


Starting SlamData
~~~~~~~~~~~~~~~~~

Instructions for starting SlamData can be found
`here <administration-guide.html#starting-slamdata>`__.

Once SlamData Community Edition or SlamData Advanced Edition is running then
continue to Section 2.




Section 2 - Exploring Data
--------------------------

By the end of this Developers Guide the reader will have a fully working
SlamData environment that is securely embedded with user authentication,
interactive forms and dynamic charts.  To start, however, the basics of
the user interface will need to be covered.  The guide will then move
on to more complex topics focused on importing data, exploring that data
and searching it with keywords and eventually using SlamData's SQL² dialect
to perform SQL queries on the data.


Interface Navigation
~~~~~~~~~~~~~~~~~~~~

The image below shows the Home screen after starting SlamData Community
Edition.  Note the numbers and their descriptions following the image.

|Home-Annotated|


+--------+------------------------------------------------------------------------------+
| Number | Description                                                                  |
+========+==============================================================================+
|     1  |  Server or Mount names that have been configured.                            |
+--------+------------------------------------------------------------------------------+
|     2  |  The current path you are viewing. In this example it is the Home path (/).  |
+--------+------------------------------------------------------------------------------+
|     3  |  The eye icon toggles visibility of the trash can icon.                      |
+--------+------------------------------------------------------------------------------+
|     4  |  Download all data starting from this path.                                  |
+--------+------------------------------------------------------------------------------+
|     5  |  Mount a new database server.                                                |
+--------+------------------------------------------------------------------------------+
|     6  |  Create a new folder in the datasource virtual file system.                  |
+--------+------------------------------------------------------------------------------+
|     7  |  Upload a data file.                                                         |
+--------+------------------------------------------------------------------------------+
|     8  |  Create a new workspace.                                                     |
+--------+------------------------------------------------------------------------------+


Creating a New Mount
~~~~~~~~~~~~~~~~~~~~

In this guide the MongoDB database will be used in the examples; as such,
the reader should download and run the latest stable version of MongoDB.

Default MongoDB installations run on port ``27017`` and have no user
authentication enabled.  This guide assumes this configuration in the following
instructions.

Click the New Mount Icon.  |Icon-Mount|

A dialog will appear requesting the name and Mount type.

|Mount-Dialog|

Enter the values below and the dialog will expand.

+------------+-----------+
| Parameter  | Value     |
+============+===========+
| Name       |  devguide |
+------------+-----------+
| Mount Type |  MongoDB  |
+------------+-----------+

In the expanded dialog enter the values below and click ``Mount``.
If a parameter in the table below has no value, leave that
field empty in the interface.

+----------------+-----------+
| Parameter      | Value     |
+================+===========+
| Host           | localhost |
+----------------+-----------+
| Port           |  27017    |
+----------------+-----------+
| Username       |           |
+----------------+-----------+
| Password       |           |
+----------------+-----------+
| Database       |           |
+----------------+-----------+
| Other Settings |           |
+----------------+-----------+


|Mount-Dialog-Complete|


Creating a Database
~~~~~~~~~~~~~~~~~~~

* Click on the newly created server named ``devguide``.  The interface now
  shows the databases that reside within MongoDB.

  A new database will need to be created to follow along with the guide.

* Click on the Create Folder icon.  |Create-Folder|

  A new folder will appear titled ``Untitled Folder``.

* Hover the mouse over the new ``Untitled Folder`` folder.

* Click the **Move/Rename** icon that appears to the right.  |Move-Rename|

* Change the name from ``Untitled Folder`` to ``devdb`` and click **Rename**.

* Click on the newly renamed ``devdb`` folder.

The interface should now look like this:

|In-Devdb|



Importing Example Data
~~~~~~~~~~~~~~~~~~~~~~



Exploring Data
~~~~~~~~~~~~~~



Searching Data
~~~~~~~~~~~~~~



Querying Data with SQL²
~~~~~~~~~~~~~~~~~~~~~~~




Section 3 - Interactive Forms and Visualizations
------------------------------------------------


Advanced Queries with SQL²
~~~~~~~~~~~~~~~~~~~~~~~~~~


SlamDown Cells and Interactive Forms
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Query Using SlamDown Variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Visualization Card
~~~~~~~~~~~~~~~~~~


Creating a Chart
~~~~~~~~~~~~~~~~



Section 4 - Publishing and Simple Embedding
-------------------------------------------


Publishing Reports and Charts
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Simple Embedding
~~~~~~~~~~~~~~~~



Section 5 - Secure Embedding
----------------------------


Introduction to User Security
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Creating an OIDC Provider
~~~~~~~~~~~~~~~~~~~~~~~~~


Configuring the Provider
~~~~~~~~~~~~~~~~~~~~~~~~


Testing the Provider
~~~~~~~~~~~~~~~~~~~~


Creating Administrators
~~~~~~~~~~~~~~~~~~~~~~~


Creating Users
~~~~~~~~~~~~~~


Enabling Security with Roles
~~~~~~~~~~~~~~~~~~~~~~~~~~~~





.. _eCharts: https://ecomfe.github.io/echarts/index-en.html



.. |Murray| image:: images/SD3/murray.png

.. |Murray-Small| image:: images/SD3/murray-small.png

.. |Home-Annotated| image:: images/SD3/screenshots/home-annotated-with-numbers.png

.. |Icon-Mount| image:: images/SD3/icon-mount.png

.. |Mount-Dialog| image:: images/SD3/screenshots/mount-dialog.png

.. |Mount-Dialog-Complete| image:: images/SD3/screenshots/mount-dialog-complete.png

.. |Create-Folder| image:: images/SD3/icon-create-folder.png

.. |Move-Rename| image:: images/SD3/icon-move-rename.png

.. |In-Devdb| image:: images/SD3/screenshots/in-devdb-clean.png

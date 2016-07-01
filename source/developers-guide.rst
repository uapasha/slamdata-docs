.. figure:: /images/white-logo.png
   :alt: SlamData Logo



SlamData Developer's Guide
==========================





Section 1 - The Basics
----------------------

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

- Has a working understanding of SQL
- Has a working understanding of JSON
- Has a working understanding of HTML-based applications
- Understands 

Installation
~~~~~~~~~~~~

Configuration
~~~~~~~~~~~~~

Importing Demo Data
~~~~~~~~~~~~~~~~~~~





Section 2 - Generating Data and Publishing
------------------------------------------

Decks and Cards
~~~~~~~~~~~~~~~

Exploring Your Data
~~~~~~~~~~~~~~~~~~~

Searching Your Data
~~~~~~~~~~~~~~~~~~~

Querying Your Data
~~~~~~~~~~~~~~~~~~

Visualizing Your Data
~~~~~~~~~~~~~~~~~~~~~

Publishing Your Findings
~~~~~~~~~~~~~~~~~~~~~~~~





Section 3 - Interactive Reports and Embedding
---------------------------------------------

Embedding Cards
~~~~~~~~~~~~~~~

User Interaction - The SlamDown Card
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Dynamic Charts - The API Card
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Using Queries as Data Sources - The SQL² View
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~






Section 4 - Security and Best Practices
---------------------------------------

OpenID / OAuth Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Roles and Restricted Access
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Authorizing Users
~~~~~~~~~~~~~~~~~

SlamData Best Practices
~~~~~~~~~~~~~~~~~~~~~~~



.. _eCharts: https://ecomfe.github.io/echarts/index-en.html

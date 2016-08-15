.. figure:: images/white-logo.png
   :alt: SlamData Logo


Users Guide
===========


Section 1 - Introduction
------------------------


1.1 Purpose
~~~~~~~~~~~

The purpose of this Users Guide is to provide information that helps
users to install, configure and navigate SlamData.

Intended audience:  system administrators, application administrators,
content authors, developers and business analysts.

For enhanced developer-focused instructions please see the
`Developers Guide <developers-guide.html>`__.


1.2 Introduction
~~~~~~~~~~~~~~~~

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


1.3 Assumptions
~~~~~~~~~~~~~~~

This guide was written with the following assumptions in mind.  The reader:

- Has a basic to moderate understanding of JSON or semistructured data
- Has appropriate permissions to install the software


1.4 Requirements
~~~~~~~~~~~~~~~~

For SlamData to run in an optimal environment see the
`Minimum System Requirements <administration-guide.html#minimum-system-requirements>`__
section.


Section 2 - Cards
-----------------

2.1 Introduction to Cards
~~~~~~~~~~~~~~~~~~~~~~~~~

Cards each have a distinct purpose and typically provide a single, unique action
that can often be combined with the cards before and after it to create a workflow.
This section describes the types of cards and the purpose of each.


2.2 - Cache Card
~~~~~~~~~~~~~~~~

|Cache-Card|

Description
@@@@@@@@@@@

The Cache Card will store results from a Query Card, an Open Card or a Search
Card for faster retrieval while typically reducing database system load.

Card Relationships
@@@@@@@@@@@@@@@@@@

+-------------------+----------------------+
| Required          | Allowable            |
| Previous Cards    | Next Cards           |
+===================+======================+
| Open Card         | Query Card           |
+-------------------+----------------------+
| Query Card        | Search Card          |
+-------------------+----------------------+
| Search Card       | Show Table Card      |
+-------------------+----------------------+
|                   | Setup Download Card  |
+-------------------+----------------------+
|                   | Setup Chart Card     |
+-------------------+----------------------+
|                   | Troubleshoot Card    |
+-------------------+----------------------+
|                   | Cache Card           |
+-------------------+----------------------+

Behavior
@@@@@@@@

The Cache Card requires a location to store its results.  When a newly selected
Cache Card becomes active, the user will be presented with a pre-populated text
field and a **Confirm** button.  The value in this field can be edited directly
to change the location of the cached information. The credentials provided to
mount the original DB must have read and write privileges to the specified path
or the cache card will not be created.

Results stored in a Cache Card are updated when one of the following occurs:

* The table or collection in the Open Card is modified
* The query in the Query Card is modified
* The search parameters in the Search Card are modified


2.3 - Open Card
~~~~~~~~~~~~~~~

|Open-Card|

Description
@@@@@@@@@@@

The Open Card can be used to specify a collection or table from which
subsequent cards will operate from.

Card Relationships
@@@@@@@@@@@@@@@@@@

+-------------------+----------------------+
| Required          | Allowable            |
| Previous Cards    | Next Cards           |
+===================+======================+
| N/A               | Query Card           |
+-------------------+----------------------+
|                   | Search Card          |
+-------------------+----------------------+
|                   | Show Table Card      |
+-------------------+----------------------+
|                   | Setup Download Card  |
+-------------------+----------------------+
|                   | Setup Chart Card     |
+-------------------+----------------------+
|                   | Troubleshoot Card    |
+-------------------+----------------------+
|                   | Cache Card           |
+-------------------+----------------------+

Behavior
@@@@@@@@

The Open Card is typically the first card in a workflow if a query
is not used as the source for subsequent cards.  By selecting a table
or collection with the Open Card, the next card will have access to
that collection or table as a whole.

Common scenarios leveraging the Open Card include following it with
a Search Card or Show Table Card.


2.4 - Query Card
~~~~~~~~~~~~~~~~

|Query-Card|

Description
@@@@@@@@@@@

The Query Card allows a user to execute a SQL² query against one or
more tables or collections.  If variables were defined from either
a Setup Variables Card or a Markdown Card in previous cards then
those variables may be used in the query.  For more information
on SQL² syntax please see the
`SQL² Reference Guide <sql-squared-reference.html>`__.


Card Relationships
@@@@@@@@@@@@@@@@@@

+-------------------+----------------------+
| Required          | Allowable            |
| Previous Cards    | Next Cards           |
+===================+======================+
| N/A               | Cache Card           |
+-------------------+----------------------+
|                   | Search Card          |
+-------------------+----------------------+
|                   | Query Card           |
+-------------------+----------------------+
|                   | Show Table Card      |
+-------------------+----------------------+
|                   | Setup Download Card  |
+-------------------+----------------------+
|                   | Setup Chart Card     |
+-------------------+----------------------+
|                   | Troubleshoot Card    |
+-------------------+----------------------+


Behavior
@@@@@@@@

If a Query Card follows a Show Table Card then the collection name
will be automatically populated in the query and cannot be changed.

A Query Card contains a ``Run Query`` button that is used when the user
is finished entering a query.  If a query is not changed the query will
execute automatically within a workflow.


2.5 - Search Card
~~~~~~~~~~~~~~~~~

|Search-Card|

Description
@@@@@@@@@@@

The Search Card allows users to search for entries from a data source.
This data source can either be a specific collection or table designated
via the Open Card or it can also be the result set from a Query Card.

Card Relationships
@@@@@@@@@@@@@@@@@@

+-------------------+----------------------+
| Required          | Allowable            |
| Previous Cards    | Next Cards           |
+===================+======================+
| Open Card         | Query Card           |
+-------------------+----------------------+
| Query Card        | Search Card          |
+-------------------+----------------------+
|                   | Show Table Card      |
+-------------------+----------------------+
|                   | Setup Download Card  |
+-------------------+----------------------+
|                   | Setup Chart Card     |
+-------------------+----------------------+
|                   | Troubleshoot Card    |
+-------------------+----------------------+
|                   | Cache Card           |
+-------------------+----------------------+

Behavior
@@@@@@@@

A Search Card is typically followed by a Show Table Card to display
the result of the search.

Values not preceded by a field name and
colon, such as ``fieldName:``, will cause the database to search through
all fields and may cause a delay in producing results from large tables
or collections.  Additionally, specifying a field name before a value will
typically result in a database leveraging an indexed query (if an appropriate
index exists), resulting in a faster database response.

Search parameters are "AND"ed together, so the more parameters that a user
provides, the more selective the result will be.

* Search for everything containing the text "foo":

    ``foo``

    ``+foo``

* Search for everything *not* containing the text "foo":

    ``-foo``

* Search for everything that contains a "foo" field whose value is greater than 2:

    ``foo:>2``

* Search for everything containing a "foo" field whose value falls inside the range of 0..2:

    ``foo:0..2``

* Search for everything that contains a "foo" field which contains a "bar" field which contains the text "baz":

    ``foo:bar:baz``


See the table below for some helpful search examples:

+---------------------------+---------------------------------------------------------------+
| Example                   | Description                                                   |
+===========================+===============================================================+
| ``colorado``              | Searches for the **substring** ``colorado`` in **all fields** |
+---------------------------+---------------------------------------------------------------+
| ``=colorado``             | Searches for the **full word** ``colorado`` in **all fields** |
+---------------------------+---------------------------------------------------------------+
| ``age:=50``               | Searches the field **age** for a value of 50                  |
+---------------------------+---------------------------------------------------------------+
| ``age:>=50``              | Searches the field **age** for any value greater than or      |
|                           | equal to 50                                                   |
+---------------------------+---------------------------------------------------------------+
| ``age:50..60``            | Searches the field **age** for values between or equal to     |
|                           | 50 and 60                                                     |
+---------------------------+---------------------------------------------------------------+
| ``codes:"[*]":desc:flu``  | Performs a deep search through the **codes** array and        |
|                           | examines each subdocument's **desc** field for the            |
|                           | **substring** ``flu``                                         |
+---------------------------+---------------------------------------------------------------+


2.6 - Setup Chart Card
~~~~~~~~~~~~~~~~~~~~~~

|Setup-Chart-Card|

Description
@@@@@@@@@@@

The Setup Chart Card is required before using the Show Chart Card.  This
card allows an author to specify the chart type and chart options of the
subsequent Show Chart Card.

Major Chart Types
@@@@@@@@@@@@@@@@@

* Area Chart
* Bar Chart
* Line Chart
* Pie Chart
* Radar Chart
* Scatter Plot Chart

Card Relationships
@@@@@@@@@@@@@@@@@@

+-------------------+----------------------+
| Required          | Allowable            |
| Previous Cards    | Next Cards           |
+===================+======================+
| Query Card or     | Show Chart Card      |
+-------------------+----------------------+
| Show Table Card   |                      |
+-------------------+----------------------+

Behavior
@@@@@@@@

The available chart types in the left column of a Setup Chart Card will
vary depending on the result set returned from a preceding card.

Each major chart type will have options that allows an author to control
the look of the chart.  For instance an Area Chart will allow an author
the choice to stack values or not.


2.7 - Setup Download Card
~~~~~~~~~~~~~~~~~~~~~~~~~

|Setup-Download-Card|

Description
@@@@@@@@@@@

The Setup Download Card precedes the Show Download Card.  An author can
configure the format of the downloaded file, JSON or CSV, in addition
to several other parameters.

Card Relationships
@@@@@@@@@@@@@@@@@@

+-------------------+----------------------+
| Required          | Allowable            |
| Previous Cards    | Next Cards           |
+===================+======================+
| Query Card or     | Show Download Card   |
+-------------------+----------------------+
| Open Card or      |                      |
+-------------------+----------------------+
| Search Card       |                      |
+-------------------+----------------------+

Behavior
@@@@@@@@

The Setup Download Card must always precede a Show Download Card.  Each
file format (CSV/JSON) will have different export options available.  Once
options are configured, they can be change by the workspace author but not
by a user through a published or embedded workspace.


2.8 - Setup Draftboard Card
~~~~~~~~~~~~~~~~~~~~~~~~~~~

|Setup-Draftboard-Card|

Description
@@@@@@@@@@@

The Setup Draftboard Card may only be selected as the first card in the
first deck inside of a workspace.  Creating a Setup Draftboard Card is
similar to flipping a workspace that contains a single deck and
choosing **Wrap**, except there is no existing deck and one must now
be created.

Card Relationships
@@@@@@@@@@@@@@@@@@

+-------------------+----------------------+
| Required          | Allowable            |
| Previous Cards    | Next Cards           |
+===================+======================+
| N/A               | N/A                  |
+-------------------+----------------------+

Because the Setup Draftboard Card creates a workspace with no decks or
cards, it must be the first card in the deck.  Additionally an author
must now create a new deck inside of this Draftboard so the concept
of an allowable next card is not applicable.


2.9 - Setup Markdown Card
~~~~~~~~~~~~~~~~~~~~~~~~~

|Setup-Markdown-Card|

Description
@@@@@@@@@@@

The Setup Markdown Card allows an author to write the Markdown code that
will be rendered within a Show Markdown Card.

Card Relationships
@@@@@@@@@@@@@@@@@@

+-------------------+----------------------+
| Required          | Allowable            |
| Previous Cards    | Next Cards           |
+===================+======================+
| N/A               | Show Markdown Card   |
+-------------------+----------------------+

Behavior
@@@@@@@@

The Setup Markdown Card acts like a text editor to edit Markdown.  Valid
Markdown code will typically be highlighted blue and line numbers are
listed in the left column.

For detailed information regarding SlamDown,
the SlamData-enhanced version of Markdown, please see the
`SlamDown Reference Guide <slamdown-reference.html>`__.  The reference
guide describes how to create interactive UI elements such as drop
downs, radio boxes, check boxes and more.


2.10 - Setup Variables Card
~~~~~~~~~~~~~~~~~~~~~~~~~~~

|Setup-Variables-Card|

Description
@@@@@@@@@@@

The Setup Variables Card allows an author to create a workspace where the
results are controlled by parameters being programatically passed into it.

Card Relationships
@@@@@@@@@@@@@@@@@@

+--------------------------+----------------------+
| Required                 | Allowable            |
| Previous Cards           | Next Cards           |
+==========================+======================+
| N/A - Must be first card | Query Card           |
+--------------------------+----------------------+
|                          | Setup Markdown Card  |
+--------------------------+----------------------+
|                          | Troubleshoot Card    |
+--------------------------+----------------------+

Behavior
@@@@@@@@

Each variable in the Setup Variables Card is defined on a separate line.  A
variable may be any data type listed in the Data Types section below.

Note that following a Variables Card with a Troubleshoot Card is helpful in
validating values passed into the Workspace.

When embedding a Workspace that contains a Setup Variables Card into a third party
application, the JavaScript and HTML that SlamData generates for the author
will be slightly different than workspaces without a Setup Variables Card.

For example, if two variables called ``state`` and ``city`` with values of
``CO`` and ``DENVER``, respectively, are defined in a variables card, the
resulting JavaScript will contain a ``vars`` section, similar to the following:

.. code-block:: javascript

      SlamData.embed({
        deckPath: "/server/db/collection/MyWorkspace.slam/",
        deckId: "deckid...abc...123...",
        // An array of custom stylesheets URLs can be provided here
        stylesheets: [],
        // The variables for the deck(s), you can change their values here:
        vars: {
          "deckid...abc...123...": {
            "state": "CO",
            "city": "DENVER"
          }
        }
      });

Third party applications may generate this JavaScript programatically, changing
the values of the ``state`` and ``city`` variables based on custom logic.


Data Types
@@@@@@@@@@

Text
!!!!

An input field will appear when the Text data type is chosen.  Alphanumeric
text may be entered.

Example: ``My 123 value here``

DateTime
!!!!!!!!

A date and time picker will appear when the Date data type is chosen.  Selecting a
date and time will designate the default value.

Date
!!!!

A date picker will appear when the Date data type is chosen.  Selecting a
date from the date picker will designate the default value.

Time
!!!!

A time picker will appear when the Time data type is chosen.  Selecting a time
will designate the default value.

Interval
!!!!!!!!

Pending

Boolean
!!!!!!!

A checkbox will appear when the Boolean data type is chosen.  Checking
the box will designate the default value to ``true``.

Numeric
!!!!!!!

An input field will appear when the Numeric data type is chosen.  Only
numeric values are allowed in this field.

Example:  ``1`` or ``1.5``

Object ID
!!!!!!!!!

An input field will appear when the Object ID data type is chosen.  Any
valid Object ID can be entered here.  The subsequent query should not
be preceded by the ``OID`` function in SQL² as this will be handled
automatically.  For instance, if the value ``5792b247045175200c4fcd0f``
is entered for the ``myoidvar`` variable, the resulting query would
look similar to:

.. code-block:: SQL

    SELECT * FROM `/server/db/collection`
    WHERE _id = :myoidvar

Array
!!!!!

An input field will appear the Array data type is chosen.  A valid array
should be entered as the default.

Example:  ``["S1", "S2", "S3"]``

The subsequent query should reference the values in the array appropriately.
For example, if the variables ``sensors`` was defined in the Setup
Variables Card, and we wanted a query to return all records containing
a ``sensor`` field that matched any entry from the array, the query might
look like this:

.. code-block:: SQL

    SELECT * FROM `/server/db/collection`
    WHERE sensor IN :sensors[_]


Object
!!!!!!

Pending

SQL² Expression
!!!!!!!!!!!!!!!

Pending

SQL² Identifier
!!!!!!!!!!!!!!!

An input field will appear when the SQL² Identifier data type is chosen.
A valid query path should be entered as the default.  This allows a developer
to pass in a specific query path while the remainder of the query remains
unchanged.

Example: mypath = ``/server/db/collection``

The subsequent query would look like:

.. code-block:: SQL

    SELECT * FROM :mypath




2.11 - Show Chart Card
~~~~~~~~~~~~~~~~~~~~~~

|Show-Chart-Card|


2.12 - Show Download Card
~~~~~~~~~~~~~~~~~~~~~~~~~

|Show-Download-Card|


2.13 - Show Markdown Card
~~~~~~~~~~~~~~~~~~~~~~~~~

|Show-Markdown-Card|


2.14 - Show Table Card
~~~~~~~~~~~~~~~~~~~~~~

|Show-Table-Card|


2.15 - Troubleshoot Card
~~~~~~~~~~~~~~~~~~~~~~~~

|Troubleshoot-Card|




Section 3 - Workflow Examples
-----------------------------

**COMING SOON**





.. |Cache-Card| image:: images/SD3/cards/card-cache.png
   :height: 150px
   :width: 150px

.. |Open-Card| image:: images/SD3/cards/card-open.png
   :height: 150px
   :width: 150px

.. |Query-Card| image:: images/SD3/cards/card-query.png
   :height: 150px
   :width: 150px

.. |Search-Card| image:: images/SD3/cards/card-search.png
   :height: 150px
   :width: 150px

.. |Setup-Chart-Card| image:: images/SD3/cards/card-setup-chart.png
   :height: 150px
   :width: 150px

.. |Setup-Download-Card| image:: images/SD3/cards/card-setup-download.png
   :height: 150px
   :width: 150px

.. |Setup-Draftboard-Card| image:: images/SD3/cards/card-setup-draftboard.png
   :height: 150px
   :width: 150px

.. |Setup-Markdown-Card| image:: images/SD3/cards/card-setup-markdown.png
   :height: 150px
   :width: 150px

.. |Setup-Variables-Card| image:: images/SD3/cards/card-setup-variables.png
   :height: 150px
   :width: 150px

.. |Show-Chart-Card| image:: images/SD3/cards/card-show-chart.png
   :height: 150px
   :width: 150px

.. |Show-Download-Card| image:: images/SD3/cards/card-show-download.png
   :height: 150px
   :width: 150px

.. |Show-Markdown-Card| image:: images/SD3/cards/card-show-markdown.png
   :height: 150px
   :width: 150px

.. |Show-Table-Card| image:: images/SD3/cards/card-show-table.png
   :height: 150px
   :width: 150px

.. |Troubleshoot-Card| image:: images/SD3/cards/card-troubleshoot.png
   :height: 150px
   :width: 150px


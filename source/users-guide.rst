.. figure:: images/white-logo.png
   :alt: SlamData Logo


Users Guide
===========


Section 1 - Introduction
------------------------


1.1 Assumptions
~~~~~~~~~~~~~~~

This guide was written with the following assumptions in mind.  The reader:

- Has a basic to moderate understanding of JSON or semistructured data
- Has appropriate permissions to install the software


1.2 Requirements
~~~~~~~~~~~~~~~~

For SlamData to run in an optimal environment see the
`Minimum System Requirements <administration-guide.html#minimum-system-requirements>`__
section.


1.3 Installation
~~~~~~~~~~~~~~~~

See the 
`Installation Section <administration-guide.html#section-1-installation>`__
of the Administrator's Guide for installation instructions.


Section 2 - Quick Start (or "Read Me First!")
-----------------------------------------------------

The following two sections will take a new user from zero knowledge of the SlamData
workflow to creating a basic Workspace with some suggestions.  This section is intended as a
quick start and not an exhaustive instruction set.  See the remaining
sections of the User's Guide for detailed information on specific
functionality.


2.1 - Configuration Suggestions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. JVM Settings

Modify the vmoptions.txt file to adjust Java memory heap space.  JVM memory
allocation varies by default based on JVM vendor and version.  To ensure
proper functionality, reserve 1GB or more of JVM heap space, increasing it
based on requirements.  It is not uncommon to have more than 4GB of heap space
reserved for SlamData server environments.
   
+-------------------------+-------------------------------------------------------------+
| Operating System        | File Location                                               |
+=========================+=============================================================+
| Mac OS                  | /Applications/SlamData-version.app/Contents/vmoptions.txt   |
+-------------------------+-------------------------------------------------------------+
| Microsoft Windows       | C:\Programs Files (x86)\slamdata-version\SlamData.vmoptions |
+-------------------------+-------------------------------------------------------------+
| Linux (various vendors) | $HOME/slamdata-version/SlamData.vmoptions                   |
+-------------------------+-------------------------------------------------------------+

Example of reserving 4GB of JVM heap space for a server-class system:

::

    -server
    -Xms4g
    -Xmx4g


The ``-server`` entry, depending on JVM vendor and version, will typically focus on
longer initial load times to allow better compilation methods for faster run-time.  ``-Xms4g``
immediately allocates no less than 4GB of memory and ``-Xmx4g`` allocates no more
than 4GB of memory.


2.2 The Log File
~~~~~~~~~~~~~~~~

If a user suspects that SlamData is not functioning properly, the first step
to troubleshooting is looking at the most recent log file, located in the
table below:

+-------------------------+-------------------------------------------------------------------------------+
| Operating System        | File Location                                                                 |
+=========================+===============================================================================+
| Mac OS                  | /Applications/SlamData <version>.app/Contents/java/app/slamdata-<version>.log |
+-------------------------+-------------------------------------------------------------------------------+
| Microsoft Windows       | C:\Program Files (x86)\slamdata <version>/slamdata-<version>.log              |
+-------------------------+-------------------------------------------------------------------------------+
| Linux (various vendors) | $HOME/slamdata<version>/slamdata-<version>.log                                |
+-------------------------+-------------------------------------------------------------------------------+

Some JVM errors can cause the JVM to stop running completely, resulting in the SlamData
UI becoming unresponsive.  Reviewing the log file should provide helpful information.


2.3 Browsers
~~~~~~~~~~~~

Most modern browsers are supported by SlamData.  The most compatible browsers are always the
most recent versions of Google Chrome and Mozilla Firefox.  Microsoft IE, Microsoft Edge and Apple Safari will
work with SlamData but users are strongly encouraged to use Google Chrome or Firefox when possible
as other browsers are less flexible and code fixes to support those browsers are not
as frequent.


Section 3 - The Workspace (or "Don't Blow Yourself Up!")
-------------------------------------------------------


3.1 Workspace Background
~~~~~~~~~~~~~~~~~~~~~~~~

SlamData approaches analytics workflows with the metaphor of a deck or multiple
decks of cards, sometimes on a Draftboard layout.  A deck is built by stacking
unique cards on top of one another, each card having a specific purpose such
as opening a table or collection, displaying a result set, displaying a
chart, etc.


3.2 Mount database
~~~~~~~~~~~~~~~~~~~~

This section assumes you have MongoDB running locally on the system you
installed SlamData on.  Adjust IP addresses and hostnames as appropriate.

Click the Mount icon |Icon-Mount| to mount a database server.

A dialog will appear requesting the name and Mount type.

|Mount-Dialog|

Select MongoDB in the Mount Type and enter the appropriate values in the dialog.

Example:

+------------+-----------+
| Parameter  | Value     |
+============+===========+
| Name       | myserver  |
+------------+-----------+
| Mount Type | MongoDB   |
+------------+-----------+

In the expanded dialog enter the appropriate values and click **Mount**.

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




3.3 Creating Your First Database
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* Click on the newly created server.  The interface now
  shows the databases that reside within that server.

If databases exist on your server, some may be displayed here depending upon
the credentials supplied in the mount dialog.

* Click on the Create Folder icon.  |Create-Folder|

  A new folder will appear titled **Untitled Folder**.

* Hover the mouse over the new **Untitled Folder** folder.

* Click the **Move/Rename** icon that appears to the right.  |Move-Rename|

* Change the name from **Untitled Folder** to ``testdb`` or another name and click **Rename**.

* Click on the newly renamed folder.  Any tables or collections for this database will
  be displayed here.


3.4 Importing Sample Data
~~~~~~~~~~~~~~~~~~~~~~~~~

You can download a data set with 10,000 documents by following these
instructions:

* Right click `this link <https://github.com/damonLL/tutorial_files/raw/master/patients>`__
  and save the file as ``patients``.  This is a 9 MB JSON file.

* If your operating system named the file something other than
  **patients** you can either rename it or you can rename it
  inside of SlamData once it has been uploaded.

* Ensure the SlamData UI is in the *testdb* database, and click
  the Upload icon.  |Upload|

* In the file dialog find the patients file and submit it.

As you can see it is easy to import JSON and CSV data into
SlamData quickly.  The underlying database in this case is
MongoDB.

If the uploaded file appears as `patients.json` or anything other than
simply `patients` the user should consider renaming it to simplify
queries and shorten the query path.

The user may wish to index the newly imported patients data set. If
using MongoDB refer to 
`this section <developers-guide.html#indexing-your-database>`__ of
the Developer's Guide to increase search and query performance.


3.5 Exploring Sample Data
~~~~~~~~~~~~~~~~~~~~~~~~~

* Click on the new patients file in the user interface.

* A dialog will appear asking the name of the new workspace being created.

* User will be presented with a table showing the contents of the patients file.

Take note that the data in the table is not only top level fields but also
contains arrays of various types of data for each record or document.

In this instance SlamData created a new Workspace for the user, created an
Open Card pointing to the new patients file, then stacked a Show Table card
on top of the Open Card.

The user can verify this by clicking on the left dots (gripper) on the left side
of the screen and seeing the top most card slide to the right.  The card now
displayed is the Open Card.  This determines which table or collection is used
by the cards following it.

* Click on the right grippers to go back to the Show Table Card

The user can now navigate between pages of results.

Click on the Zoom Out |Zoom-Out| icon in the upper left of the interface to return to
the database view.


3.6 Querying Sample Data
~~~~~~~~~~~~~~~~~~~~~~~~

* Create a new workspace by clicking on the Create Workspace icon

* Select the *Query Card*
  
* Replace the provided query text with the query below:
  
.. code-block:: sql

    SELECT
      last_name || ", " || first_name AS Name,
      city as City,
      state as State,
      codes[*].code AS Code,
      codes[*].desc AS Description
    FROM `/myserver/db_name/patients`

Change the path of the `FROM` clause to match your environment.

Notice that we are concatenating two fields (``last_name`` and ``first_name``),
as well as analyzing each document within the ``codes`` array and fetching
the ``code`` and ``desc`` fields from each of those documents.

* Depending upon the version of SlamData running the user may see a
  ``Run Query`` button in the Query Card.  If displayed, the user must click
  this to execute the query.

* Click on the right gripper (dots) on the right side of the interface
  to stack a new card on top of this card.

* Select the Show Table Card
  
* View the results of your query

* Click the Zoom Out |Zoom-Out| icon to return to the database view.
  
* Optionally rename the Untitled Workspace that was created for this workflow.


3.7 Searching Data
~~~~~~~~~~~~~~~~~~

In this example the user will learn how to create a draftboard card to store
multiple decks of cards, and mirror one deck of cards to recreate functionality
in a second deck of cards.

* Create a new Workspace

* Select the Open Card
  
* Locate the patients entry in your database and select it
  
* Click the right gripper (dots) to stack a new card on top of this card.
  
* Select the Search Card

* Click the Flip-Icon |Icon-Flip| in the upper right of the interface.

* Select the Wrap option

Notice the deck is now within a workspace where you can drag the deck
by its top gripper, and resize it by using the lower-right gripper of
that deck.

This deck will now serve as the basis of an additional deck whereby
the contents and user entry of the first deck will flow into the
mirrored deck.
  
.. warning:: Workspace Nuances

  The user is advised to avoid clicking in the open space of the draftboard
  in the UI as it will create a new deck which is not associated with the
  original deck.  If this occurs, the user can click on the Flip Icon |Icon-Flip| of
  the newly created deck and select Delete Deck.  Decks do not need to be
  created by mirroring other decks; however that option is not covered in
  that section.

  Users are also advised to avoid dragging one deck on top of another deck unless
  the desired effect is to have nested decks.

* Activate or highlight the existing deck.

* Click the Flip Icon |Icon-Flip| for the deck.
  
* Select Mirror Deck

A new deck will appear directly below the original deck.  This deck is synchronized
with the original deck.  Changes made to either deck at this point will reflect in
the other deck; however, new cards stacked onto the new deck will not impact the
original deck.

* Consider resizing the original deck to use less screen space, and moving the
  new deck alongside the original deck and resizing it to take more space.

* Activate the newly mirrored deck and click the right gripper (dots) to stack a
  new card.

* Select the Show Table card

Now information entered into the search field in the original deck will immediately
cause the results to be displayed in the new deck.

* Enter the value ``AUSTIN`` in the search string and see the results shortly after
  in the new deck.

Notice no field name was specified.  SlamData, by default, will search all fields
for the value.  Prefixing a search term with a field name will cause SlamData to
search a specific field for the value.  

* Enter the value ``city:AUSTIN`` to restrict the search to just the ``city`` field name.

The next steps shows multiple values which will be ANDed together, and will search
through nested data as well.

* Enter the string ``previous_addresses:"[*]":state:CA age:>50 gender:=male``

This searches all documents where the `previous_addresses` array contains multiple entries,
each with a `state` field for the state of California. It also searches for ages over
50 and where gender is male.


3.8 - Downloading Data
~~~~~~~~~~~~~~~~~~~~~~

This workspace can be adjusted to allow a user to download the results of the
search after the search is complete.

* In the deck containing the results table click the Flip Icon |Icon-Flip|

* Select Mirror Deck.  A newly created deck will appear below the existing deck.

* In the newly created deck click the right gripper (dots) to stack a new
  card on top of the Show Table card.

* Select the Setup Download option

* Select either ``C;S;V`` (CSV) or ``{JS}`` (JSON) format for the download.

* Click the right gripper (dots) to stack a new card on the deck.

* Select the Show Download card

* Resize the deck so that the Download button can be seen but the deck
  is much smaller.

* Optionally move the deck to align with the other two decks for better
  visual appearance.

Now a user may enter search criteria, browse the results and download
the results in CSV or JSON format.


Section 4 - Cards
-----------------

4.1 Introduction to Cards
~~~~~~~~~~~~~~~~~~~~~~~~~

Cards each have a distinct purpose and typically provide a single, unique action
that can often be combined with the cards before and after it to create a workflow.
This section describes the types of cards and the purpose of each.


4.2 - Cache Card
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


4.3 - Open Card
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


4.4 - Query Card
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


4.5 - Search Card
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


4.6 - Setup Chart Card
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


4.7 - Setup Download Card
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


4.8 - Setup Draftboard Card
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


4.9 - Setup Markdown Card
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


4.10 - Setup Variables Card
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




4.11 - Show Chart Card
~~~~~~~~~~~~~~~~~~~~~~

|Show-Chart-Card|


4.12 - Show Download Card
~~~~~~~~~~~~~~~~~~~~~~~~~

|Show-Download-Card|


4.13 - Show Markdown Card
~~~~~~~~~~~~~~~~~~~~~~~~~

|Show-Markdown-Card|


4.14 - Show Table Card
~~~~~~~~~~~~~~~~~~~~~~

|Show-Table-Card|


4.15 - Troubleshoot Card
~~~~~~~~~~~~~~~~~~~~~~~~

|Troubleshoot-Card|




Section 5 - Workflow Examples
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

.. |Icon-Mount| image:: images/SD3/icon-mount.png

.. |Zoom-Out| image:: images/SD3/icon-zoom-out.png

.. |Icon-Flip| image:: images/SD3/icon-flip.png

.. |Mount-Dialog| image:: images/SD3/screenshots/mount-dialog.png

.. |Create-Folder| image:: images/SD3/icon-create-folder.png

.. |Move-Rename| image:: images/SD3/icon-move-rename.png

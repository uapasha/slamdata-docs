.. figure:: images/white-logo.png
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

1.1 Purpose
~~~~~~~~~~~

The purpose of this Developer's Guide is to walk a software developer
through SlamData from installation through completed project.  The goal
is to provide a step-by-step process that a developer can follow,
including sample data, that is repeatable with other data sets and
environments.


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

This guide was written with the following assumptions in mind.  The reader
is a developer that:

- Has a basic to moderate understanding of SQL
- Has a basic to moderate understanding of JSON
- Has a basic to moderate understanding of HTML web applications
- Can perform basic navigation of the MongoDB database via the **mongo** shell
- Has appropriate permissions to install relevant software


1.4 Requirements
~~~~~~~~~~~~~~~~

For SlamData to run in an optimal environment see the
`Minimum System Requirements <administration-guide.html#minimum-system-requirements>`__
section.

.. attention:: Windows Developers

  This Developer's Guide includes example code in several sections in addition to
  shell scripts or command line utilities.  While this guide can be followed
  by most Mac OS and Linux developers, Microsoft Windows developers will have to
  implement similar functionality through other means such as DOS shell scripts.


1.5 Installation
~~~~~~~~~~~~~~~~

Instructions for installing SlamData Community Edition can be found
`here <administration-guide.html#obtaining-slamdata>`__

SlamData Advanced Edition is delivered with an automated installer.


1.6 Starting SlamData
~~~~~~~~~~~~~~~~~~~~~

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


2.1 Interface Navigation
~~~~~~~~~~~~~~~~~~~~~~~~

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


2.2 Workspaces, Decks and Cards
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Before we start looking at our data we need to discuss how to interact with
it.  This is done through the use of a **Workspace**.  A Workspace is the
primary method that users interact with data within SlamData.  A
Workspace in turn is comprised of cards, and decks of cards.

* **Root Deck** - Each Workspace must have a Root Deck in which all other unit types
  are stored. A Root Deck is always present in a Workspace but never visible.

* **Deck** - Each deck contains at least one or more cards that each perform a
  specific action and build upon each other.  Decks can be mirrored which allows
  easy creation of a new target deck that starts with the same functionality as
  the origin deck.  Changes in each deck, up to the point where they were
  mirrored, will impact each other.

* **Draftboard Card** - A special card type that creates a visual area to arrange
  multiple decks.

* **Card** - A unit that performs a distinct action. Examples include:

    * Query Card
    * Show Table Card
    * Show Cart Card
    * and more...

+-----------------+---------------------------------------------------------------+
| Unit Type       | May Contain:                                                  |
+=================+===============================================================+
| Root Deck       | Either a single **Draftboard Card** or multiple normal cards. |
+-----------------+---------------------------------------------------------------+
| Deck            | One or more cards, including one **Draftboard Card**          |
+-----------------+---------------------------------------------------------------+
| Draftboard Card | One or more decks.                                            |
+-----------------+---------------------------------------------------------------+
| Card            | N/A                                                           |
+-----------------+---------------------------------------------------------------+

A visual example of the allowable nesting follows:

|SD-Nesting|

Don't worry!  You won't need to know any of this until section 3, and by then we
will take you through it step by step.


2.3 Creating a New Mount
~~~~~~~~~~~~~~~~~~~~~~~~

In this guide the MongoDB database will be used in the examples; as such,
the reader should download and run the latest stable version of MongoDB.

Default MongoDB installations run on port **27017** and have no user
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

In the expanded dialog enter the values below and click **Mount**.
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


2.4 Creating a Database
~~~~~~~~~~~~~~~~~~~~~~~

* Click on the newly created server named **devguide**.  The interface now
  shows the databases that reside within MongoDB.

  A new database will need to be created to follow along with the guide.

* Click on the Create Folder icon.  |Create-Folder|

  A new folder will appear titled **Untitled Folder**.

* Hover the mouse over the new **Untitled Folder** folder.

* Click the **Move/Rename** icon that appears to the right.  |Move-Rename|

* Change the name from **Untitled Folder** to ``devdb`` and click **Rename**.

* Click on the newly renamed **devdb** folder.

The interface should now look like this:

|In-Devdb|

So far in this guide you've installed SlamData, mounted a database and
created and renamed a folder.  Good progress.  Let's get some data into
the database now and start exploring.

2.5 Importing Example Data
~~~~~~~~~~~~~~~~~~~~~~~~~~

This guide uses a data set of fictitious patient information that was
randomly generated.  The reader can use any data set they wish, but
the examples in the remaining sections will assume the patients data
set is being used.

You can download a data set with 10,000 documents by following these
instructions:

* Right click `this link <https://github.com/damonLL/tutorial_files/raw/master/patients>`__
  and save the file as ``patients``.  This is a 9 MB JSON file.

* If your operating system named the file something other than
  **patients** you can either rename it or you can rename it
  inside of SlamData once it has been uploaded.

* Ensure the SlamData UI is in the devdb database, and click
  the Upload icon.  |Upload|

* In the file dialog find the patients file and submit it.

* After successful upload a new collection should appear in the UI
  like the following:

|After-Upload|

As you can see it is easy to import JSON and CSV data into
SlamData quickly.  The underlying database in this case is
MongoDB.


.. attention:: Indexing Your Database

  While this step is not exactly necessary, any database without
  indexes is going to perform slowly.  In SlamData this can be
  seen as a delay in displaying results.  If you choose to skip
  this step be prepared to wait several seconds while MongoDB
  performs your searches.


The following commands are specific to MongoDB and must be executed
from the ``mongo`` shell console.

::

    use devdb
    db.patients.createIndex({first_name:1})
    db.patients.createIndex({middle_name:1})
    db.patients.createIndex({last_name:1})
    db.patients.createIndex({city:1})
    db.patients.createIndex({county:1})
    db.patients.createIndex({state:1})
    db.patients.createIndex({zip_code:1})
    db.patients.createIndex({street_address:1})
    db.patients.createIndex({height:1})
    db.patients.createIndex({weight:1})
    db.patients.createIndex({age:1})
    db.patients.createIndex({gender:1})
    db.patients.createIndex({last_visit:1})
    db.patients.createIndex({previous_visits:1})
    db.patients.createIndex({previous_addresses:1})
    db.patients.createIndex({codes:1})
    db.patients.createIndex({"codes.code":1})
    db.patients.createIndex({"codes.desc":1})


Congratulations!  There is now a usable dataset in your database
that is full of complex, nested data that you can explore.  Let's
start!


2.6 Exploring Data
~~~~~~~~~~~~~~~~~~

To simply look around and explore data, you can click on any file
(collection) that you see.  Start by clicking on the **patients**
file.

You'll be prompted to provide a name for a new Workspace.  A
Workspace is how users interact with the actual data within the
database.  Let's start by calling this ``My First Test`` or something
similar and clicking **Explore**

|Name-Workspace|

Once you click Explore, the following screen should appear:

|First-Explore-Annotated|

+--------+---------------------------------------------------------------------------------------+
| Number | Description                                                                           |
+========+=======================================================================================+
|     1  |  Zoom icon takes user back out of the Workspace and back to the database screen.      |
+--------+---------------------------------------------------------------------------------------+
|     2  |  Flip the card over for more options.                                                 |
+--------+---------------------------------------------------------------------------------------+
|     3  |  Card grips.  Slide these left or right to see the previous card or create a new one. |
+--------+---------------------------------------------------------------------------------------+
|     4  |  Browse controls for the current card.                                                |
+--------+---------------------------------------------------------------------------------------+
|     5  |  Your position within the deck. Gray circle indicates your place, white circles are   |
|        |  available to view.                                                                   |
+--------+---------------------------------------------------------------------------------------+

Feel free to click around on the browse arrows at the bottom to flip through the pages of
data.  It's easy to get an idea of the schema of this data set by looking at the top row.
In this case you can also see that the **codes** field is not actually a simple field but
an array of other documents!  Each of those documents in turn have a **code** and **desc**
field.

.. hint:: Workspace Usage

  You may not know it, but you actually just created a Workspace and a Root Deck,
  which contains an **Open Card** and an **Explore Card**!  SlamData did this
  automatically to save you time.

Any changes made within a Workspace are saved automatically.
At any time the user may zoom out of the current window.


2.7 Searching Data
~~~~~~~~~~~~~~~~~~

Viewing and browsing the data is helpful but data becomes less useful if you can't
find what you're looking for.  SlamData has two very powerful ways of finding
the data you need.  One is the **Search Card** and the other is the
**Query Card**.   We'll start with the **Search Card**.

* Click the **Flip Card** Icon (#2 in previous image)

You'll see the following options on the back of that card:

|Card-Back|

* Click on **Delete Card**

The UI will now show the only remaining card in the deck which is the
**Open Card**.  This card allows you to select which collection you wish
to operate on with subsequent cards.  Let's leave this card in place.

* Click and drag the right-hand grip and slide it to the left.

You'll be presented with the following card types to choose from:

|Card-Choices-1|

Notice how the cards are different shades of gray.  The dark gray cards
are those that can be created directly after the **Open Card**.  Light
gray cards are those cards that cannot be used following the previous
card.  A helpful checkmark in the upper right of each selection also
indicates which cards can be used in the current situation.

* Select the **Search Card**

A new **Search Card** will appear in the UI.  The search string appears
simple but has some very powerful search features within.

* Click and drag the right grip bar and slide it to the left, to
  create a new card.

* Select **Show Table Card**

Now that we have a card that can display search results, slide back
to the **Search Card**.

* Type the word ``Austin`` and either drag the right grip bar
  to the left, or simply click on the right grip bar.

Depending on the performance of your system and database it may take
several seconds before the results are displayed.  Keep in mind that
SlamData is searching the patients collection that we imported into
MongoDB, and that indexes can significantly boost performance for
searches.

Once the results appear, you can browse them just like you did earlier
in the **Explore Card** with the controls in the bottom left of the
interface.

Did you notice that in the search string earlier we did not specify
which field we wanted to search?  That is part of the power of SlamData.
Relatively non-technical users can use SlamData to search all of
their datasources with little (or even no) knowledge in advance of the data
stored within.

Of course when searching all available fields for the search string
it is going to take longer than if we were to explicitly define which field.
Let's go back to the search card by dragging the current card
to the right again, or single-click on the left grip.

Let's search for any patients currently living in the city of Dallas.

* Type the string ``city:Dallas`` and slide back to the **Table Card**

The results should have appeared much faster than the previous search
because we told SlamData to only look at the **city** field.

We can also search on non-string values such as numbers.  Let's find
all of the patients who are between the ages of 45 and 50:

* Go back to the **Search Card**

* Enter the string ``age:>=45 age:<=50``

* View the results in the **Table Card** again.

As one last example let's show how you can mix and match different types.
We want to know how many males over age 50 used to live in California.

* Go back to the **Search Card**

* Enter the string ``previous_addresses:"[*]":state:CA age:>50 gender:=male``

* View the results

See the table below for some helpful query examples:


+---------------------------+---------------------------------------------------------------+
| Example                   | Description                                                   |
+===========================+===============================================================+
| ``colorado``              | Searches for the **substring** ``colorado`` in **all fields** |
+---------------------------+---------------------------------------------------------------+
| ``=colorado``             | Searches for the **full word** ``colorado`` in **all fields** |
+---------------------------+---------------------------------------------------------------+
| ``age:=50``               | Searches the field **age** for a value of 50                  |
+---------------------------+---------------------------------------------------------------+
| ``age:>=50``              | Searches the field **age** for any value over 50              |
+---------------------------+---------------------------------------------------------------+
| ``age:>=50 age:<=60``     | Searches the field **age** for values between or equal to     |
|                           | 50 and 60                                                     |
+---------------------------+---------------------------------------------------------------+
| ``codes:"[*]":desc:flu``  | Performs a deep search through the **codes** array and        |
|                           | examines each subdocument's **desc** field for the            |
|                           | **substring** ``flu``                                         |
+---------------------------+---------------------------------------------------------------+

As you can see even users with no knowledge of SQL² can perform powerful
searches within SlamData!  


2.8 Querying Data with SQL²
~~~~~~~~~~~~~~~~~~~~~~~~~~~

In addition to the **Search Card** SlamData provides a **Query Card** which
allows users to execute ANSI-compatible SQL queries on top of any data source,
including NoSQL databases!  This is accomplished by using SlamData's SQL²
dialect, which is a superset of SQL that allows dynamic modeling and querying
of deeply nested, semi-structured data.

Using the same dataset we are going to perform queries, moving from basic
queries to more advanced queries.  Let's start off by cleaning up our
Workspace.

* Go to the **Table Card**

* Flip it over

* Click on **Delete Card**

This should take you to the **Search Card**

* Flip it over

* Click on **Delete Card**

This should take you to the **Open Card**.  We will be using full
path names in the queries we will write, and **Query Cards** do not
use the **Open Card** so let's get rid of that one as well.

* Flip it over

* Click on **Delete Card**

* Create a new **Query Card**

The UI now presents the **Query Card**.  Within this card users can
enter simple or very long and complex SQL² queries against one,
two or more collections.

Before we perform any real queries, leave the existing contents
of the card as the default.  Let's create a **Table Card** to the right
of this one so when the queries execute, we can see the results.

* Click the right grip.

* Create a new **Show Table Card**

* Now click back to the **Query Card**

* Type in the following query:

.. code-block:: sql

    SELECT * FROM `/devguide/devdb/patients`

Notice how the path to the dataset is surrounded by
back-ticks (`````) not apostrophes (``'``)

* Slide over to the **Show Table Card** to see the results.

* Slide back to the **Query Card**

* Type in or paste the following query:

.. code-block:: sql

    SELECT
        first_name,
        last_name
    FROM `/devguide/devdb/patients`
    WHERE
        state="TX" AND
        city="DALLAS"

Note that the query can span multiple lines, and that strings
are surrounded by quotation marks (``"``) on both ends.  This
is a requirement for all string data types.

* Slide back to the **Show Table Card** to see the results.

* Slide back to the **Query Card**

Let's now create a query that formats the results a little cleaner:

* Type in or paste the following query:

.. code-block:: sql

    SELECT
        last_name || ',' || first_name AS Name,
        city AS City,
        zip_code AS Zip
    FROM `/devguide/devdb/patients`
    WHERE
        state="TX"
    ORDER BY zip_code ASC

* Slide to the **Show Table Card** to see the results.

Notice in this query we are concatenating **last_name** and
**first_name** fields together, separated by a comma.  The comma
itself is surrounded by apostrophes (``'``) because it is a single
character.  If it was more than one character it would be a string
and would require full quotation marks around it.

We have also given the results some aliases to display rather
than the actual field names.

Finally we are ordering (**ORDER BY**) the results in ascending (**ASC**)
order based on the **zip_code** field.

The results table should now look similar to the following image:

|Zip-Results|

Up to this point we have been using SQL² to query simple *top-level* fields,
or those fields which are not nested.  We know from previous examples
that this data set stores nested data in the **codes** array, but 
it also contains **previous_addresses** and **previous_visits** arrays.

Let's find out the total number of male and female patients
from each state that have an illness related to an ulcer. This will
require using the flattening operator (``[*]``) so SlamData
can examine all of the documents in the **codes** array.

* Slide to the **Query Card**

* Type or paste the following query:

.. code-block:: sql

    SELECT
        state AS State,
        gender AS Gender,
        COUNT(*) AS Count
    FROM `/devguide/devdb/patients`
    WHERE
        codes[*].desc LIKE "%ulcer%"
    GROUP BY state, gender
    ORDER BY COUNT(*) DESC
    LIMIT 20

* Slide to the **Show Table Card** to see the results.

SQL² allows for very complex queries.  You can find out more by
reviewing the `SQL² Reference <sql-squared-reference.html>`__.
Additional features include using the **JOIN** command to combine data
from two or more tables, utilizing variables within queries
(as explained in Section 3), using standard math operations,
retrieving not only field values but also field names
dynamically, and much more.

Now that you have a good idea of what can be accomplished with
SQL² queries, let's create some forms that your users can
interact with.  These forms can drive the results of the charts
we'll use for visualization, which makes it easy for your users
to find, report and chart complex data without understanding
the mechanics behind it!


Section 3 - Interactive Forms and Visualizations
------------------------------------------------

SlamData provides everything you need to create an interactive
visual analytics environment for your users.

From this point on in the guide we will assume that we
are creating an environment for medical facilities to search
through patient data for various reasons.  The Workspaces we
create will be used by medical staff for this purpose.


3.1 Static Markdown Forms
~~~~~~~~~~~~~~~~~~~~~~~~~

We will start this section with a new Workspace.  You can leave
the existing Workspace alone or you can delete it if you wish.

To (optionally) delete the existing Workspace:

* If you are still in the Workspace, click on the zoom-out
  icon |Zoom-Out|

* Locate the **My First Test** Workspace and hover your mouse over it.

* Click on the trash can icon that appears to the right |Trash-Can|

We'll create a new Workspace and call it **Average Weight by City**

* Click the Create Workspace icon in the upper right |Create-Workspace|

* Select the **Setup Markdown Card**

This step is necessary so that the Workspace is saved and we can go
back to rename it soon.

* Create a **Show Markdown** card directly after the **Setup Markdown Card**

* Zoom back out to the database view

Let's rename the Workspace now so it's obvious that we are working
with it.

* Hover over the new Workspace labeled **Untitled Workspace.slam**

* Click the Move/Rename icon to the right |Move-Rename|

* Replace **Untitled Workspace** with ``Average Weight by City``
  and click **Rename**

* Click on the **Average Weight by City.slam** Workspace again

We are now back in the **Setup Markdown Card**.

SlamData uses a specific form of `Markdown <https://daringfireball.net/projects/markdown/>`__ 
sometimes referred to
as SlamDown.  Markdown allows a user to format text with a few
simple syntax rules.  SlamData's version also allows UI elements
(such as drop downs, radio buttons and check boxes) to be dynamically
populated from the results of queries.

Let's first show some examples of what the Markdown forms can do.
Replace the text within the card with the following:

::

    # Heading 1

    ## Heading 2

    ### Text formatting

    * Here is an unnumbered list.
    * You can have _emphasized_ and **bold** text.

    1. Here is a numbered list.
    2. Here is the second entry with ```inline formatting```

    Paragraphs are separated by
    an empty line.

    This is another new paragraph.

    > You can also have some nice
    > block quote areas.

    You can also have fenced code blocks like this:

    ```
    SELECT * FROM `/devguide/devdb/patients`
    WHERE
      first_name = "Sue"
    ```

    ### Interactive Elements

    #### Input Fields

    name = ____ (Sue)

    numberOnly = #____ (1984)

    #### Selectors

    city = {Austin, Dallas, Houston}

    favoriteColor = (x) red () blue () green

    computers = [] PC [x] Mac [x] Linux

    beginDate = ____-__-__

    stopTime = __:__

    fullDateTime = ____-__-__ __:__


* Click over to the **Show Markdown Card** to view the results.

Notice how much control you have over the presentation of
the information.  You can also include links and images inside
of Markdown as well.  For a full description of all fields
and their behavior see the `SlamDown Reference <slamdown-reference.html>`__.

* Click back to the **Setup Markdown Card**

Replace the contents with something more useful and appropriate
to our use case:

::

    ## General Patient Information

    There are !`` SELECT COUNT(*) FROM `/devguide/devdb/patients` `` patients

    _Average_ age: !`` SELECT AVG(age) FROM `/devguide/devdb/patients` ``

    The *Heaviest* patient: !`` SELECT MAX(weight) FROM `/devguide/devdb/patients` `` pounds

    The **Shortest** patient: !`` SELECT MIN(height) FROM `/devguide/devdb/patients` `` inches


* Click over to the **Show Markdown Card** to see the results.

Notice that we populated some of the text with actual results from the database.
Keep in mind that to print the results of a query in Markdown, the query must
begin with an exclamation point (``!``) and two back-ticks (``````) and end
with two more back-ticks (``````).

* Click back to the **Setup Markdown Card**

We will use similar syntax to populate the elements of an interactive form
in the next section.



3.2 Interactive Markdown Forms
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Here is where things get really fun for both you and your own users.
Let's actually provide the functionality that we promise with the
title of **Average Weight by City**.

First we want the user to select the state to report on.  This will
then allow us to query the database for patients that reside in
cities within that state.

* Replace the contents of the current **Markdown Setup Card**
  with the following code.

::

    ### Select the state to report on

    state = {!``SELECT DISTINCT(state) FROM `/devguide/devdb/patients` ORDER BY state``}

* Click over to the **Show Markdown Card** to see the results.

* Click on the dropdown next to **State** to see that the element
  was populated with the query we typed in.

* Flip the **Show Markdown Card** over by clicking the icon in the upper right |Icon-Flip|

* Select the **Wrap** option.

Note that your interface should now look similar to the following:

|Wrapped-Deck|

You can drag the existing deck around the board now.  You can also click and
drag the left and right hand grips just as before to see the previous cards.

* Click on the deck to make it active.

* Flip the deck by clicking the icon |Icon-Flip|

* Select the **Mirror** option.

Your interface should now look similar to the following:

|Mirrored-Deck|

We have just mirrored a deck.  This means that the second deck starts off
from where the first left off, but it also means any changes to the first
deck will immediately impact the second deck as well.  This is how
we chain events in a Workspace and allow the actions in one deck to
affect other decks.

* Click on the new second deck to make it active.

* Create a new card in this second deck, selecting the **Query Card**

* Type in or paste the following query into the **Query Card**:

.. code-block:: sql

    SELECT
      city AS City,
      AVG(weight) AS AvgWeight
    FROM `/devguide/devdb/patients`
    WHERE
      state IN :state[_]
    GROUP BY
      city
    ORDER BY AVG(weight) DESC

One new feature we see here is the use of **:state[_]**.  Whenever a
variable from a Markdown form is used in a query it must be
preceded by a colon ( ``:`` ).  Some variables may also require special
syntax after the name as well.  In this case since we are using an array of
states we had to add the ``[_]`` suffix to the variable name.

Also note that we can **ORDER BY** an aggregation value such as **AVG**.

* Click on the right grip to create a new card and select **Show Table Card**

* Adjust the decks with their border controls until they look similar
  to the following image:

|MD-and-Show-Decks|

* Select a different state in the first deck and watch the results
  table update automatically.

Viewing data in table form is useful but sometimes a graphical representation
makes all the difference.  To prepare for that, let's go back and change
query and limit the results to 20 cities so a bar chart doesn't appear as
crowded.

* Click the left grip to go back to the **Query Card**

* Add the following line to the end of the query:

.. code-block:: sql

  LIMIT 20

* Slide back over to the **Show Table Card**

Now we are ready to add some visualizations!


3.3 Creating a Chart
~~~~~~~~~~~~~~~~~~~~

Before creating an actual chart we need to set it up.  Remember earlier
that decks can build off one another.  We need to now mirror the
**Show Table Card**:

* Click on second deck to make it active

* Click on the flip icon to flip the deck over |Icon-Flip|

* Select the Mirror option.

* Drag the newly mirrored deck to the right and resize it so your interface
  looks similar to the following image:

|All-3-Decks|

* Flip the new deck over and now select the **Setup Chart** option

* Select the Bar Chart icon on the left |Icon-Gray-Bar-Chart|

The bar chart icon will change from gray to blue to show that it is active.

* In the **Category** drop down select **.City** as the axis source

* Slide to the right to create a new card and select the **Show Chart** option

Your interface should now look like the following image:

|All-3-With-Chart|

* Select a new state in the first deck and watch both of the other
  decks update dynamically.

* Try hovering your mouse over the individual bars in the chart and you can
  view the actual value.

Setting up interactive forms and charts is as simple as that!  In the next
section we'll go over how to share these charts with others.


Section 4 - Publishing and Simple Embedding
-------------------------------------------

4.1 - Publishing
~~~~~~~~~~~~~~~~

SlamData makes it easy to take all the work you've done up to this
point and publish it so that others can use it as well.

* Click the flip icon on the **Draftboard Card**.  Note that this
  is the card that contains all of the existing decks.  Just as
  each deck has a back to it, each card does as well, including
  the **Draftboard Card**.  Be sure not to flip any of the three
  decks we've created - click the icon in the white box border
  surrounding the other decks.

* Select the **Publish deck** option.

A URL will be presented to you that you can share with others.
The URL will only be accessible while SlamData is running.

.. warning:: Published URLs

  Anyone with access to the URL may be able to view this deck. They may also be able
  to modify the link to view or edit any deck in this workspace. Please see
  Securing SlamData Community Edition for more information.

  **NOTE**: SlamData Advanced Edition provides complete security including
  authorization, authentication and full auditing.  


4.2 - Simple Embedding
~~~~~~~~~~~~~~~~~~~~~~

SlamData allows content authors and developers to embed Decks into
external web applications such as customer portals, dashboards, etc.

4.2.1 - Downloading Sample Code
'''''''''''''''''''''''''''''''

For examples of how to do this go to this |Repo-Link|.  You can either download
the zip file or clone the repository

**Option 1 - Download Zip File**

* Click the |Repo-Link|.

* Click the green **Clone or download** button.

* Select **Download ZIP**

* Unzip the contents once downloaded

**Option 2 - Clone the Repository**

You will need to install `git <https://git-scm.com/downloads>`__ and then
type the following in a command line terminal:

.. code-block:: shell

    git clone https://github.com/slamdata/slamdata-dev-examples.git
    cd slamdata-dev-examples

This section will be using the **sample1** code from that repository.

* Open a web browser and open the **sample1/index.html** file.

In this mock-up app we are going to simulate a reporting application that allows
healthcare professionals to run a few reports based on patient data.  You can see
the in this example we will have two reports.

4.2.2 - Sample Report 1
'''''''''''''''''''''''

We have already done most of the work for the first report, we just need to
embed the appropriate code from SlamData into the web application.  Again - this
is a mock-up application which does not actually generate dynamic web pages, so
we will be modifying static HTML files to simulate this.  The guide will point
out relevant areas in code that should be generated by your application.

* If not already open then navigate to the **Average Weight by City** Workspace

* Flip the **Draftboard Card** over (again - this is the card that surrounds all
  of the decks with a white border)

* Select the **Embed Deck** option

Notice that SlamData provides sample code to copy and paste into your own
application or HTML file.


4.2.2.1 Snippet 1 Code
@@@@@@@@@@@@@@@@@@@@@@

* Copy the highlighted part of the text (see image below).

|Embed-Code-1|


* Open the **sample1/report1.html** file in a text editor

* Paste the **Snippet 1 code** that SlamData provided into the HTML file's ``<HEAD>`` section,
  just after the line that reads ``<!-- SLAMDATA SNIPPET 1 -->``.

Let's refer to this section of code as **Snippet 1**.

**Snippet 1** should be placed within the HTML file's <HEAD>
tags as it's a JavaScript snippet.  This section of code can
easily be inserted into individual HTML files, or you can save it
to it's own JavaScript (.js) file to include in many documents.

This snippet is generic and is typically the same regardless of
what is being embedded - which makes it a great candidate to
save into that JS file and insert into multiple web pages based on
your web application framework.

You'll see with Snippets 2 and 3 how we control what is being seen
even though the code in this snippet is generic.


4.2.2.2 Snippet 2 Code
@@@@@@@@@@@@@@@@@@@@@@

* Go back to the SlamData UI.  Scroll down until you see the next section of
  sample code, highlighted in the image below.

|Embed-Code-2|

* Copy the ``id`` value from the <div> element. It starts with ``sd-deck-``.

* Go back to your text editor, and replace the text ``REPLACE_ME``
  with the copied value.  This should be in the section directly below
  ``<!-- SLAMDATA SNIPPET 2 -->``.

One important piece to note here is that the example **report1.html** file
is formatted with some CSS and <div> tags already.  In your own application
you can either paste the entire line of code that SlamData provides, or create
your own <div> tag and programmatically insert the id as we did in this example.


4.2.2.3 Snippet 3 Code
@@@@@@@@@@@@@@@@@@@@@@

* Go back to the SlamData UI.  Scroll down until you see the next section of
  sample code, highlighted in the image below.

|Embed-Code-3|

* Copy the highlighted text as shown above.

* Go back to your text editor, and paste the contents of **Snippet 3 code** directly
  below the line that reads ``<!-- SLAMDATA SNIPPET 3 -->``.

* Save your **sample1/report1.html** file to disk.

This is the code that provides the most important information when embedding
the Deck.  Notice the variables ``deckPath`` and ``deckId``.  This section of code
would normally be generated by your own web application, and these two variables
would be populated based on some logic in your application.

In small examples where we are only using two reports it's easy enough to paste
this code directly into files; however when the number of reports that are being
embedded grows, it will quickly start to make sense when to programmatically
generate this code.

4.2.2.4 Full Code - Report 1
@@@@@@@@@@@@@@@@@@@@@@@@@@@@

After making changes to the **sample1/report1.html** file and saving it,
it should appear almost identical to the following.  The differences will
only be related to your local environment, such as possibly the hostname,
the deckId, sd-deck value, etc.

Code:

.. code-block:: html

    <head>
      <meta charset="utf-8">
      <title>Your Reporting App</title>
      <link rel="stylesheet" type="text/css" href="styles.css">

      <!-- SLAMDATA SNIPPET 1 -->

      <script type="text/javascript">
      var slamdata = window.SlamData = window.SlamData || {};
      slamdata.embed = function(options) {
        var queryParts = [];
        if (options.permissionTokens) queryParts.push("permissionTokens=" + options.permissionTokens.join(","));
        if (options.stylesheets) queryParts.push("stylesheets=" + options.stylesheets.map(encodeURIComponent).join(","));
        var queryString = "?" + queryParts.join("&");
        var varsParam = options.vars ? "/?vars=" + encodeURIComponent(JSON.stringify(options.vars)) : "";
        var uri = "http://localhost:8080/files/workspace.html" + queryString;
        var iframe = document.createElement("iframe");
        iframe.width = iframe.height = "100%";
        iframe.frameBorder = 0;
        iframe.src = uri + "#" + options.deckPath + "/" + options.deckId + "/view" + varsParam;
        var deckElement = document.getElementById("sd-deck-" + options.deckId);
        if (deckElement) deckElement.appendChild(iframe);
      };
      </script>

    </head>
    <body>
      <div class="container">
        <nav class="navbar navbar-default" role="navigation">
              <div class="navbar-header">
                <div class="row">
                  <a class="navbar-brand" href="index.html"><img width="10" src="images/spacer.png"/></a>
                    <a class="navbar-brand" href="index.html"><img src="images/dashboard.svg"/></a>
                  </div>
                  <div class="row">
                  <a class="navbar-brand" href="index.html"><img width="10" src="images/spacer.png"/></a>
                    <a class="navbar-brand" href="index.html">Your Reporting App</a>
                  </div>
              </div>
          </nav>
        <div id="main">
          <div class="container">
            <div class="row">
              <div class="col-md-6">
                <H3>Average Weight by City</H3>
              </div>
            </div>

             <!-- SLAMDATA SNIPPET 2 -->

            <div
                style="min-height: 700px;min-width: 800px;"
                class="col-lg-12 col-md-12 col-sm-12"
                class="row"
                id="sd-deck-5e2ce240-bb3f-4aca-8471-dae06925a429">

            </div>
          </div>
        </div>
      </div>

      <!-- SLAMDATA SNIPPET 3 -->

      <script type="text/javascript">
        SlamData.embed({
          deckPath: "/devguide/devdb/Average+Weight+by+City.slam/",
          deckId: "5e2ce240-bb3f-4aca-8471-dae06925a429",
          // An array of custom stylesheets URLs can be provided here
          stylesheets: []
        });
      </script>

    </body>



4.2.2.4 Overview of Report 1
@@@@@@@@@@@@@@@@@@@@@@@@@@@@

Now that the **sample1/report1.html** file has been saved, it can be loaded
into the web browser.

* Go back to the browser where **sample1/index.html** is displayed,
  or optionally re-open the file with the browser.

* Click on the **Average Weight by City** link.  It should appear similar
  to the image below

* Observe how the entire contents of that Deck is now being displayed
  in a third party web application.

|Sample-1-1-Full-Report|

The purpose of copying and pasting all of the values in the file above
was to show what a completed web page is comprised of, including the
code to make the calls to SlamData.

A larger web application would typically generate the entire contents
of **sample1/report1.html**, replacing the relevant values in
**Snippet 2** and **Snippet 3**.   Again, **Snippet 1** can simply be
saved as a JS file and included in the necessary pages within the application.


4.2.3 - Sample Report 2
'''''''''''''''''''''''

This section will give you the relevant information for creating a new
Workspace, Deck and report, but will not give you the full instructions.

From your previous work you understand how to create a Workspace, rename
it, add cards, etc.  The list below shows the necessary cards you'll need to create
and their order.  Remember you'll need to **Wrap** everything to be able
to move the individual decks around.

**Initial Card Order**:

    1. Query Card (wrap the deck here)

    Query:

        .. code-block:: sql

            SELECT
              COUNT(*) as Count,
              state,
              gender
            FROM `/devguide/devdb/patients`
            WHERE
              codes[*].desc like "%ulcer%"
            GROUP BY state, gender

    2. Show Table Card (mirror the deck here)


**Mirrored Deck Card Order**

1. Setup Chart Card

    * Bar Chart
    * Category: .state
    * Series: .gender

2. Display Chart Card

The results should look similar to the following image:

|Report-2-Workspace|

Copy all of the relevant data from the **Embed Deck** option and paste
it into the **sample1/report2.html** file.  Once it is saved, you
can click on the **Ulcer-related Illnesses by Gender** report in the
mock-up app and see something similar to the following image.  Note that
in this image the user would need to scroll right to see the full chart.

|Sample-1-2-Full-Report|


Section 5 - Secure Embedding
----------------------------

This section describes how to enable user authorization and authentication
with examples.  This not only provides security when users are within
the SlamData user interface but can also be used to control access
from other web applications as well.

.. attention:: SlamData Advanced Required

  |Murray-Small| This section requires SlamData Advanced Edition

This section assumes you understand the basics of SlamData
Advanced Edition security
`here <http://docs.slamdata.com/en/v3.0/administration-guide.html#security-overview>`__

SlamData Advanced Edition utilizes `OpenID Connect <http://openid.net/connect/>`__,
which is a simple identity layer on top of the OAuth 2.0 protocol.

5.1 Bootstrapping Security
~~~~~~~~~~~~~~~~~~~~~~~~~~

If you have already setup authentication for SlamData you may skip this section.

To enable user security a default administrator group must be created along with
a user email.  In the next step this user will be provided all permissions
within SlamData.  This allows the user to perform administration tasks within
the user interface as well as make calls via the SlamData API that require
elevated privileges.

From the SlamData Advanced Edition directory, type the following to bootstrap
the SlamData Advanced Edition environment, replacing the email address with
the user you wish to authenticate with.

```
java -jar jars/quasar.jar bootstrap -u you@example.com -g admin
```


5.2 Creating an OIDC Provider
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you have already setup an OIDC provider you may skip this section.

At least one OpenID Connect (OIDC) Provider must be listed in the configuration
file for SlamData Advanced Edition.   This OpenID Connector Provider (OP) will be
trusted by SlamData for authentication information. 

The remainder of this guide will assume that a Google OP will be used and the
examples are configured based on this assumption; however,
any OpenID Connect Provider can be used.

5.2.1 Google OIDC Provider
''''''''''''''''''''''''''

The best method to create an OP is to follow instructions from the
Google API Console project `here <https://developers.google.com/identity/sign-in/web/devconsole-project>`__

Most of the fields should be self explanatory.  Once the project is created, go to the
Credentials tab in the API Manager.  Under the **Authorized redirect URIs** enter the following
value and save your changes, assuming hostname and port are correct for your environment:

.. code-block:: shell

http://localhost:8080/files/auth_redirect.html


In SlamData's quasar-config.json file create a new entry similar based off the client_id,
similar to to the highlighted portion in the image below:

|Config-Example|

Restart SlamData Advanced Edition so the new provider will be active.

5.3 Logging Into SlamData
~~~~~~~~~~~~~~~~~~~~~~~~~

You should now be able to click on the application tab bar pull out at the top of the page.

|Header-Grip|

You can then click on the **Sign In** icon to the right.

Once clicked it should display all of the OIDC Providers that are configured, similar
to the image below:

|Sign-In|

Sign in with the user you specified in the bootstrap step above.  This user has
complete access to all SlamData Advanced Edition functionality.

5.4 New Decks for Secure Embedding
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In this section we're going to spend time setting up SlamData so that multiple
customers can utilize it from an external web application.  This will require
creating SQL² Views, new Workspaces and permission tokens.

Additionally we'll configure SlamData so that reports and views are now stored
in a separate directory structure for enhanced security.

5.4.1 Setting up SQL² Views
'''''''''''''''''''''''''''

In this simulated application we will assume we are a national
healthcare provider.  We also want to create some reports for
our healthcare professionals; however, those reports must be limited
to the states to which the healthcare professional is licensed.

One option would be to create a report for each state, and specify access
to that report for each of that state's healthcare professionals.  Now
consider we would have to do that for **each report type**.  So if one report type was
**Average Age by City**, we would have to create 50 of those reports, and
then provide access to each professional in each state.
Then if we wanted another report called **Most Diagnosed Disease**
we would have to create yet another 50 reports, one for each state, and
setup the professionals to view it again.

The better answer to this is to create a single report, and change
the source data set based on who is logged in.  This is accomplished
through the use of a view.  Let's set one up as an example.

In SlamData, navigate to the root folder.  We have primarily been
working in the **/devguide/devdb** database which means we'll need
to go up two levels.

From the main Home page in SlamData, to the ``devguide`` mount,
then into the ``devdb`` database where the previous Workspaces
were created, similar to this image:

|Navigate|

* Click on the Create Folder icon |Create-Folder|

* Hover over the **Untitled Folder** and click the Move-Rename icon to the right |Move-Rename|

* Rename the folder to ``state-views``

Now we have a folder which is specifically designed to hold views.  This makes
it easier to manage.

Now let's create our first view.

* Click into the **state-views** folder

* Click on the Mount icon |Icon-Mount|

* In the mount dialog provide ``colorado`` as the name

* Select ``SQL²`` as the mount type

* Paste or type the following query into the **SQL² query** field:

.. code-block:: sql

    SELECT * FROM `/devguide/devdb/patients` WHERE state = "CO"

* Click **Mount**

Congratulations, you just created a view!  Now this view path can
be used in queries.  When this view is used as the data source,
the results will only be those documents where the ``state``
field is ``CO``.

What we just did can also be accomplished via the SlamData API
quite easily.  This is covered in the SlamData API Reference.
To create a view for each of the 50 states would take some time
through the user interface (even with the API), so let's create
just one more view to use.

* Create another view named ``texas`` that queries against the
  ``state`` field for the value of ``TX``

We'll now use the **colorado** and **texas** views as the data
sources for some of our reports.


5.4.2 Setting up the Reports
''''''''''''''''''''''''''''

Just like we setup a special folder for the state-views, we
will now setup a special folder for the reports we wish
to securely embed into third party web applications.

* Navigate back to the **/devguide/devdb** location within SlamData

* Create a new folder and rename it ``reports``

* Click into the **reports** folder

We are only going to create a single report but this process can
of course be repeated for as many reports as you like.  This report
will make use of the views we created previously.

* Click on the Create Workspace icon |Create-Workspace|

* Create a **Setup Variables Card**

* Provide the values from the following table:

+---------------+-------------------------------------------+
| Field         | Value                                     |
+===============+===========================================+
| Name          | ``viewpath``                              |
+---------------+-------------------------------------------+
| Type          | **SQL² Identifier**                       |
+---------------+-------------------------------------------+
| Default value | ``/devguide/devdb/state-views/colorado``  |
+---------------+-------------------------------------------+

* Create a **Query Card** with the following query:

.. code-block:: sql

    SELECT
        count(codes[*]),
        _id as id,
        first_name,
        last_name
    FROM :viewpath
    GROUP BY _id
    ORDER BY count(codes[*]) DESC
    LIMIT 20

* Create a **Setup Chard Card** with the following settings:

+---------------+-------------------------------------------+
| Field         | Value                                     |
+===============+===========================================+
| Chart Type    | **Bar Chart**                             |
+---------------+-------------------------------------------+
| Category      | **.City**                                 |
+---------------+-------------------------------------------+
| Default value | ``/devguide/devdb/state-views/colorado``  |
+---------------+-------------------------------------------+

* Create a **Show Chart Card**

We've created an interesting chart.  Let's go back out and rename
the Workspace now.

* Zoom back out to the navigation screen

* Rename the **Untitled Workspace.slam** Workspace to
  ``Average Age by City``

* Click into the **Average Age by City** Workspace again

* Flip the deck |Icon-Flip|

* Select the **Embed Deck** icon

This screen should look familiar!  You'll notice that a few new entries
are now residing in the code.  Specifically the ``viewpath`` variable is
exposed.  We'll be able to change this value later to control which
data set we're looking at.

* Click on the **Include a permission token...** checkbox at the bottom
  of the code window.

Notice how the ``permissionTokens`` value is now populated within the code.
Now we are ready to securely embed this deck into our simulated web application.


5.4.3 - Setting up the Web Application
''''''''''''''''''''''''''''''''''''''

Now that we have the views and reports created we can move on to copying
the provided code into the appropriate HTML files to simulate our
healthcare web application.


5.4.3.1 Snippet 1 Code
@@@@@@@@@@@@@@@@@@@@@@

* Copy the highlighted part of the text (see image below).

|Embed-Code-Secure-1|

* Open the **sample2/report1.html** file in a text editor (note this is **sample2** now,
  not **sample1**)

* Paste the **Snippet 1 code** that SlamData provided into the HTML file's ``<HEAD>`` section,
  just after the line that reads ``<!-- SLAMDATA SNIPPET 1 -->``.

Let's refer to this section of code as **Snippet 1**.

As before, this snippet is ideal for usage in an external JS file
that can be included in multiple web pages.


5.4.3.2 Snippet 2 Code
@@@@@@@@@@@@@@@@@@@@@@

* Go back to the SlamData UI.  Scroll down until you see the next section of
  sample code, highlighted in the image below.

|Embed-Code-Secure-2|

* Copy the ``id`` value from the <div> element. It starts with ``sd-deck-``.

* Go back to your text editor, and replace the text ``REPLACE_ME``
  with the copied value.  This should be in the section directly below
  ``<!-- SLAMDATA SNIPPET 2 -->``.

One important piece to note here is that the example **report1.html** file
is formatted with some CSS and <div> tags already.  In your own application
you can either paste the entire line of code that SlamData provides, or create
your own <div> tag and programmatically insert the id as we did in this example.


5.4.3.3 Snippet 3 Code
@@@@@@@@@@@@@@@@@@@@@@

* Go back to the SlamData UI.  Scroll down until you see the next section of
  sample code, highlighted in the image below.

|Embed-Code-Secure-3|

* Copy the highlighted text as shown above.

* Go back to your text editor, and paste the contents of **Snippet 3 code** directly
  below the line that reads ``<!-- SLAMDATA SNIPPET 3 -->``.

* Save your **sample2/report1.html** file to disk.

* Now go to your browser and load **sample1/index.html**

* Click on the **Average Age by City - Colorado** link

Notice how the Deck is embedded securely inside of our simulated web application.

Try changing the secret token in the **sample2/report1.html** file and reloading
the page.  You'll notice that you receive an authentication error.

We are now going to use the exact same report, and same code but provide this
functionality to our Texas healthcare professionals as well.

From the command line inside of the repository directory, type or paste the
following command:

.. code-block:: shell

    cp sample2/report1.html sample2/report2.html

* Open the **sample2/report2.html** file with a text editor.

* Change the title of the page in the ``<H3>`` header to ``Average Age by City - Texas``

* Change the **viewpath** value toward the bottom of this file to
  ``/devguide/devdb/state-views/texas``

* Save your changes

* Open the **sample2/index.html** file again, and now click on the
  **Average Age by City - Texas** report.

Notice that with just the change of the viewpath we are able to provide this
to our Texas professionals as well.

In a real-world application we would generate the web pages represented by
**report1.html** and **report2.html**, replacing the variables where
necessary.


.. _eCharts: https://ecomfe.github.io/echarts/index-en.html


.. |Murray| image:: images/SD3/murray.png

.. |Murray-Small| image:: images/SD3/murray-small.png

.. |Home-Annotated| image:: images/SD3/screenshots/home-annotated-with-numbers.png

.. |Icon-Mount| image:: images/SD3/icon-mount.png

.. |Mount-Dialog| image:: images/SD3/screenshots/mount-dialog.png

.. |Mount-Dialog-Complete| image:: images/SD3/screenshots/mount-dialog-complete.png

.. |Create-Folder| image:: images/SD3/icon-create-folder.png

.. |Move-Rename| image:: images/SD3/icon-move-rename.png

.. |Zoom-Out| image:: images/SD3/icon-zoom-out.png

.. |Create-Workspace| image:: images/SD3/icon-create-workspace.png

.. |Upload| image:: images/SD3/icon-upload.png

.. |Trash-Can| image:: images/SD3/icon-trash-can.png

.. |Icon-Flip| image:: images/SD3/icon-flip.png

.. |Icon-Gray-Bar-Chart| image:: images/SD3/icon-gray-bar.png

.. |In-Devdb| image:: images/SD3/screenshots/in-devdb-clean.png

.. |After-Upload| image:: images/SD3/screenshots/after-upload.png

.. |Name-Workspace| image:: images/SD3/screenshots/name-workspace.png

.. |First-Explore-Annotated| image:: images/SD3/screenshots/first-explore-annotated.png

.. |Wrapped-Deck| image:: images/SD3/screenshots/wrapped-deck.png

.. |Mirrored-Deck| image:: images/SD3/screenshots/mirrored-deck.png

.. |Card-Back| image:: images/SD3/screenshots/back-of-card.png

.. |Card-Choices-1| image:: images/SD3/screenshots/new-card-choices-1.png

.. |MD-and-Show-Decks| image:: images/SD3/screenshots/md-and-show-decks.png

.. |All-3-Decks| image:: images/SD3/screenshots/all-3-decks.png

.. |Zip-Results| image:: images/SD3/screenshots/zip-results.png

.. |All-3-With-Chart| image:: images/SD3/screenshots/all-3-with-chart.png

.. |SD-Nesting| image:: images/SD3/screenshots/sd-nesting.png

.. |Embed-Code-1| image:: images/SD3/screenshots/embed-code-1.png

.. |Embed-Code-2| image:: images/SD3/screenshots/embed-code-2.png

.. |Embed-Code-3| image:: images/SD3/screenshots/embed-code-3.png

.. |Sample-1-1-Full-Report| image:: images/SD3/screenshots/sample-1-1-full-report.png

.. |Report-2-Workspace| image:: images/SD3/screenshots/report-2-workspace.png

.. |Sample-1-2-Full-Report| image:: images/SD3/screenshots/sample-1-2-full-report.png

.. |Config-Example| image:: images/SD3/screenshots/config-example.png

.. |Header-Grip| image:: images/SD3/screenshots/header-grip.png

.. |Sign-In| image:: images/SD3/screenshots/sign-in.png

.. |Navigate| image:: images/SD3/screenshots/navigate.png

.. |Embed-Code-Secure-1| image:: images/SD3/screenshots/embed-code-secure-1.png

.. |Embed-Code-Secure-2| image:: images/SD3/screenshots/embed-code-secure-2.png

.. |Embed-Code-Secure-3| image:: images/SD3/screenshots/embed-code-secure-3.png

.. |Sample-2-1-Full-Report| image:: images/SD3/screenshots/sample-2-1-full-report.png

.. |Repo-Link| raw:: html

   <a href="https://github.com/slamdata/slamdata-dev-examples" target="_blank">repository link</a>

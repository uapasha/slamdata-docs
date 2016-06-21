.. figure:: /images/white-logo.png
   :alt: SlamData Logo

SlamData Tutorial
=================

Introduction
------------

This tutorial will step the user through many of the features in
SlamData within the context of a Notebook. The tutorial starts with
providing the data needed to perform the steps and continues through the
end where a chart will be generated based on a dynamic user form.

--------------

Setup
-----

The examples in the tutorial will reference the path
``/local/demo/tutorial`` which means a mount point of ``/local`` is
created, which has a database called ``demo`` and a collection called
``tutorial``.

1.  `Download <https://raw.githubusercontent.com/damonLL/tutorial_files/master/tutorial>`__
    and save the tutorial file. This file contains information about
    which countries won medals in the Olympics as far back as 1924.
2.  Start the SlamData application
3.  Click on a server that has been configured. See the `Connecting to a
    Database <administration-guide.html#connecting-to-a-database>`__
    section of the Administration Guide if no servers exist.
4.  Click the New Folder |New Folder| icon in the upper right. A new
    folder called ``Untitled Folder`` now exists.
5.  Rename the folder to ``demo`` by hovering the mouse over the
    Untitled Folder and then clicking on the rename icon that appears
    seen here: |Rename Folder|
6.  Click on the renamed folder to open it.
7.  Click on the Upload |Upload Icon| icon.
8.  Locate and select the ``tutorial`` file in step 1. After a few
    moments a new Exploration cell will be displayed.
9.  Click the Back |Back Icon| icon in the upper left to view the folder again.

Your folder display should appear as follows:

.. figure:: /images/screenshots/tutorial-folder-display.png
   :alt: Tutorial Folder Display

Depending on your server mount name the path to the newly created
collection should be similar to ``/local/demo/tutorial``.

--------------

Create the Notebook
-------------------

From the ``/local/demo`` view, where the ``tutorial`` file can be seen:

1. Create a Notebook |New Notebook| and rename it ``Tutorial Notebook``
   by double-clicking the title.

Once the Notebook has been renamed it will be saved. At this point the
Notebook can be shared in whatever form it currently is saved. Click on
the Notebook menu in the top and select ``Publish``. A new browser
window will open displaying the contents of the Notebook. At this point
nothing will be displayed as no cells have been added. The URL in the
browser can be shared so others may see the results of the Notebook
without modifying it.

--------------

Explore the Data
----------------

To get an idea of what the schema looks like and to become familiar with
the data a new Exploration cell will now be created.

1. Click on the Plus |Plus| icon and then click on the Explore |Explore|
   icon.
2. Click on the *Select a file* drop-down menu and locate the
   ``/local/demo/tutorial`` file: |Drop Down|.
3. Click the Play |Play| icon. Once clicked the exploration cell will
   appear as below:

.. figure:: /images/screenshots/cell-exploration.png
   :alt: Exploration Cell

Take a moment to view what the published Notebook looks like now. The
exploration cell is displayed but the drop-down file selector is not.
Again - publishing a Notebook allows others to view the results without
modifying anything (unless desired - which will be covered shortly)

The schema shows several fields such as ``city``, ``country``,
``event``, etc. and also has a numeric field ``year``. Now that the
schema has been viewed along with a sampling of data a SlamDown form
will be created to show some of the reporting capabilities of SlamData.

--------------

Create a Static Form
--------------------

1. Delete the exploration cell by clicking on the Trashcan |Trashcan
   icon| icon above the Explore cell.
2. Now create a new SlamDown |SlamDown| cell.
3. Enter the following text into the SlamDown form and then click the
   Play |Play Icon| icon.

::

    # Olympics Report ![Olympics](https://raw.githubusercontent.com/damonLL/slamdemofiles/master/olympic-logo.jpg)

    There are !``SELECT COUNT(DISTINCT(country)) FROM `/local/demo/tutorial` `` unique countries in this data set.

    There are !``SELECT COUNT(DISTINCT(year)) FROM `/local/demo/tutorial` `` Olympiads in this data set.

    There are !``SELECT COUNT(DISTINCT(sport)) FROM `/local/demo/tutorial` `` unique sports in this data set
    comprised of !``SELECT COUNT(DISTINCT(event)) FROM `/local/demo/tutorial` `` unique events.

-  See the SlamDown rendering similar to the following image:

.. figure:: /images/screenshots/olympics-slam1.png
   :alt: Basic SlamDown Report

Again - take a moment to review the published version of this Notebook.
And keep this important note in mind: *All of these queries and
reports are executing against a live database. When the data underneath
changes, this report will change the next time it is viewed.*

This section described how to create a static form and publish its
results. There are many other SlamDown features you can read about in
the `SlamDown Reference <slamdown-reference.html>`__.

The next section describes how to publish a dynamic form that allows
user interaction.

--------------

Create a Dynamic form
---------------------

This section describes how to create a dynamic form that allows users to
interact with the data and eventually results in a chart generated from
data the user has selected.

The existing Notebook can either be modified or a new Notebook can be
created. This tutorial will modify the existing Notebook but users can
choose either path.

1. Replace the Markdown code with the following code and click the Play
   |Play Icon| icon.

::

    # Olympics Report ![Olympics](https://raw.githubusercontent.com/damonLL/slamdemofiles/master/olympic-logo.jpg)

    Years in this dataset range from
    !``SELECT DISTINCT(year) FROM `/local/demo/tutorial` ORDER BY year ASC``
    to
    !``SELECT DISTINCT(year) FROM `/local/demo/tutorial` ORDER BY year DESC``

    ### Enter start year and end year for report

    startyear = #____

    endyear = #____

Note the pound or hashtag ``#`` symbol directly before the underscores
``_``. This tells SlamData that the input field is for numbers. In
SlamData input fields are String types by default. The data type is
important as the query listed below would not work with String types
against a numeric database field.

This results in the following rendered Markdown:

.. figure:: /images/screenshots/olympics-slam2.png
   :alt: Olympic Date Selectors

The rendered drop downs of ``startyear`` and ``endyear`` create
variables that allow users to select data that is then used as query
criteria in a query cell.

--------------

Query Based on a Form
---------------------

In this section the variables defined in the previous section will be
used to control the subsequent query.

1. Click on the gray Query |Query Icon| icon to the left of the SlamDown
   rendered cell. This is **important** as it allows the created
   variables to bind to the query cell.
2. Enter the following query into the Query cell and click the Play
   |Play Icon| icon.

::

    SELECT
      COUNT(*) as cnt,
      country,
      type
    FROM `/local/demo/tutorial`
    WHERE
      year >= :startyear AND
      year <= :endyear
    GROUP BY country, type
    ORDER BY cnt DESC

It is important to group the results in the query above so the chart we
create in the next section has valid data to work with.

--------------

Chart the Results
-----------------

This section will display the results in the table from the previous
section in chart form.

1. Next to the results cell, click the Visualize |Chart Icon| icon to
   create a new visualization cell.
2. Select the Bar |Bar Chart| chart icon.
3. Select or enter the values below in the configuration fields:

+--------------------+------------+
| Field              | Value      |
+====================+============+
| Category           | .country   |
+--------------------+------------+
| Measure            | .cnt       |
+--------------------+------------+
| Series             | .type      |
+--------------------+------------+
| Height             | 400        |
+--------------------+------------+
| Width              | 800        |
+--------------------+------------+
| Axis Label Angle   | 45         |
+--------------------+------------+
| Axis Font Size     | 12         |
+--------------------+------------+

This will result in the following chart:

.. figure:: /images/screenshots/olympics-chart.png
   :alt: Olympics Chart

   Olympics Chart

The items in the legend at the top (``Bronze``, ``Silver``, ``Gold``)
can be clicked to toggle viewing of that data series.

--------------

Put It All Together
-------------------

You've learned how to create static and dynamic forms, add variables to
queries and generate charts from user input. The only thing left is to
publish the Notebook (or re-open the published link) and view how
interactive the Notebook is. Because the SlamDown cell allows inputs
this Notebook allows controlled input from users to create the charts.

Beyond publishing the Notebook as a whole, each cell can be published or
embedded into another web application. Simply click the Embed |Embed
Icon| icon and copy the contents (JavaScript and/or URL) and embed them
directly into the HTML of the other web application. You can also view
our YouTube video which demonstrates this functionality here.

Note: > To allow users to interact with data and have cells update other
cells, the user must interact with the published Notebook, not the
individual cells. When individual cells are embedded in applications the
dynamic dependencies no longer work outside of the Notebook context.

--------------

.. |New Folder| image:: /images/icon-create-folder.png
.. |Rename Folder| image:: /images/screenshots/rename-folder.png
.. |Upload Icon| image:: /images/icon-upload.png
.. |Back Icon| image:: /images/icon-back.png
.. |New Notebook| image:: /images/icon-notebook.png
.. |Plus| image:: /images/icon-plus.png
.. |Explore| image:: /images/icon-explore.png
.. |Drop Down| image:: /images/screenshots/select-a-file.png
.. |Play| image:: /images/icon-play.png
.. |Trashcan icon| image:: /images/icon-gray-trashcan.png
.. |SlamDown| image:: /images/icon-slamdown.png
.. |Play Icon| image:: /images/icon-play.png
.. |Query Icon| image:: /images/icon-gray-query.png
.. |Chart Icon| image:: /images/icon-gray-chart.png
.. |Bar Chart| image:: /images/icon-gray-bar.png
.. |Embed Icon| image:: /images/icon-embed.png


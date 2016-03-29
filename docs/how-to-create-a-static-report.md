---
layout: support
title:  "How to Create a Static Report"
---

> ***Read This First!***
> We recently released SlamData 2.5 — it was a major release. We're in a tear to get all of our documentation updated but we have not finished. So, before you scroll down and start reading please check out the latest blog from our CTO, John De Goes — it outlines key changes that you'll want to follow: [SlamData 2.5 Released: A Bold New Step Into the World of Post-Relational Analytics](/releases/2016/02/19/slamdata-2-5-released-a-bold-new-step-into-the-world-of-post-relational-analytics.html)


This is a short tutorial on how to create a static (i.e., not interactive) report. The report will contain data, a chart, and text.

Be sure to start with the [Getting Started](./getting-started.html) page to download and install SlamData, as well as mount a MongoDB database.

For this tutorial you will be using a sample file that is provided by MongoDB and contains latitude, longitude, and population data for various cities in the United States. If you haven't already uploaded the file as part of the Getting Started instructions, please follow these steps to do so:

1. Option-click (Mac) or right-click (Windows) the following link and save it to your local drive: 
[Sample JSON file](http://media.mongodb.org/zips.json?_ga=1.138295545.8598417.1408291048).

2. Click on your database. 
![Database listing](/images/screenshots/getting-started-db-listed.png)

3. Click the upload button. 
![Upload button](/images/screenshots/getting-started-upload.png)

4. Navigate to your file and click **Open**. You should now see file loading. 
![Uploading file](/images/screenshots/getting-started-file-loading.png)

5. Once loaded, you will see a table with data. 
![File data](/images/screenshots/getting-started-file-data.png)



## Running a Query

Let's run a SlamSQL query to generate data for the report. The following query will sum all of the data for each state, providing a table with population numbers and  state abbreviations.

    SELECT state, SUM(pop) FROM "/SD/sampleslamdata/SampleJSON.json" GROUP BY state 

Replace `/SD/sampleslamdata/SampleJSON.json` with the path to the file you uploaded.

Follow these steps to run the SQL statement:

1. Click on the **query** icon on the left side. 
![Query icon](/images/screenshots/front-end-query-icon.png)

2. Towards the bottom of the page, type in the SlamSQL statement. Don't forget to replace the path with your own.
![Query statement](/images/screenshots/how-to-static-query-statement.png)

3. Click the "play" button to run the query. The results will be displayed below.  
![Query results](/images/screenshots/how-to-static-query-results.png)

The first column is the summed population data, and the second column is the state
abbreviation.


## Creating a chart

Now you can create a bar chart that displays the data. Follow these steps:

1. Click on the **visualization** icon on the left side. 
![Visualization icon](/images/screenshots/front-end-visualization-icon.png)

2. Choose the bar chart icon.
![Bar chart icon](/images/screenshots/how-to-static-bar-chart-icon.png)

3. The category and measure will automatically be chosen for you, since there are only two columns. Keep the height at 400 and change the width to 1200. 
![Set chart width](/images/screenshots/how-to-static-chart-width.png)

4. The chart with all of the states and their population numbers is then shown below.
![Set chart width](/images/screenshots/how-to-static-chart.png)



## Adding Markdown

In a static report, you can create a SQL query that is evaluated as part of the Markdown. To do this, start with an explanation point and a backtick, and then end with another
backtick. In other words, it should look like this:

    !`<SQL query>;`

where `<SQL query>` is your query.

For example, the following markdown embeds a SQL query that counts up the number of distinct values in the `state` column.

     The number of states in this database is !`SELECT COUNT(*) FROM (SELECT DISTINCT t.state FROM "/SD/sampleslamdata/SampleJSON.json" AS t) AS s`.

Again, replace `/SD/sampleslamdata/SampleJSON.json` with the path to the file you uploaded.

Follow these steps to add this Markdown to your report:

1. Scroll to the bottom of the page and click the plus icon to create a new stand-alone cell. ![Plus icon](/images/screenshots/front-end-plus-icon.png)

2. Click on the **Markdown** icon. 
![Markdown icon](/images/screenshots/front-end-markdown-icon.png)

3. Enter Markdown in the text below to create the form. For example, the following Markdown creates a text box whose value is set to the variable `state`, with a default value of "CA".
![Markdown example](/images/screenshots/how-to-static-markdown.png)

4. Click the play icon to generate the text with the embedded results.
![Run form](/images/screenshots/how-to-static-markdown-results.png)

Note that although the United States has only 50 states, the database includes the District of Columbia (DC) as a state, bringing the total to 51.



## Publishing a Notebook

Once you have created your report, you may want to share it with less technical users so that they can view the data. Publishing creates a public webpage that contains exploration, markdown, and visualization cells, but not query or search cells. Forms are displayed, so that uses can make changes to the variables and see the results.

To publish a notebook:

1. From the **Notebook** menu, choose **Publish**. 
    ![Publish notebook](/images/screenshots/front-end-publish-notebook.png)

2. A new web page will be loaded with the published notebook.

3. Now you can share the URL of the new page.
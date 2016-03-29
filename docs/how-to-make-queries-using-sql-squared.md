---
layout: support
title:  "How to Make Queries Using SQL²"
---

> ***Read This First!***
> We recently released SlamData 2.5 — it was a major release. We're in a tear to get all of our documentation updated but we have not finished. So, before you scroll down and start reading please check out the latest blog from our CTO, John De Goes — it outlines key changes that you'll want to follow: [SlamData 2.5 Released: A Bold New Step Into the World of Post-Relational Analytics](/releases/2016/02/19/slamdata-2-5-released-a-bold-new-step-into-the-world-of-post-relational-analytics.html)


This is a short tutorial on how to use SQL<sup>2</sup> to make queries. For more details on SQL<sup>2</sup>, read the [SQL<sup>2</sup> reference guide](/documentation/reference/#sql²reference).

For this tutorial you will be using a sample file that is provided by MongoDB and contains latitude, longitude, and population data for various cities in the United States. If you haven't already uploaded the file as part of the Getting Started instructions, please follow these steps to do so:

1. Option-click (Mac) or right-click (Windows) the following link and save it to your local drive: [Sample JSON file](http://media.mongodb.org/zips.json?_ga=1.138295545.8598417.1408291048).

2. Click on your database. </br>

![Database listing](/images/screenshots/getting-started-db-listed.png)

3. Click the upload button. </br>

![Upload button](/images/screenshots/getting-started-upload.png)

4. Navigate to your file and click **Open**. You should now see file loading. </br>

![Uploading file](/images/screenshots/getting-started-file-loading.png)

5. Once loaded, you will see a table with data. </br>

![File data](/images/screenshots/how-to-sql2-table.png)





## Basic Queries

Follow these steps to run a query:

1. Click on the **query** icon on the left side. ![Query icon](/images/screenshots/front-end-query-icon.png)

2. Towards the bottom of the page, type in the SQL query statement. </br>

![Query statement](/images/screenshots/how-to-sql2-type-query.png)

3. Click the "play" button to run the query. The results will be displayed below.  </br>

![Query results](/images/screenshots/how-to-sql2-all-query-results.png)


In the examples below, replace `/TestSlamData/testslamdata/SampleJSON.json` with the path to where you've uploaded the JSON file.

To simply see all of the data, use this query:

    SELECT * FROM "/TestSlamData/testslamdata/SampleJSON.json"

To see data from the state of Washington, modify the query to be this:

    SELECT * FROM "/TestSlamData/testslamdata/SampleJSON.json" WHERE state='WA'

To see only city name and population, modify the query to be this:

    SELECT city, pop FROM "/TestSlamData/testslamdata/SampleJSON.json" WHERE state='WA'

If you need to reference anything in a different part of the query you can
use aliases, like this:

    SELECT citydata.city, citydata.pop FROM "/TestSlamData/testslamdata/SampleJSON.json"
      AS citydata WHERE state='WA'





## String and Math Operations

Again, replace `/TestSlamData/testslamdata/SampleJSON.json` with the path to your own data.

You can use the `||` operator to concatenate both strings and arrays. In this query,
the city and state name are concatenated so that they appear in one column:

    SELECT city || ', ' || state, pop FROM "/TestSlamData/testslamdata/SampleJSON.json"

You can have math operations as part of your selection as well. For example, you
can modify the above query so that the population number represents the number
of thousands of people.

    SELECT city || ', ' || state, pop / 1000  FROM "/TestSlamData/testslamdata/SampleJSON.json"

Use the COUNT operator to count the number of rows. For example, this query
counts the number of distinct states (51, which is the number of U.S. states
plus the District of Columbia).

    SELECT COUNT(*) FROM (SELECT DISTINCT t.state
      FROM "/TestSlamData/testslamdata/SampleJSON.json" AS t) AS s






## Grouping Data

Again, replace `/TestSlamData/testslamdata/SampleJSON.json` with the path to your own data.

Use the COUNT operator to count data. For example, this query
counts the number of distinct states (51, which is the number of U.S. states
plus the District of Columbia).

    SELECT COUNT(*) FROM
     (SELECT DISTINCT t.state FROM "/TestSlamData/testslamdata/SampleJSON.json" AS t) AS s

Use the SUM operator to add up data. For example, this query returns the total
population, summing up the population of each city.

    SELECT SUM(pop) FROM "/TestSlamData/testslamdata/SampleJSON.json"

You can use the GROUP BY operator to group results. For example, this query returns
a table of the total population of each state.

    SELECT SUM(pop) FROM "/TestSlamData/testslamdata/SampleJSON.json" GROUP BY state





## String Matching

For the rest of the queries, we will use a different database. This database contains information on commits made to the slamengine project. Follow these steps to upload this database:

1. In a browser, navigate to this URL: <https://github.com/damonLL/tutorial_files>.

2. Click on the **Download ZIP** button.

![Download zip](/images/screenshots/how-to-sql2-download.png)

3. Open the zip file and extract the files.

4. Click on your database. </br>

![Database listing](/images/screenshots/getting-started-db-listed.png)

5. Click the upload button. </br>

![Upload button](/images/screenshots/getting-started-upload.png)

6. Navigate to the **slamengine_commits** file and click **Open**.

7. Once loaded, you will see a table with data. </br>

![File data](/images/screenshots/how-to-sql2-commits-table.png)

If you scroll to the right, you can see a `date` column with timestamps.

![Commits table](/images/screenshots/how-to-sql2-timestamps.png)

Click on the query icon to create a query. ![Query icon](/images/screenshots/front-end-query-icon.png)

In the examples below, replace the `/TestSlamData/testslamdata/slamengine_commits` with the path to your own file.






## LIKE

Use the [LIKE](http://www.w3schools.com/sql/sql_like.asp) operator to match strings using patterns. Use the percentage sign (`%`) to indicate a "wildcard", which means any text. Try the following query, which will
return any commit message that starts with the text "Merge".

    SELECT commit.message FROM "/TestSlamData/testslamdata/slamengine_commits"
      WHERE commit.message LIKE 'Merge%'

**Note:** Currently there is a bug where only one-line messages are returned.

You can also put the percentage sign in other positions in the string. For example, try this query, which returns any message that contains the text "branch" and later has the text "ready":

    SELECT commit.message FROM "/TestSlamData/testslamdata/slamengine_commits"
      WHERE commit.message LIKE '%branch%ready%'





## Regular Expressions

[Regular expressions](https://en.wikipedia.org/wiki/Regular_expression) allow you to do very sophisticated pattern matching. Use the tilde (`~`) character to indicate that the literal is a regular expression. For example, try this query, which returns any commit message that starts with the text "Merge".

    SELECT commit.message FROM "/TestSlamData/testslamdata/slamengine_commits"
      WHERE commit.message ~ 'Merge.*'






## Date and Time Data

You can create queries that make use of date and time data. Again, replace the `/TestSlamData/testslamdata/slamengine_commits` with the path to your own file. Try this following query, which returns all of the dates that are later than January 17, 2015.

    SELECT commit.committer."date" FROM "/TestSlamData/testslamdata/slamengine_commits"
      WHERE commit.committer."date" > DATE '2015-01-17'

To add a time, making it a full timestamp, try this query:

    SELECT commit.committer."date" FROM "/TestSlamData/testslamdata/slamengine_commits" WHERE commit.committer."date" > TIMESTAMP '2015-01-17T08:00:00Z'

Now, try something more complex, such as counting the number of commits for each day of the week by using DATE_PART and the argument `'dow'`, which stands for "day of week".

    SELECT DATE_PART('dow', commit.committer."date") AS day, COUNT(*) AS count
      FROM "/TestSlamData/testslamdata/slamengine_commits"
      GROUP BY DATE_PART('dow', commit.committer."date")

---
layout: support
title:  "How to Make Reports Using SlamDown"
---

> ***Read This First!***
> We recently released SlamData 2.5 — it was a major release. We're in a tear to get all of our documentation updated but we have not finished. So, before you scroll down and start reading please check out the latest blog from our CTO, John De Goes — it outlines key changes that you'll want to follow: [SlamData 2.5 Released: A Bold New Step Into the World of Post-Relational Analytics](/releases/2016/02/19/slamdata-2-5-released-a-bold-new-step-into-the-world-of-post-relational-analytics.html)


This is a short tutorial on how to use SlamDown, the SlamData Markdown language, to make reports. For more details on SlamDown, read the  [SlamDown reference guide](slamdown-reference.html). You may also want to go through these tutorials:

* [How to Create a Static Report](how-to-static-report.html)
* [How to Create an Interactive Report](how-to-interactive-report.html)

For this tutorial you will be downloading a sample file with information on commits made to the slamengine project. If you haven't done it already, follow these steps to  upload this database: 2015-10-23-how-to-interactive-report

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





## Basic Markdown

Let's start with some basic Markdown. First, you'll need to follow these steps:

1. Scroll to the bottom of the page and click the plus icon to create a new stand-alone cell. ![Plus icon](/images/screenshots/front-end-plus-icon.png)

2. Click on the **Markdown** icon. </br>

![Markdown icon](/images/screenshots/front-end-markdown-icon.png)

Now, enter the following Markdown. The `#` sign creates a top level header, 
`[SlamData](http://slamdata.com)` creates a link, and `**` creates strong
emphasis.
    
    # Report on SlamEngine Commits

    This is a report on [SlamData](http://slamdata.com) **SlamEngine** commits.

Click the play icon to render the Markdown.

![Run markdown](/images/screenshots/how-to-slamdown-run.png)


* * *


## Images

Now you'll add the SlamDown logo as an image. Add this line at the very top, followed by a blank line:

    ![SlamData Logo](https://media.licdn.com/media/p/6/005/088/002/039b9f8.png)

Click the play icon to render the Markdown.

![With image](/images/screenshots/how-to-slamdown-image.png)





## Evaluated SQL Queries

Next you'll add a statement that evaluates a SQL query and puts the result in the Markdown. Evaluated queries take the form:

    !`<SQL query>`

where `<SQL query>` is your query.

Add a blank line to your Markdown, and then add the following line. This will simply count the number of records in the database and add it to the Markdown. Replace `/TestSlamData/testslamdata/slamengine_commits` with the path to the file you uploaded.

    There are a total of !`There are a total of !`SELECT COUNT(*) FROM "/TestSlamData/testslamdata/slamengine_commits"` commits.` commits.

The rendered Markdown should look like this:

![Evaluated SQL](/images/screenshots/how-to-slamdown-eval-sql.png)






## Text box

Now, add a second-level heading using two hash marks (`##`) as a place to search for the number of commits by a particular user. For this, you'll add a form. 
Add a blank line to the Markdown followed by:

    ## Search on Committer Email

    committer = ____

The underscores create a textbox so that a user can enter a value. 

To the left of the Markdown, click on the query icon to create a query based on this Markdown.

![Query icon](/images/screenshots/how-to-slamdown-query-icon.png)

Now, enter the following query that uses the variable, comparing its value to the value of the email of the committer. Replace `/TestSlamData/testslamdata` with the path to your own file.

    SELECT * FROM "/TestSlamData/testslamdata/slamengine_commits" WHERE commit.committer.email=:committer

Choose an email address from the **email** column of the **committer** section in the **commit** section and type it into the text box. Click the Play button to run the query.

![Run query](/images/screenshots/how-to-slamdown-run-query.png)

You will now only see entries from that email address.





## Dropdown

Even better than a text box would be a dropdown menu that showed all possible emails. You can evaluate a SQL query to do this. Replace 

    committer = ____

with

    committer = {!`SELECT DISTINCT commit.committer.email 
    FROM "/TestSlamData/testslamdata/slamengine_commits" 
    ORDER BY commit.committer.email`}

Click the play button to render the Markdown. Now you have a dropdown list populated with all potential email addresses. Select an email address in the dropdown list, and you will  see that the query updates automatically.

![Dropdown with query](/images/screenshots/how-to-slamdown-dropdown.png)

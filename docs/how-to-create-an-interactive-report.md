---
layout: support
title:  "How to Create an Interactive Report"
---

> ***Read This First!***
> We recently released SlamData 2.5 — it was a major release. We're in a tear to get all of our documentation updated but we have not finished. So, before you scroll down and start reading please check out the latest blog from our CTO, John De Goes — it outlines key changes that you'll want to follow: [SlamData 2.5 Released: A Bold New Step Into the World of Post-Relational Analytics](/releases/2016/02/19/slamdata-2-5-released-a-bold-new-step-into-the-world-of-post-relational-analytics.html)


This is a short tutorial on how to create an interactive report with user interface elements that non-technical users can use to change the data that is displayed.

Be sure to start with the [Getting Started](./getting-started.html) page to download and install SlamData, as well as mount a MongoDB database.

For this tutorial you will be using a sample file that is provided by MongoDB and contains latitude, longitude, and population data for various cities in the United States. If you haven't already uploaded the file as part of the Getting Started instructions, please follow these steps to do so:

1. Option-click (Mac) or right-click (Windows) the following link and save it to your local drive: 

[Sample JSON file](http://media.mongodb.org/zips.json?_ga=1.138295545.8598417.1408291048).

2. Click on your database. </br>

![Database listing](/images/screenshots/getting-started-db-listed.png)

3. Click the upload button. </br>

![Upload button](/images/screenshots/getting-started-upload.png)

4. Navigate to your file and click **Open**. You should now see file loading. </br>

![Uploading file](/images/screenshots/getting-started-file-loading.png)

5. Once loaded, you will see a table with data. </br>

![File data](/images/screenshots/getting-started-file-data.png)



## Creating a Textbox

First use Markdown to create a text box to specify what state to display data for.  Follow these steps:

1. Scroll to the bottom of the page and click the plus icon to create a new stand-alone cell. ![Plus icon](/images/screenshots/front-end-plus-icon.png)

2. Click on the **Markdown** icon. </br>

![Markdown icon](/images/screenshots/front-end-markdown-icon.png)

3. Enter the following Markdown in the text box below to create the form. This will create a text box whose value is set to the variable `state`, with a default value of "MA".</br>
    `state = ____ (MA)`</br>

![Markdown example](/images/screenshots/how-to-interact-markdown.png)

4. Click the play icon to generate the form.</br>

![Run form](/images/screenshots/how-to-interact-form.png)




## Running A Query

Next, create a query that shows the population of all cities for a user-specified state.

1. Click on the **query** button to the left of the form. </br>
![Query button](/images/screenshots/how-to-interact-query-btn.png)

2. Type in the following query, using the variable name "state: prefixed with a colon. Replace the path with the path to your own data.</br>
    `SELECT city, SUM(pop) FROM "/SD/sampleslamdata/SampleJSON.json" WHERE state=:State GROUP BY city ORDER BY city`</br>

3. You will now see population numbers for the cities of Massachussets (MA), starting with ABINGTON. Return to the textbox and replace MA with NM. Within a few seconds, the table will update, and you will  see the population numbers for the cities of New Mexico (NM), starting with ABIQUIU.</br>
![New Mexico table](/images/screenshots/how-to-interact-nm.png)




## Creating A Chart

Next, you'll create a pie chart to visualize the population data for all of the cities. 

1. Click on the **visualization** icon on the left side. 

![Visualization icon](/images/screenshots/front-end-visualization-icon.png)

2. Leave it at the default of pie chart. You can see a pie chart attempting to visualize all of New Mexico's cities. It's too many cities for a good pie chart, but if you hover over the biggest slice, you can see that it belongs to Albuquerque. </br>

![New Mexico pie chart](/images/screenshots/how-to-interact-nm-pie.png)

3. Now change the state to the abbreviation for a smaller state, such as DE (Delaware). The pie chart updates automatically.</br>

![Delaware pie chart](/images/screenshots/how-to-interact-de-pie.png)



### Populating A Dropdown

You can even evaluate data from the database and use it to populate user elements, like dropdown lists. To evaluate a SQL query within Markdown, use this format:

    !`<SQL query>;`

where `<SQL query>` is your query.

Let's replace the text box for the state with a dropdown box.

1. Replace the markdown so that it populates a markdown with all of the unique values of the state column: </br>
    ``State = {!`SELECT DISTINCT state FROM "/SD/sampleslamdata/SampleJSON.json" ORDER BY state`}``

2. Click the play icon to generate the form.</br>

![Run form](/images/screenshots/how-to-interact-dropdown1.png)

3. The form is now populated with all of the states, in alphabetical order. If you change the value of the state, then the table and chart will update automatically.</br>

![Run form](/images/screenshots/how-to-interact-dropdown2.png)



## Publishing the Notebook

Once you have created your report, you may want to share it with less technical users so that they can view the data. Publishing creates a public webpage that contains exploration, markdown, and visualization cells, but not  query or search cells. Forms are displayed, so that uses can make changes to the variables and see the results.

To publish a notebook:

1. From the **Notebook** menu, choose **Publish**. </br>

![Publish notebook](/images/screenshots/front-end-publish-notebook.png)

2. A new web page will be loaded with the published notebook.

3. Now you can share the URL of the new page. Note that you can change the value in the dropdown box and see the results immediately.
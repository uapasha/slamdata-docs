---
layout: support
title:  "How To Search Data"
---

> ***Read This First!***
> We recently released SlamData 2.5 — it was a major release. We're in a tear to get all of our documentation updated but we have not finished. So, before you scroll down and start reading please check out the latest blog from our CTO, John De Goes — it outlines key changes that you'll want to follow: [SlamData 2.5 Released: A Bold New Step Into the World of Post-Relational Analytics](/releases/2016/02/19/slamdata-2-5-released-a-bold-new-step-into-the-world-of-post-relational-analytics.html)


This is a short tutorial on how to use the search capability to explore data. Be sure to start with the [Getting Started](./getting-started.html) page to download and install SlamData, as well as mount a MongoDB database.

For this tutorial you will be using a sample file that is provided by MongoDB and contains latitude, longitude, and population data for various cities in the United States. If you haven't already uploaded the file as part of the Getting Started instructions, please follow these steps to do so:

1. Option-click (Mac) or right-click (Windows) the following link and save it to your local drive: [Sample JSON file](http://media.mongodb.org/zips.json?_ga=1.138295545.8598417.1408291048).

2. Click on your database. 
    ![Database listing](/images/screenshots/getting-started-db-listed.png)

3. Click the upload button. 
    ![Upload button](/images/screenshots/getting-started-upload.png)

4. Navigate to your file and click **Open**. You should now see file loading. 
    ![Uploading file](/images/screenshots/getting-started-file-loading.png)

5. Once loaded, you will see a table with data. 
    ![File data](/images/screenshots/getting-started-file-data.png)


* * *


## Searching for a City
Let's say you wanted to use this data to find the data for the city of Springfield. You don't need to create a SQL statement to do this. You can use the Google-style search capability. Follow these steps:

1. Click on the **search** icon on the left side of your data. 
    ![Search icon](/images/screenshots/how-to-search-icon1.png)

2. Towards the bottom of the page, type in "springfield". Then click the search button (magnifying glass) to the right.
    ![Search statement](/images/screenshots/how-to-search-term1.png)

3. The search results will be displayed below.  Note that there are several cities named Springfield, and data for all of them is displayed. 
    ![Search results](/images/screenshots/how-to-search-results1.png)

Note that the search term is not case-sensitive.


* * *


## Searching within Results
Perhaps there is one specific Springfield you want to know about. Let's say that you want the data for Springfield, Illinois, which has a state abbreviation of IL. You can  simply perform a search on the previous search results. Follow these steps:

1. Click on the **search** icon on the left side of your results.  
    ![Search icon](/images/screenshots/how-to-search-icon2.png)

2. Towards the bottom of the page, type in "IL". Then click the search button (magnifying glass) to the right.
    ![Search statement](/images/screenshots/how-to-search-term2.png)

3. The search results will be displayed below.  Now, you just see the data for
Springfield, Illionois. 
    ![Search results](/images/screenshots/how-to-search-results2.png)

To do the next piece, delete the cells with the search results by clicking on the "delete cell" button (the trash can).



* * *


## Searching with Field Names

You can specify field names by ending them with a colon. For example, to specify the city of  Springfield and the state of Illinois, you can use this search term:

    city:springfield state:il

You can also use field names with comparisons. For example, to return all data where the city contains "springfield" and the population is greater than 10,000, use this search term:

    city:springfield pop:>10000


* * *


## Suppressing Search Results

By using the minus sign, you can suppress search results that contain a search term. For example, to search on Redding but not include anything with CA (thus eliminating Redding, California from the search),  you can use this search term:

    Redding -CA

This will return data with Redding but not with CA.

You can also specify the data fields. For example, you might think that the search term `Portland -OR` will return data for Portland cities other than Portland, Oregon (such as Portland, Maine). However, it will return nothing because the word "Portland" contains the term "or". You can look specifically for cities containing Portland while suppressing data where the state contains "OR" with this search term:

    city:Portland -state:OR


* * *


## More Search Functionality

More search functionality is described at the end of the SlamData 
[Cheat Sheet](http://slamdata.com/support/cheatsheet.pdf), including:

* Ranges
* SQL LIKE statements
* Glob patterns
* Metadata attributes
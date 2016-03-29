---
layout: support
title: SlamData Users Manual
---

> ***Read This First!***
> We recently released SlamData 2.5 — it was a major release. We're in a tear to get all of our documentation updated but we have not finished. So, before you scroll down and start reading please check out the latest blog from our CTO, John De Goes — it outlines key changes that you'll want to follow: [SlamData 2.5 Released: A Bold New Step Into the World of Post-Relational Analytics](/releases/2016/02/19/slamdata-2-5-released-a-bold-new-step-into-the-world-of-post-relational-analytics.html)


## Opening the Web App

If you've installed the SlamData application on your server, then opening the application will automatically load a web page with the web app at the following location:

    http://localhost:20223/slamdata/index.html


However, you don't need to be on the server to access the web app. If you have access to  the server's address, you can access the web app at:

    http://<server address>:20223/slamdata/index.html

where \<server address\> is the server's domain.



## Key Concepts

It is useful to understand the following key concepts when using the web app.


## File System

The file system displays the databases and their data. To view the file system from the top,  click on the file icon in the upper left corner.

![File system](/images/screenshots/front-end-file-system.png)

There is a mapping between MongoDB concepts and SlamData concepts:

* A MongoDB cluster appears in the file system as a mount.

![Mount icon](/images/screenshots/front-end-mount-icon.png)

* A MongoDB database appears in the file system as a folder. 

![Folder icon](/images/screenshots/front-end-folder-icon.png)

* A MongoDB collection appears in the file system as a file. 

![File icon](/images/screenshots/front-end-file-icon.png)





## Notebooks

Notebooks capture data workflows in a visual fashion that allows you to query data,  transform it, and visualize it in the form of charts and reports. Reports are created with
[Markdown](http://daringfireball.net/projects/markdown/), which is a simple markup language, and SlamData uses that Markdown to generate polished web pages with interactive reports that a non-technical user can understand and use.

Each stage in the workflow is called a *cell*. Cells can either use data from previous cells or else work as standalone cells, starting with the original data.


## Clusters and Databases

SlamData uses MongoDB databases. This section describes how to mount and unmount a  database cluster and then to view the databases it contains.



## Mounting a Database Cluster

Follow these instructions to mount a database cluster.

1. Click the mount icon.

![Mount button](/images/screenshots/getting-started-mount.png)

2. Fill in details for the mount. In the case of MongoLab, you can add a name and then paste in the URL from the MongoLab website, and the dialog will fill in the other fields. It's acceptable to leave the Path and Properties blank. Click the **Mount* button.

![Database dialog](/images/screenshots/getting-started-db-dialog.png)

3. The file system will be displayed. You should see your mount listed. 

![Directory listing](/images/screenshots/getting-started-db-listed.png)

**Note:** You cannot create a mount inside of another mount. 


### Unmounting a Database Cluster  
 
 
Follow these instructions to unmount a database cluster.

1. Move your cursor over the mount to remove.
2. Click on the trash icon.

![Unmount icon](/images/screenshots/front-end-unmount.png)

**Note:** You may have to refresh the page to see that the mount is no longer listed.



## Viewing the Databases

To view the databases in a cluster, simply click on the mount. The next view will  show you all of the databases in that mount as folders. The example below has one database,  named "testslamdata".

![Folders](/images/screenshots/front-end-folders.png)


## Files

Files are collections of data. They can be uploaded as .csv (comma-separated values) or JSON (JavaScript Object Notation) files. Once they are part of the file system, they can be renamed, moved, downloaded, deleted, and shared, as well as explored using notebooks.


### Uploading a File

To upload a file, follow these steps:

1. At the top of the page, click the **upload file** icon

![Upload file](/images/screenshots/front-end-upload-icond.png). 

2. Choose a file from your file system.

3. Wait a few moments for the file to be uploaded and analyzed.

4. A new, untitled notebook will be created that displays the data.

To rename, move, or delete the file, use the file system. You can get to the file system at any time by clicking on the file system icon.  
![File system icon](/images/screenshots/front-end-file-system.png)


### Renaming a File

To rename a file, follow these steps:

1. Move your cursor over the file to rename and click on the **move/rename** icon 

![Upload icon](/images/screenshots/front-end-upload-icon.png)

2. Edit the top text box to choose another name.

![Top text box](/images/screenshots/front-end-rename.png)

3. Click **Rename**.



### Moving a File

To move a file to another folder, follow these steps:

1. Move your cursor over the file to rename and click on the **move/rename** icon 

![Move/Rename](/images/screenshots/front-end-move-rename.png). 

2. Click the dropdown arrow on the bottom text box and choose a location to move to. (Alternatively, you can type the location in the text box.)

![Move dropdown](/images/screenshots/front-end-move-dropdown.png)

3. Click **Rename**.


### Downloading a File

To download a file, follow these steps:

1. Move your cursor over the file to rename and click on the **download** icon 
![Download](/images/screenshots/front-end-download-icon.png). 
![Download dialog](/images/screenshots/front-end-download-dialog.png)

2. Choose your source file and the name of the file you want saved. 

3. Check the **Compress** checkbox if you want the file downloaded as a compressed zip file.

4. Choose CSV or JSON. If CSV, choose your delimiters and quotes. If JSON, choose how to handle multiple values and precision.

5. Click **Download**.


### Deleting and Undeleting a File

To delete a file, move your cursor over the file to rename and click on the **delete** icon 
![Delete icon](/images/screenshots/front-end-delete-icon.png). The file will be deleted.

Deleted files are moved to the trash folder. By default, the trash folder is not visible. To see it, click on the **show hidden files** icon. 
![Show hidden icon](/images/screenshots/front-end-show-hidden.png) The  **.trash** folder will be visible.

To move a deleted file from the trash, follow the instructions for [moving a file](#moving-file), and move it from the trash to where you want it to go.

To permamently delete a file, delete it from the **.trash** folder.


### Sharing a File

Files can be shared by providing other users with a URL. To do this, follow these steps:

1. Move your cursor over the file to rename and click on the **share** icon 

![Share icon](/images/screenshots/front-end-share-icon.png). 

2. A dialog will appear with the URL that can be shared. 

![Share dialog](/images/screenshots/front-end-share-dialog.png)

3. Click **Copy**. Now the URL has been copied to your clipboard to use however you like.

### Exploring a File to a New Notebook

To explore a file using a new [notebook](#notebooks), simply click on its name,  and it will create a new, untitled notebook that you can use. The notebook will not be saved unless it is modified.

## Notebooks

Notebooks are the main way to interact with data when using the SlamData web application. Notebooks give you the ability to:

* View and explore data
* Run queries on data
* Search data
* Visualize data as charts
* Create interactive reports

A notebook acts like a workflow, allowing you to obtain the data you want through searches and queries, and then visualize that data in the form of charts and reports. 


### Cells
Each notebook contains one or more "cells", which correspond to the stages in the workflow. Clicking on a query, search, or visualization icon in a cell will create a new cell that flows from the first one cell. For example, if you create a cell using a query, and then use the result to create a new cell with a chart, then the chart will use data from the query results.

You can also create a stand-alone cell by clicking the plus icon at the bottom of the page.
![Plus icon](/images/screenshots/front-end-plus-icon.png)

Stand-alone cells use the original data., and can be used for querying,
markdown, exploring, or searching.

![Add standalone cell](/images/screenshots/front-end-standalone-cells.png)

Here are some actions you can do to all cells:

* Delete a cell using the **Delete** icon: 
![Delete cell icon](/images/screenshots/front-end-delete-cell.png)

* Refresh the data in a cell using the **Refresh** icon: 
![Refresh cell icon](/images/screenshots/front-end-refresh-icon.png)

* Display messages from the creation of the cell using the **Display messages** icon: 
![Display messages icon](/images/screenshots/front-end-display-messages.png)

* Embed the cell in any web page using the **Embed** icon.
![Embed icon](/images/screenshots/front-end-embed-icon.png)

**Note:** When you click the **Embed** icon, you will see the HTML code to embed the chart
into a web page. Click the **Copy** button to copy it into the clipboard, and then
paste the embed code into your web page.

![Embed code](/images/screenshots/front-end-embed-code.png)


### Renaming a Notebook

To rename a notebook, simply click on the title at the top and type in a new name.



### Running a Query

Queries use the SlamSQL syntax. See the  [cheat sheet](http://slamdata.com/support/cheatsheet.pdf) for information about the  syntax.

To run a query, follow these steps:

1. Click on the **query** icon on the left side. 
![Query icon](/images/screenshots/front-end-query-icon.png)

2. Towards the bottom of the page, type in a SlamSQL statement. 
![Query statement](/images/screenshots/front-end-query-statement.png)

3. Click the "play" button to run the query. The results will be displayed below.  
![Query results](/images/screenshots/front-end-query-results.png)


### Searching

Searches can be performed using a Google-style syntax. See the 
[cheat sheet](http://slamdata.com/support/cheatsheet.pdf) for information about the 
how to format searches.

To perform a search, follow these steps:

1. Click on the **search** icon on the left side. 
![Search icon](/images/screenshots/front-end-search-icon.png)

2. Towards the bottom of the page, type in a search statement. Then click the **search** button to the right.
![Search statement](/images/screenshots/front-end-search.png)

3. The search results will be displayed below.  
![Search results](/images/screenshots/front-end-search-results.png)



### Visualization

To visualize data as a chart, follow these steps:

1. Go to the results from either a query or search.
2. Click on the **visualization** icon on the left side. 
![Visualization icon](/images/screenshots/front-end-visualization-icon.png)

3. To the left, choose either pie chart, line chart, or bar chart.  
![Chart types](/images/screenshots/front-end-chart-types.png)

4. To the right, select which data you want to use for the chart. 
![Chart dialog](/images/screenshots/front-end-chart-dialog.png)
    * In the **Category** text box, choose the data to use as your category.
    * In the **Measure** text box, choose the data to use as your value.
    * If the data set has more dimensions that are being charted, then use the button next to the **Measure** text box to select how the duplicates will be reduced, such as sum, min, max, or average.
    * Optionally, choose a series axis source, if there are multiple series in your chart.
    * Choose the height and width of your chart.

5. You will see the chart displayed below.
![Chart results](/images/screenshots/front-end-chart-results.png)



### Forms

You can make the notebooks interactive to end users by using  [Markdown](http://daringfireball.net/projects/markdown/) to create forms. Markdown is a simple markup language, and SlamData uses a Markdown "flavor"  called SlamDown, which extends it with interactive form elements.  The syntax for SlamDown can be found in the [cheat sheet](http://slamdata.com/support/cheatsheet.pdf).

To add a form:

1. Scroll to the bottom of the page and click the plus icon to create a new stand-alone cell. ![Plus icon](/images/screenshots/front-end-plus-icon.png)

2. Click on the **Markdown** icon. 
![Markdown icon](/images/screenshots/front-end-markdown-icon.png)

3. Enter Markdown in the text below to create the form. For example, the following Markdown creates a text box whose value is set to the variable `state`, with a default value of "CA".
![Markdown example](/images/screenshots/front-end-form-markdown.png)

4. Click the play icon to generate the form.
![Run form](/images/screenshots/front-end-form-created.png)


Once you have form elements, you can use the variables from the form in a query, using these steps:

1. Click on the **query** button to the left of the form. 
![Form query](/images/screenshots/front-end-form-query.png)

2. Type in a query, using the variable name prefixed with a colon. In the example below, the `state` variable is used as `:state`. 
![Form query with variables](/images/screenshots/front-end-form-variable.png)

3. Click on the play button to run the query. The results will be displayed below.
![Form query results](/images/screenshots/front-end-form-results.png)

4. You can now visualize the data and create a chart, as in the [Visualization](#visualization) section, and when you change the values in the form, the chart will automatically update.




### Reports

The reports allow you to evaluate queries and place the results inside the text of the report. To create a report:

1. Scroll to the bottom of the page and click the plus icon to create a new stand-alone cell. 
![Plus icon](/images/screenshots/front-end-plus-icon.png)

2. Click on the **Markdown** icon. 
![Markdown icon](/images/screenshots/front-end-markdown-icon.png)

3. Enter Markdown in the text below to create the report.
![Markdown example](/images/screenshots/front-end-markdown-example.png)

4. Click the play icon to generate the report.
![Run report](/images/screenshots/front-end-run-report.png)




### Publishing a Notebook

Once you have created your notebook, you may want to share it with less technical users so that they can view the data and interact with it. Publishing creates a public webpage that contains exploration, markdown, and visualization cells, but not  query or search cells. Forms are displayed, so that uses can make changes to the variables and see the results.

To publish a notebook:

1. From the **Notebook** menu, choose **Publish**. 
![Publish notebook](/images/screenshots/front-end-publish-notebook.png)

2. A new web page will be loaded with the published notebook.

3. Now you can share the URL of the new page.




### Troubleshooting Notebooks

If you see an error, you can click on the eye icon to get more information. For example, in the screenshot below, a variable is used that is not a valid variable. Click on the eye icon to see more detail.

![Error](/images/screenshots/front-end-error1.png)

Error message will then be displayed.

![Error](/images/screenshots/front-end-error2.png)
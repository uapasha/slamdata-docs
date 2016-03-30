


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
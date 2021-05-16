# UFOs

<p align="center">
  <img src="static/images/UFO_Homepage.png" width="1000">
</p>


## Table of Contents
* [Overview](https://github.com/rkaysen63/UFOs/blob/master/README.md#overview)
* [Resources](https://github.com/rkaysen63/UFOs/blob/master/README.md#resources)
* [Results](https://github.com/rkaysen63/UFOs/blob/master/README.md#results)
* [Summary](https://github.com/rkaysen63/UFOs/blob/master/README.md#summary)

## Overview:
This data visualization project is a webpage that will allow the user to search for UFO sightings using one or more filters.  The filters include date of the sighting ("Date"), location of the sighting ("City", "State", and/or "Country") and the observed shape of the UFO ("Shape").  The dataset, data.js, includes UFO sightings data that include datetime, city, state, country, shape, durationMinutes, and comments held in a JavaScript object.  Users are encouraged to look through the data and come to their own conclusions about the existence of alien life.

## Resources:

* UFO Sightings Data File: data.js
* Jumbotron Background Image: nasa.jpg
* Tools: 
  * JavaScript
  * HTML
  * CSS
  * Bootstrap 4 library  
* Lesson Plan: UTA-VIRT-DATA-PT-02-2021-U-B-TTH, Module 11 Challenge

## Results:

* Code files used to create webpage
* style.css
* This file provides additional Bootstrap CSS styling.
* index.html
* This file creates the basic webpage.
* data.js
* This file is the data file to build the Table of UFO sightings.  It contains the variable "data" that is an array of key: value pairs
* app.js
* This file contains functions that can iterate through the rows of filtered data to create populate the data Table with values returned as a result of filtering.
* 
data.JS:  
static/css/style.css - additional Bootstrap CSS styling
index.html
includes links to Bootstrap's content delivery network (CDN)
app.js loops filters through functions


<p align="center">
  <img src="static/images/filter_placeholders.png" width="1000"><br/><br/>Filter Search  
</p>

The "Filter Search" allows the user to use one or more of the filters at a time.  The search will become more refined by the number of filters selected.  The "Filter Search" boxes contain placeholders when the webpage is opened that provide an example of the text to be entered into the user input box. In addition, the full table of data is displayed to the right of the "Filter Search" box so that the user may scroll through all of the data before refining a search.  The HTML code below shows the unordered list that holds the "Filter Search" user boxes. Under each list element is a label element `<label for="city">Enter City</label>` and an input element `<input type="text" placeholder="pratt" id="city" class="form-control" />`.  The JavaScript code will look for the `id` attribute within the input element and `class="form-control` identifies this element as a user-input form.  
          
        <!-- Create a set of list tags within an unordered list tag -->
        <ul class="list-group bg-dark">
            <li class="list-group-item bg-dark">
                <!-- label for each search parameter -->
                <label for="date">Enter Date</label>
                <!-- input element that provides the input box -->
                <input type="text" placeholder="1/10/2020" id="datetime" class="form-control"/>     
            </li>
            <li class="list-group-item bg-dark">
                <label for="city">Enter City</label>
                <input type="text" placeholder="pratt" id="city" class="form-control" /> 
            </li>
            <li class="list-group-item bg-dark">
                <label for="state">Enter State</label>
                <input type="text" placeholder="ks" id="state" class="form-control" />  
            </li>
            <li class="list-group-item bg-dark">
                <label for="country">Enter County</label>
                <input type="text" placeholder="us" id="country" class="form-control" />  
            </li>
            <li class="list-group-item bg-dark">
                <label for="shape">Enter Shape</label>
                <input type="text" placeholder="circle" id="shape" class="form-control" />  
            </li>
        </ul>


* Enter text in the same format as shown by the placeholder example, i.e., date format m/dd/yyyy, lower case letters, two letter state code, etc.  For example, type in "fireball" in the "Shape" user input box, followed by "ca" in the "State" user input box.

* The table reacts immediately.  First, after "fireball" is typed and entered in the "Shape" user box, all of the instances where "fireball" is observed is displayed. Then after "ca" is typed and entered in the "State" user box, only those instances where the "fireball" is observed in "ca" are displayed.

<p align="left">
  <img src="static/images/filter_fireball.png" width="800"><br/><br/>Filter Search for "fireball"
</p>

<p align="left">
  <img src="static/images/two_filters.png" width="800"><br/><br/>Filter Search for "fireball" and "ca"
</p>

   * The table reacts immediately upon the entry of the first filter because the event listener is looking for change.  

          // Event listener - listens for changes to each filter
          d3.selectAll("input").on("change", updateFilters);


   * After "fireball" is entered either by pressing "enter" or by typing in the second filter box, the event listener notices the change and calls the `updateFilters()` function.  The `updateFilters()` function updates the filters after receiving input and initializes an array to hold the id and value of the element(s) that changed.

          function updateFilters() {

              // Initialize an array to store the value and id of the changed element.
              let changedElement = d3.select(this);

              // Save the value that was changed as a variable.
              let elementValue = changedElement.property("value");
              console.log(elementValue);

              // Save the id of the filter that was changed as a variable.
              let filterId = changedElement.attr("id");
              console.log(filterId);

              // If a filter value was entered then add that filterId and value to the filters list. 
              // Otherwise, clear that filter from the filters object.
              if (elementValue) {
                filters[filterId] = elementValue;
              }
              else {
                delete filters[filterID];
              }
              console.log(filters);
              // Call function to apply all filters and rebuild the table
              filterTable();
            }
   

   * The `updateFilters()` function calls the `filterTable()` function to filter the table when the data is entered.
    
         function filterTable() {

           let filteredData = tableData;

           // Loop through all of the filters and keep any data that matches the filter values
           Object.entries(filters).forEach(function([kee, val]) {
             // if the value is not empty then filteredData = data set that matches filters
             if (val != "") {
               filteredData = filteredData.filter(row => row[kee] === val);
             }
           });
           // Finally, rebuild the table using the filtered data
           buildTable(filteredData); 
         };
   
   * The `filterTable()` function calls the `buildTable()` function that builds the table per user input.  In the example, the table is built using objects containing the shape "fireball."   
   
          function buildTable(data) {
            // First, clear out any existing data
            tbody.html("");

            // Next, loop through each object in the data
            // and append a row and cells for each value in the row
            data.forEach((dataRow) => {
              // Append a row to the table body
              let row = tbody.append("tr");

              // Loop through each field in the dataRow and add
              // each value as a table cell (td)
              Object.values(dataRow).forEach((val) => {
                let cell = row.append("td");
                cell.text(val);
              });
            });
          }
   
   * Then after "ca" is entered, the event listener notices the change and calls the `updatedfilters()` function again and the process is repeated but now it's filtering both "fireball" and "ca".



   
   
   





<p align="left">
  <img src="static/images/UFO_Mobile.png" width="200">
</p>

## Summary:

1. Address one drawback of this webpage.
2. Address two additional recommendations for further development.

[Back to the Table of Contents](https://github.com/rkaysen63/UFOs/blob/master/README.md#table-of-contents)

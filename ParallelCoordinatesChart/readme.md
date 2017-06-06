#Parallel Coordinates Chart
##Version 1.2

##Usage

Please see **example.html** to see this in action.
**NOTE: Version 1.2 now requries d3 v4.**
**Version 1.2 also changed now callbacks work and requires changes be made to any client programs**

##Creating a new chart
To create a new chart, simply call the constructor like so:
```javascript
	var chart = new ParallelCoordinatesChart(parent, pathToCSV, filter, callback);
```
####Arguments
* **parent**: A d3 selection of the container for the chart. The chart will be appended to this and will be made to fill the entire size of the parent. If you wish to use padding, margins or other layout techniques, it is recommended to wrap this parent in another container first.
* **pathToCSV**: The path to CSV file that the chart will use for its data.
* **filter (optional)**: An array of dimensions to ignore when building the chart. Dimensions that are filtered out will still be accessible from results, but will not appear on the chart.
* **callback (optional)**: Done Loading callback function. Called when the chart has finished loading. Some functions of the chart may not work if called before the chart has finished loading.

##Commonly-accessed fields
* **ParallelCoordinatesChart.results**: An array of all the results in the chart (as objects). Since most callbacks only provide an index, you will want to make use of this array to access the fields of a result.
Example:
```javascript
	chart.dispatch.on("mouseover", function(index) {
		console.log('Moused over: ' + chart.results[index].name);
	});
```
* **ParallelCoordinatesChart.query**: An array of the indices of all currently selected results.
* **ParallelCoordinatesChart.dimensions**: An array of the dimensions used in the chart. (This will not include dimensions filtered out in the **filter** argument of the constructor).
* **ParallelCoordinatesChart.dispatch**: The emitter for various event triggers (see **Events**)

##Events
Creating and interacting with the chart will often trigger events through the **dispatch**. The functions for these events can be set using the **on** function.
* **selectionChanged(query)**: 
```javascript
chart.dispatch.on("selectionchange",function(query){
	//handle event
});
```
This function is called whenever the selected results in the chart change. **query** is an array of the indices of all of the selected results.
* **mouseover (index, event)**:
```javascript
chart.dispatch.on("mouseover",function(index, event){
	//handle event
});
```
This function is called when a result in the chart is moused-over. **index** is the index of the moused-over result. **event** is the mouse event that triggered it. When a result is moused-off, this function is called again, but with **index** being **null**.

##Commonly-used functions
* **ParallelCoordinatesChart.updateSize()**: Redraw the chart to fit into the current size of its parent. This should be called whenever its parent may change size. If not, the chart will still fit inside the parent, but it will appear distorted.
* **ParallelCoordinatesChart.setHighlight(index)**: Highlight the path of the result represented by the given index in the chart. To un-set the highlight, call this again with **null** for **index**.

####Again, look at **example.html**. It will all make more sense then.

##Other things
###To change the dataset used by the chart:
Simply create another chart by calling the constructor again, set to look at the new csv file. (Don't forget to remove the old chart from its parent, first)
```javascript
//This function can be called multiple times with different csv files.
//It will simply replace the current chart (all callbacks should work the same)
function load(pathToCSV) {
	d3.select('#svgContainer').html('');
	chart = new ParallelCoordinatesChart(d3.select('#svgContainer'),
										pathToCSV,
										doneLoading,
										onSelectionChange,
										updateInfoPane,
										filter);
}
```

#Changelog
##v1.2 (June 6, 2016)
- Updated to d3 v4
- selectionChanged and mouseOverChanged have been renamed to selectionchange and mouseover, and now work as events through **chart.dispatch**
##v1.11 (June 5, 2016)
- Fixed Brushes on NaN dimensions not resizing vertically
##v1.1 (June 2, 2016)
- Added support for NaN dimensions
##v1.0 (June 1, 2016)
- Initial


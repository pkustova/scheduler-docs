 Exporting Scheduler to PNG
===========================
Starting from version 4.1, dhtmlxScheduler provides  a possibility to export the scheduler to the PNG format. 

<br>

To export the scheduler as a PNG image, do the following steps:

<ol>
	<li>Include the <b>"http://export.dhtmlx.com/scheduler/api.js"</b> file on the page to enable the online export service:
~~~html
<script src="codebase/dhtmlxscheduler.js"></script>
<script src="http://export.dhtmlx.com/scheduler/api.js"></script>  /*!*/
<link rel="stylesheet" href="codebase/dhtmlxscheduler.css" type="text/css">
~~~
</li>
	<li>Call the <a href="png.md#parametersoftheexportmethod">exportToPNG</a> method to export the scheduler: 
~~~html
<input value="Export to PNG" type="button" onclick='scheduler.exportToPNG()'>/*!*/

<script>
	scheduler.config.xml_date="%Y-%m-%d %H:%i";
	scheduler.init('scheduler_here',new Date(2009,5,30),"month");
	scheduler.load("data/events.xml");
</script>
~~~

</li>
</ol>
{{sample
04_export/06_online_export.html
}}

Parameters of the export method
----------------------------------------------------------
The **exportToPNG()** method takes as a parameter the object with 6 possible properties (all of the properties are optional):

<table class="webixdoc_links">
	<tbody>
    	<tr>
			<td class="webixdoc_links0"><b>name</b></td>
			<td>(<i>string</i>) the name of the output file</td>
		</tr>
       <tr>
			<td class="webixdoc_links0"><b>format</b></td>
			<td>(<i>'A0', 'A1', 'A2', 'A3', 'A4', 'A5'</i>) the format of the output PNG image/td>
		</tr>
        <tr>
			<td class="webixdoc_links0"><b>orientation</b></td>
			<td>(<i>'portrait', 'landscape'</i>) sets the orientation of the output PNG image</td>
		</tr>        
        <tr>
			<td class="webixdoc_links0"><b>zoom</b></td>
			<td>(<i>number</i>) sets the zoom coefficient of the output PNG image</td>
		</tr>
        <tr>
			<td class="webixdoc_links0"><b>header</b></td>
			<td>(<i>string</i>) specifies the header that will be added to the output PNG image. Note, you can use any HTML here</td>
		</tr>
        <tr>
			<td class="webixdoc_links0"><b>footer</b></td>
			<td>(<i>string</i>) specifies the footer that will be added to the output PNG image. Note, you can use any HTML here</td>
		</tr>
    </tbody>
</table>
{{snippet
Calling the export method with optional properties
}}
~~~js
scheduler.exportToPNG({
	format:"A4",
	orientation:"portrait",
	zoom:1,
    header:"<h1>My company</h1>",
    footer:"<h4>Bottom line</h4>"
});
~~~


##Name of the output file

To set a custom name for the output file, use the **name** property in the 
<a href="export.md#parametersoftheexportmethod">exportToPNG</a> method:

~~~js
scheduler.exportToPNG({
	name:"my_beautiful_scheduler.png"/*!*/
});
~~~



##Header/footer of the output file
To add the header/footer to the output PNG file, use the **header**/**footer** properties in the 
<a href="export.md#parametersoftheexportmethod">exportToPNG</a> method:

{{note
Note, you can use any HTML while specifying the parameters. While specifying images, remember that you need to set global paths as values of the "src" attribute
}}

~~~js
scheduler.exportToPNG({
    name:"myscheduler.png",
    header:"<h1>My company</h1>",
    footer:"<h4>Bottom line</h4>"
});
~~~


##Custom style for the output file
To apply a custom style for the scheduler, provide the stylesheet with your custom CSS classes:

<ul>
	<li>through a link:
~~~js
scheduler.exportToPNG({
    name:"calendar.png",
    header:'<link rel="stylesheet" href="http://mysite.com/custom.css">' /*!*/
});
~~~
	</li>
	<li>or through the 'style' tag:
~~~js
scheduler.exportToPNG({
    name:"calendar.png",
    header:'<style>... custom css classes here ...</style>' /*!*/
});
~~~
	</li>
</ul>
<br>

Note, the aforementioned solution works for the global HTTP reference. If you have CSS classes specified in an Intranet/local environment, you can embed all styles as in:

~~~js
scheduler.exportToPNG({
	header:"<style>.tier1{   background: red;   color:white;}</style>"
});
~~~


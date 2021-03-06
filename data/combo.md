 Combo
==============

A combo box presented by the <a href="http://docs.dhtmlx.com/doku.php?id=dhtmlxcombo:toc">DHTMLX Combo component</a>.

<img src="combo_editor.png"/>

~~~js
var holders = [
	{ key: 1, label: 'James' },
    { key: 2, label: 'Alex' },
    { key: 3, label: 'Antony' },
    { key: 4, label: 'Andrew' }
];
            
scheduler.locale.labels.section_holder = "Holder";

scheduler.config.lightbox.sections = [
	{ name:"description", height:50, map_to:"text", type:"textarea", focus:true },
	{ name:"holders", options:holders, map_to:"holders", type:"combo", 
    image_path:"../common/dhtmlxCombo/imgs/", height:30, filtering:true},
	{ name:"time", height:72, type:"time", map_to:"auto"}
];
~~~ 

{{sample
	02_customization/15_combo_select.html
}}


Initialization
-----------------------
To add the Combo control to the lightbox, follow these steps:

<ol>
	<li><b>Include the <a href="http://docs.dhtmlx.com/doku.php?id=dhtmlxcombo:toc">dhtmlxCombo</a> files and the 'ext/dhtmlxscheduler_editors.js' extension file on the page:</b>
~~~js
<script src="../codebase/dhtmlxscheduler.js" ...></script>
<link rel="stylesheet" href="../codebase/dhtmlxscheduler.css" ...>

<script src="../codebase/ext/dhtmlxscheduler_editors.js" ...></script>

<link rel="stylesheet" href="common/dhtmlxCombo/dhtmlxcombo.css" ..>
<script src="common/dhtmlxCombo/dhtmlxcombo.js" ...></script>
~~~
    </li>
    <li><b>Add the section to the lightbox configuration:</b>
~~~js
scheduler.config.lightbox.sections = [
	{ name:"description", ... },
	{ name:"holders", options:holders, map_to:"holders", type:"combo", 
    image_path:"../common/dhtmlxCombo/imgs/", height:30, filtering:true},
	{ name:"time", ...}
];
~~~
	</li>
    <li><b>Set the label for the section:</b>
~~~js
scheduler.locale.labels.section_holders = "Holder";
~~~
	</li>
</ol>

        

{{sample
	02_customization/15_combo_select.html
}}

Properties
---------------------------------------------

The following properties are mostly important and commonly set for the 'combo' control (see the full list <a href="api/scheduler_lightbox_config.md">here</a>):

<table class="webixdoc_links">
	<tbody>
    	<tr>
			<td class="webixdoc_links0"><b>name</b></td>
			<td>(<i>string</i>) the section's name </td>
		</tr>
        <tr>
			<td class="webixdoc_links0"><b>height</b></td>
			<td>(<i>number</i>) the section's height</td>
		</tr>
        <tr>
			<td class="webixdoc_links0"><b>map_to</b></td>
			<td>(<i>string</i>) the name of a data property that will be mapped to the section</td>
		</tr>
        <tr>
			<td class="webixdoc_links0"><b>type</b></td>
			<td>(<i>textarea,time,select,template,multiselect,radio,checkbox</i>) the type of the section's control</td>
		</tr>
        <tr>
			<td class="webixdoc_links0"  style="vertical-align: top;"><b>options</b></td>
			<td>(<i>array of objects</i>) defines select options of the control (<b>for 'select', 'multiselect, 'radio' controls</b>).<br> Each object in the array specifies a single option and takes these properies:
            	<ul>
					<li><b>key</b> -   (<i>string</i>) the option's id. This attribute is compared with the event's data property to assign options to events</li>
					<li><b>label</b> -   (<i>string</i>) the option's label</li>
			</ul>
             </td>
		</tr>
        <tr>
			<td class="webixdoc_links0"><b>image_path</b></td>
			<td>(<i>string</i>) the path to dhtmlxCombo images</td>
		</tr>
        <tr>
			<td class="webixdoc_links0"><b>filtering</b></td>
			<td>(<i>boolean</i>) activates the auto-filtering support (options will be filtered as you type). Optional</td>
		</tr>
        <tr>
			<td class="webixdoc_links0"><b>script_path</b></td>
			<td>(<i>string</i>) the path to the server-side script that will provide loading combo's options from the server. Optional</td>
		</tr>
        <tr>
			<td class="webixdoc_links0"><b>cache</b></td>
			<td>(<i>boolean</i>) enables/disables caching of the script responses (enabling the property is recommended). Optional</td>
		</tr>
    </tbody>
</table>



Populating the control with data
-------------------------------------------

In general, to set values for the Combo control you should use the [options](api/scheduler_lightbox_config.md) parameter:

~~~js
scheduler.config.lightbox.sections = 
	{ 
    	name:"holders", type:"combo", 
        ...
    	options:[
			{ key: 1, label: 'James' },
    		{ key: 2, label: 'Alex' },
    		{ key: 3, label: 'Antony' },
    		{ key: 4, label: 'Andrew' }
	]},
    ...
];
~~~

Items in the  [options](api/scheduler_lightbox_config.md) parameter must have 2 mandatory properties:

- **key** - the option's id
- **label** - the option's label

Populating the control with data from the server
-----------------------------------------------
To populate the Combo control from the server, use the **script_path** property, where specify the path to the server-side script that will 
handle the server requests:

~~~js
<?php
	require_once('../../connector-php/codebase/combo_connector.php');
	require_once("../common/config.php");

	$combo = new ComboConnector($res, $dbtype);

	$combo->event->attach("beforeFilter", "by_id");
	function by_id($filter) {
		if (isset($_GET['id']))
			$filter->add("item_id", $_GET['id'], '=');
	}	

	$combo->dynamic_loading(3);
	$combo->render_table("Countries","item_id","item_nm");

?>
~~~

Auto-filtering mode
--------------------------
The auto-filtering mode is the mode, when options are automatically filtered as the user types. To activate the mode, 
set the **filtering** property to *true*:

~~~js
scheduler.config.lightbox.sections = [
	{ name:"holders", type:"combo", filtering:true, ... },
    ...
];
~~~
{{note
Note, you can use the auto-filtering mode, regardless of the source, you load data from ( client or server-side).
}}


Read more on the topic in the dhtmlxCombo documentation <a href="http://docs.dhtmlx.com/doku.php?id=dhtmlxcombo:filtering">dhtmlxCombo. Filtering</a> .
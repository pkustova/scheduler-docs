Manipulations with Lightbox
==========================================

##Getting/setting the control value
To get/set the value of the section's control, use the api/scheduler_formsection.md object as in:

~~~js
//to get the value
var value = scheduler.formSection('description').getValue();

//to set the value
scheduler.formSection('description').setValue('abc');
~~~

{{sample
	02_customization/22_opertions_with_lightbox.html
}}

##Checking whether the lightbox is open
To check whether the lightbox is currently open or closed, use the **lightbox_id** property of the state object returned by the api/scheduler_getstate.md method.<br> If the lightbox is open - the method will return the id 
of the open event, otherwise 'null' or 'undefined'

~~~js
if (scheduler.getState().lightbox_id){
	//the code for the opened lighbox
} else {
	//the code for the closed lighbox
}
~~~

##Mapping DB fields to the lightbox sections

To map a DB field to a lightbox section, make the following:

<ol>
 <li>On the server side, just add some field to the list of data fields:
~~~php
$scheduler->render_table(
    "events_rec",
    "event_id",
    "start_date,end_date,text,rec_type"
);
~~~
If you are generating XML by a custom code, just add the subtag to the event as:

~~~xml
<event id='50' >
	<start_date><![CDATA[2009-06-10 00:00:00]]></start_date>
	<end_date><![CDATA[2009-06-12  00:00:00]]></end_date>
	<text><![CDATA[New event]]></text>
	<rec_type><![CDATA[ some value ]]></rec_type>
</event>
~~~
</li>
	<li>On the client side, set the same name for the <b>map_to</b> attribute of a section:
~~~js
{name:"reccuring", map_to:"rec_type" ...}
~~~
	</li>
</ol>


Automatic end date in the Time control
--------------------------------------
To set the initial duration of events and make the end date automatically change to preserve this value, use
 the api/scheduler_event_duration_config.md and api/scheduler_auto_end_date_config.md properties:

~~~js
//specify the event duration in minutes for the auto_end_time parameter
scheduler.config.event_duration = 60; 
scheduler.config.auto_end_date = true;
~~~

{{sample
	02_customization/11_auto_end_date.html
}}

With such configuration, each time the user changes the start event time or date in the lightbox, 
the end event time and date will be changed automatically, in order to make the event duration equal to 60 minutes 
(the value of the api/scheduler_event_duration_config.md option).

##Setting the default value for a lightbox's control
To set the default value for a lightbox's section, use the **default_value** property of the section's object.

For example, you have added a custom section to the lightbox  - 'Location' -  that displays the event's location. 
When the user creates a new event, the field will be just empty. To correct such behaviour and to display, say, the address of 
the Greenwich Observatory by default, specify the lightbox as in:

~~~js
scheduler.config.lightbox.sections=[
	{ name:"description", height:50, map_to:"text", type:"textarea", focus:true },
    { name:"location", height:43, map_to:"event_location", type:"textarea", 
    default_value:"Blackheath Avenue London,Greenwich,Greater London SE10 8XJ,UK"},
    { name:"time", height:72, type:"time", map_to:"auto"}
];
~~~
Note, the **default_value** property sets the default value for the lightbox's section, not for a new event, i.e. a new event  gets the specified value only after the user opens the lightbox and saves the event.

To set the default value directly for new events, use the api/scheduler_oneventcreated_event.md event:

~~~js
scheduler.attachEvent("onEventCreated", function(id,e){
	scheduler.getEvent(id).location = 'Blackheath Avenue London, Greenwich...';
    scheduler.updateEvent(id); // renders the updated event
    return true;
});
~~~



##Changing the order of date-time controls and removing time selectors
You have the possibility to change the order of date-time controls in the 'Time period' section or to remove some of them. To do this, 
use the **time_format** property:

~~~js
scheduler.config.lightbox.sections=[
  {name:"description", height:130, map_to:"text", type:"textarea" , focus:true},
  {name:"time", ..., time_format:["%H:%i","%m","%d","%Y"]}
];

~~~

{{note
Note, you can't change the data presentation format, just the order of items in the array. 
}}

For example, you can change the format as in:

~~~js
//default
time_format:["%H:%i", "%m", "%d", "%Y"] 
//month goes first
time_format:["%m","%d", "%Y", "%H:%i"]
//the year selector is removed
time_format:["%H:%i", "%m", "%d"]
//incorrect
time_format:["%H:%i", "%M", "%d", "%Y"] //"%m" was changed to "%M"
~~~

## Read-only mode 
The detailed information about the read-only mode see in chapter readonly.md.

## Making a section hidden for some events 
To make a section hidden for specific events, redefine its **set_value** method as in:<br>


~~~js
scheduler.form_blocks.textarea.set_value=function(node,value,ev){
	node.firstChild.value=value||"";
    var style = ev.some_property?"":"none";
    node.style.display=style; // editor area
    node.previousSibling.style.display=style; //section header
    scheduler.setLightboxSize(); //correct size of lightbox
}
~~~

### 'Full-day event' option
To add the 'full-day event' option to the lightbox, set the api/scheduler_full_day_config.md option to *true*.
For this, just add the next codeline:

~~~js
scheduler.config.full_day  = true;
~~~

Once, the api/scheduler_full_day_config.md option is enabled, the **Full Day** checkbox 
will be displayed in the left part of the **Time period** section. After selection, 
all entry fields of the section will be blocked, and the event duration will be set as 
a full day from **0.00am** the current cell date to **0.00am** next day.

{{sample
	02_customization/12_full_day_event.html
}}


Types of  lightbox
------------------------------------------------------

The lightbox can be presented in one of 2 types:

- Standard (wide);
- Short.

In the default skin, the lightbox can be presented only in the standard (wide) type, while in the 'glossy' or 'classic' skin you can choose between types.

To set the desired type, use the api/scheduler_wide_form_config.md property:

~~~js
scheduler.config.wide_form = true;

~~~


**Standard (wide) lightbox**:

![scheduler_wide_form.png](scheduler_wide_form.png)

**Short form**:

![scheduler_standard_form.png](scheduler_standard_form.png)



### Button in the section header 
It's possible to have a custom button in the section header. 
To add a button to the header of a section, do the following steps:

<ol>
	<li> Specify the 'button' property to the section object:
~~~js
{name:"description", height:130, map_to:"text", type:"textarea", button:"help"}
~~~
	</li>
    <li> Set the label for the button:
~~~js
//'help' is the value of the 'button' property
scheduler.locale.labels.button_help="Help label";
~~~
	</li> 
    <li>Specify the handler of button clicks:
~~~
scheduler.form_blocks.textarea.button_click=function(index,button,shead,sbody){
	...
}
~~~
where:
	<ul>
    	<li>index - (number) the section's index. Zero-based numbering</li>
		<li>button - (Dom element) the HTML element of the button</li>
		<li>shead - (Dom element) the HTML element of the section header</li>
		<li>sbody - (Dom element) the HTML element of the section body</li>

	</ul>
    </li>
</ol>
You can define the image used for the button through the next css class:

~~~js
.dhx_custom_button_help{
	background-image:url(imgs/but_help.gif);
}
~~~


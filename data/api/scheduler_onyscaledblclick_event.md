onYScaleDblClick
=============

@short: fires when the user makes a double click on a cell on the y-axis (the Timeline view only)
	

@params: 
- index		number	 the row index of the clicked cell (zero-based numbering)
- section		object	 a data object of the clicked cell
- e			Event	a native event object

@example: 
scheduler.attachEvent("onYScaleDblClick", function (index, section, e){
	//any custom logic here
});



@template:	api_event
@descr: 

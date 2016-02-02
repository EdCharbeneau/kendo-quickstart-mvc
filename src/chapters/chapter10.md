## Kendo Themes

In this chapter...

### Theme change

- stop the project
- Right click on project > Telerik UI For MVC > Configure Project
- Set theme to Nova
- Move     @Styles.Render("~/Content/css") just above </head>

- Set Charts to Nova

	Find
	@(Html.Kendo().Chart<QuickStart.Models.MonthlySalesByEmployeeViewModel>()
							...
	                        .AutoBind(false)
                        	.Events(e => e.DataBound("onQuarterSalesDataBound"))
	)
	
	Append
	@(Html.Kendo().Chart<QuickStart.Models.MonthlySalesByEmployeeViewModel>()
							...
	                        .AutoBind(false)
                        	.Events(e => e.DataBound("onQuarterSalesDataBound"))
                        	.Theme("nova")
	)

	Find
	@(Html.Kendo().Chart<QuickStart.Models.MonthlySalesByEmployeeViewModel>()
							...
							.Events(e => e.DataBound("onAverageSalesDataBound"))
	                        .AutoBind(false)
	)
	
	Append
	@(Html.Kendo().Chart<QuickStart.Models.MonthlySalesByEmployeeViewModel>()
							...
	                        .AutoBind(false)
                        	.Events(e => e.DataBound("onAverageSalesDataBound"))
                        	.Theme("nova")
	)

### Finishing the app theme

- Add CSS primer
- Open Views/Shared/_Layout.cshtml

	Find
	@Styles.Render("~/Content/css")
	</head>

	Update
	@Styles.Render("~/Content/css")
    <link href="~/Content/site-nova.css" rel="stylesheet" />
	</head>

- Refresh
- open site-nova.css

	Find
	/* Side Panel - Employee List */

	Append
	/* Side Panel - Employee List */
	#menuPanel .k-widget.k-datepicker.k-header {
	    width: 100%;
	}

	#employee-list > ul {
    	margin: 0 -10px;
	}

	Find
	/* Small Devices, Tablets, and Up */
	@media only screen and (min-width : 768px) {

	}

	Update
	/* Small Devices, Tablets, and Up */
	@media only screen and (min-width : 768px) {
	    .app-wraper {
	        position: relative;
	        margin-left: -15px;
	    }
	}


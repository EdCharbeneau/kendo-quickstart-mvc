## Kendo Themes

Kendo UI widgets include a number of predefined themes. In this chapter you'll learn how to make your app look amazing with themes.

### Theme change

<h4 class="exercise-start">
    <b>Exercise</b>: Theme the application.
</h4>

If the project is running, stop the project.

In Visual Studio's Project Explorer, Right click on the project and choose Telerik UI For MVC > Configure Project from the menu.

From the Project Configuration Wizard, choose the Nova theme.

Open `Views/Shared/_Layout.cshtml` and move `@Styles.Render("~/Content/css")` just above the closing head tag `</head>`.

Run the application to see the theme applied to the Kendo UI widgets.

Next, you'll finishing the themeing the app by adding styles to non-Kendo UI elements creating a completely custom look.

A style sheet was installed with the boilerplate to give you a jump-start. Let's add that to the applciation by opening `Views/Shared/_Layout.cshtml` and adding a reference to `~/Content/site-nova.css` just above the closing head tag `</head>`.

Note: This is CSS, so the order in which the style sheets are added is very important.

    <link href="~/Content/site-nova.css" rel="stylesheet" />
	</head>

Refresh the application and notice the look is starting to come together. There's just a few items that could use some fine-tuning. Let's add some additional styles to `site-nova.css` to complete the theme.

Open site-nova.css and find `/* Side Panel - Employee List */` and a style that sets the datepicker widgets inside the menuPanel to %100 width of the container.

The resulting code should be:
	
	/* Side Panel - Employee List */
	#menuPanel .k-widget.k-datepicker.k-header {
	    width: 100%;
	}

Next, offset the employee list so its content lines up with the left edge of its container.
    
	#employee-list > ul {
    	margin: 0 -10px;
	}
    
Find `/* Small Devices, Tablets, and Up */`. Here you'll find a media query that will hold some styles that are only applied to scree sizes above `768px`.

	@media only screen and (min-width : 768px) {

	}

Inside the media query add a left margin of `-15` and set the `position` to `relative`. This style will align the app with the left hand edge of the screen.
 
	/* Small Devices, Tablets, and Up */
	@media only screen and (min-width : 768px) {
	    .app-wraper {
	        position: relative;
	        margin-left: -15px;
	    }
	}

Finally, set the Kendo UI Chart themes.

Open `_MontlySalesByEmployee.cshtml` and set the `Theme` property to `nova` on the `EmployeeAverageSales` chart.

	@(Html.Kendo().Chart<KendoQsBoilerplate.MonthlySalesByEmployeeViewModel>()
        .Name("EmployeeAverageSales")
        ...
        .AutoBind(false)
       	.Events(e => e.DataBound("onAverageSalesDataBound"))
        .Theme("nova")
	)

Open `_QuarterToDateSales.cshtml` and set the `Theme` property to `nova` on the `EmployeeQuarterSales` chart.

    @(Html.Kendo().Chart<KendoQsBoilerplate.QuarterToDateSalesViewModel>()
        .Name("EmployeeQuarterSales")
        ...
	    .AutoBind(false)
        .Events(e => e.DataBound("onQuarterSalesDataBound"))
        .Theme("nova")
	)
    
<div class="exercise-end"></div>
## Input Controls

In this chapter you'll learn how to add Kendo UI components to your application. UI for MVC has powerful HTML Helpers that render Kendo UI components.

### Kendo Helper Overview

**Server-side wrappers**

Telerik UI for ASP.NET MVC is a set of server-side wrappers. A server-wrapper does the following.

- Allows the developer to configure a Kendo UI widget via C# or VB.NET code - set its value, data source etc.
- Renders the HTML and JavaScript needed to initialize the Kendo UI widget. The widget options propagate to the client-side via the widget initialization script.

![Server-side wrapper outputs HTML and JavaScript](images/chapter2/wrapper-output.png)

**Configuration**

The Kendo HtmlHelper exposes all Kendo UI server wrappers.

![Kendo HtmlHelper extension method](images/chapter2/kendo-extension.png)

**Widget options**

The widget options are exposed via fluent interface.

![Fluent interface](images/chapter2/fluent-interface.png)

Below is an example of how a Numeric Text Box input is created:

    @(Html.Kendo().NumericTextBox()
        .Name("name") // set the name of the NumericTextBox
        .Value(10) //set the value
        .Spinners(false) // disable the spinners
    )
    
### Adding a Kendo DatePicker

Let's open the `Index.cshtml` page under the folder `views/home/`. The `Index.cshtml` page is where most of the application's UI lives. This page currently contains basic HTML inputs to collect date input from the user. To provide a better user experience, replace the standard HTML inputs with Kendo date picker controls. The Kendo date picker controls offer users a fly out calendar to choose a desired date.

> Note: The Kendo date picker control is touch and mouse friendly. No additional code is necessary to support tablets and phones.

<h4 class="exercise-start">
    <b>Exercise</b>: Replace StatsFrom and StatsTo TextBoxes with Kendo Date Pickers
</h4>

Open `Views/Home/Index.cshtml`

Find the `StatsFrom` text box 

    <!-- Stats From Date Picker -->
	@Html.TextBox("StatsFrom", new DateTime(1996, 1, 1))

Replace the code with a Kendo date picker

	<!-- Stats From Date Picker -->
	@(Html.Kendo().DatePicker()
           .Name("StatsFrom")
           .Value(new DateTime(1996, 1, 1))
    )        

Find the `StatsTo` text box

	<!-- Stats To Date Picker -->
	@Html.TextBox("StatsTo", new DateTime(1996, 1, 1))    

Replace the code with a Kendo date picker

	<!-- Stats To Date Picker -->
	@(Html.Kendo().DatePicker()
    		.Name("StatsTo")
			.Value(new DateTime(1998, 8, 1))
	)
    
The Kendo HTML helper's fluent interface let you configure their behavior and appearance. The code you just added uses the following properties:

- Name: Sets the rendered HTML element's id property.
- Value: Sets a default selected date value for the date picker 

<div class="exercise-end"></div>

After you run your app with this change, you will see a calendar icon in the **Stats from** field. Click or tap the icon to reveal the date picker:

![Tap to show a date picker](images/chapter2/date-picker-flyout.jpg)
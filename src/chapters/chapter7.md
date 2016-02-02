## Kendo Datasource

In this chapter you'll learn how to work with Kendo UI datasources.

### Working with the Kendo Datasource

At this point the dashboard is showing all invoice data. Let's use the EmployeeList list view and StatsFrom/StatsTo date pickers to filter the invoice grid by invoking the grid's datasource. 

<h4 class="exercise-start">
    <b>Exercise</b>: Create a filter.
</h4>

In the view `/Views/Home/Index.cshtml` find the scripts section.

	<script>
		...
    </script>
	
Add a function named `getEmployeeFilter` that gets the `employeeId`, `salesPerson`, `statsFrom` and `statsTo` values and returns a JSON object. 

The resulting code should be:

    function getEmployeeFilter() {
        var employee = getSelectedEmployee(),
            statsFrom = $("#StatsFrom").data("kendoDatePicker"),
            statsTo = $("#StatsTo").data("kendoDatePicker");

        var filter = {
            employeeId: employee.EmployeeId,
            salesPerson: employee.FullName,
            statsFrom: statsFrom.value(),
            statsTo: statsTo.value()
        }
        return filter;
    }

In the view `/Views/Invoice/Index.cshtml` find the EmployeeSales Grid.    
	
	@(Html.Kendo().Grid<KendoQsBoilerplate.Invoice>()
	      .Name("EmployeeSales")
		  ...
	      .Scrollable(scrollable => scrollable.Enabled(false))
	      .DataSource(dataSource => dataSource
	          .Ajax()
	          .Read(read => read.Action("Invoices_Read", "Invoice"))
	      )
	)

On the Grid's `DataSource` property, set the `Data` property to `getEmployeeFilter`. The `Data` property supplies additional data to the server, in this case the datas is our filter parameters.

    .DataSource(dataSource => dataSource
                .Ajax()
                .Read(read => read.Action("Invoices_Read", "Invoice")
                .Data("getEmployeeFilter"))
    )

Add the property `AutoBind` to the end of the property chain and set the value to `false`. Setting AutoBind to false tells UI for MVC that the datasource's `Read` action is invoked on manually on the client.

The resulting code should be:

	@(Html.Kendo().Grid<QuickStart.Models.Invoice>()
	      .Name("EmployeeSales")
	      ...
	      .Scrollable(scrollable => scrollable.Enabled(false))
	      .DataSource(dataSource => dataSource
	          .Ajax()
	          .Read(read => read.Action("Invoices_Read", "Invoice")
	          .Data("getEmployeeFilter"))
	      )
		  .AutoBind(false)
	)
    
In the view `/Views/Home/Index.cshtml` add a function named `refreshGrid`, this function will invoke the Grid's `Read` action.

The resulting code should be:

	function refreshGrid() {
        var employeeSales = $("#EmployeeSales").data("kendoGrid");
        employeeSales.dataSource.read();
    }

Find the function `onCriteriaChange` and add a call to the `refreshGrid` function. This will cause the Grid's data to refresh whenever the employee selection changes.

The resulting code should be:

	function onCriteriaChange() {
        updateEmployeeAvatar();
        refreshGrid();
	}

Next, we'll need to update the Grid's `Read` action to apply the filter using Entity Framework.

Open `Controllers/InvoiceController.cs` and find the `Invoices_Read` action.

    public ActionResult Invoices_Read([DataSourceRequest]DataSourceRequest request)
    {
        IQueryable<Invoice> invoices = db.Invoices;
        DataSourceResult result = invoices.ToDataSourceResult(request, invoice => new {
            OrderID = invoice.OrderID,
            CustomerName = invoice.CustomerName,
            OrderDate = invoice.OrderDate,
            ProductName = invoice.ProductName,
            UnitPrice = invoice.UnitPrice,
            Quantity = invoice.Quantity,
            Salesperson = invoice.Salesperson
        });

        return Json(result);
    }

Add the parameters `salesPerson`, `statsFrom` and `statsTo` to the action.

    public ActionResult Invoices_Read([DataSourceRequest]DataSourceRequest request,
        string salesPerson,
        DateTime statsFrom, 
        DateTime statsTo)

Using the parameter values filter the invoices using a `Where` LINQ query.
  
The resulting code should be:

    public ActionResult Invoices_Read([DataSourceRequest]DataSourceRequest request,
        string salesPerson,
        DateTime statsFrom, 
        DateTime statsTo)
    {
        var invoices = db.Invoices.Where(inv => inv.Salesperson == salesPerson)
            .Where(inv => inv.OrderDate >= statsFrom && inv.OrderDate <= statsTo);
        DataSourceResult result = invoices.ToDataSourceResult(request, invoice => new {
            OrderID = invoice.OrderID,
            CustomerName = invoice.CustomerName,
            OrderDate = invoice.OrderDate,
            ProductName = invoice.ProductName,
            UnitPrice = invoice.UnitPrice,
            Quantity = invoice.Quantity,
            Salesperson = invoice.Salesperson
        });

        return Json(result);
    }

Run the project to see the behavior. Now the EmployeeList and EmployeeSales Grid are in sync. When an employee is selected, only that employees data will show in the Grid.

![](images/chapter7/datasource-filter.jpg)

<div class="exercise-end"></div>

At this point the EmployeeList is acting as a filter for the EmployeeSales Grid, however the data shown does not reflect the StatsFrom/StatsTo date range. With the filtering code in place, additional controls are wired up with relative ease. Let's wire up the StatsFrom/StatsTo DatePicker controls to the EmployeeSales Grid.

<h4 class="exercise-start">
    <b>Exercise</b>: Trigger the grid datasource from a DatePicker event.
</h4>

In the view `/Views/Home/Index.cshtml` find the StatsFrom DatePicker.

    @(Html.Kendo().DatePicker()
                    .Name("StatsFrom")
                    .Value(new DateTime(1996, 1, 1))
	)
                    
Add the `Events` property and set the `Change` event to `onCriteriaChange`.

    @(Html.Kendo().DatePicker()
                    .Name("StatsFrom")
                    .Value(new DateTime(1996, 1, 1))
                    .Events(e => e.Change("onCriteriaChange"))
	)

Find the StatsTo DatePicker,  the `Events` property and set the `Change` event to `onCriteriaChange`.

    @(Html.Kendo().DatePicker()
			        .Name("StatsTo")
			        .Value(new DateTime(1998, 8, 1))
			        .Events(e => e.Change("onCriteriaChange"))
	)

Save the changes and refresh the browser. The StatsFrom/StatsTo DatePickers and EmployeeList ListView will update the Grid with data based on the selected dates and employee.

![](images/chapter7/datasource-filter2.jpg)

<div class="exercise-end"></div>
## Kendo Datasource

In this chapter...

### Working with the Kendo Datasource

- Create a filter

	Find
	<script>
		...
    </script>
	
	Add
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
	
	function refreshGrid() {
        var employeeSales = $("#EmployeeSales").data("kendoGrid");
        employeeSales.dataSource.read();
    }

	Find
	
	@(Html.Kendo().Grid<QuickStart.Models.Invoice>()
	      .Name("EmployeeSales")
		  ...
	      .Scrollable(scrollable => scrollable.Enabled(false))
	      .DataSource(dataSource => dataSource
	          .Ajax()
	          .Read(read => read.Action("Invoices_Read", "Invoice"))
	      )
	)

	Append
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

	Find
	function onCriteriaChange() {
        updateEmployeeAvatar();
	}

	Append
	function onCriteriaChange() {
        updateEmployeeAvatar();
        refreshGrid();
	}

- Apply filter on the controller action Controllers/InvoiceController.cs

	Find
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

	Append
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

- run project
- inspect behavior list view and grid are in sync

- update date pickers with client side event

	Find
    @(Html.Kendo().DatePicker()
                    .Name("StatsFrom")
                    .Value(new DateTime(1996, 1, 1))
	)
                    
	Append
    @(Html.Kendo().DatePicker()
                    .Name("StatsFrom")
                    .Value(new DateTime(1996, 1, 1))
                    .Events(e => e.Change("onCriteriaChange"))
	)

	Find
    @(Html.Kendo().DatePicker()
			        .Name("StatsTo")
			        .Value(new DateTime(1998, 8, 1))
	)
			        

	Append
    @(Html.Kendo().DatePicker()
			        .Name("StatsTo")
			        .Value(new DateTime(1998, 8, 1))
			        .Events(e => e.Change("onCriteriaChange"))
	)

- Save / Reload page
- Change date to see grid reload
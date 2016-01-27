## Kendo Grid

In this chapter you will modify the scaffolded Grid code to further customize the Grid's appearance and incorporate the Grid into the dashboard  view. 

### Configuring Kendo Grid options

- Change the Grid's ID from "grid" to "EmployeeSales"
	
	Find
	@(Html.Kendo().Grid<QuickStart.Models.Invoice>()
	      .Name("grid")
		  ...
	)

	Update
	@{ Layout = null;}
	@(Html.Kendo().Grid<QuickStart.Models.Invoice>()
	      .Name("grid")
		  ...
	)
	
	Update
	@{ Layout = null;}
	@(Html.Kendo().Grid<QuickStart.Models.Invoice>()
	      .Name("EmployeeSales")
		  ...
	)

- View Controllers/InvoiceController.cs
- Add Grid to Index.cshtml

	Find
	<!-- Invoices -->
    @Html.Ipsum().table(5, 3, "d,t,n", new { @class = "table table-striped table-bordered" })

	Replace
	@Html.Action("Index","Invoice")

- Run Project
- Explore Sorting, Paging, and Exporting 

- Apply Date Format
- Open Invoice/Index.cshtml

	Find
    columns.Bound(c => c.OrderDate);

	Modify
	columns.Bound(c => c.OrderDate).Format("{0:MM/dd/yyyy}");

- Refresh page

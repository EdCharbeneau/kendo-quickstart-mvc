## Kendo Charts

In this chapter...

### Chart API

### Monthly sales chart

- Stop project

- Under Views/Home add a new empty partial view _MontlySalesByEmployee.cshtml

	Add
	@(Html.Kendo().Chart<QuickStart.Models.MonthlySalesByEmployeeViewModel>()
                        .Name("EmployeeAverageSales")
                        .HtmlAttributes(new { style = "height:30px;" })
                        .Series(series =>
                        {
                            series.Line(model => model.EmployeeSales)
                                .Width(1.5)
                                .Markers(m => m.Visible(false))
                                .Tooltip(t => t.Template("#=kendo.toString(value, 'c2')#"));
                        })
                        .CategoryAxis(c => c
                            .Date()
                            .Categories(x => x.Date)
                            .Visible(false)
                            .MajorGridLines(m => m.Visible(false))
                            .MajorTicks(mT => mT.Visible(false))
                        )
                        .Legend(leg => leg.Visible(false))
                        .ValueAxis(val => val.Numeric()
                                .Visible(false)
                                .Labels(lab => lab.Visible(false))
                                .MajorGridLines(m => m.Visible(false))
                                .MajorTicks(mT => mT.Visible(false)))
                        .DataSource(ds => ds
                            .Aggregates(a => a.Add(s => s.EmployeeSales).Average())
                            .Read(read => read.Action("EmployeeAverageSales", "Home")
                            .Data("getEmployeeFilter"))
                        )
                        .AutoBind(false)
	)

- Create a controller action
- open controllers/HomeController.cs

	Add
	public ActionResult EmployeeAverageSales(
        int employeeId,
        DateTime statsFrom,
        DateTime statsTo)
    {
        var result = EmployeeAverageSalesQuery(employeeId, statsFrom, statsTo);

        return Json(result, JsonRequestBehavior.AllowGet);
    }	

-open views/home/index.cshtml

	Find
	<!-- Montly Sales Chart -->
	@Html.Placehold(430, 120, "Chart")
	
	Replace
	<!-- Montly Sales Chart -->
	@Html.Partial("_MontlySalesByEmployee")

	Find
	<script>
		...
	</script>

	Add

	function refreshEmployeeAverageSales() {
        var employeeAverageSales = $("#EmployeeAverageSales").data("kendoChart");
        employeeAverageSales.dataSource.read();
    }

	Find
	function onCriteriaChange() {
        updateEmployeeAvatar();
        refreshGrid();
    }

	Append
	function onCriteriaChange() {
        updateEmployeeAvatar();
        refreshGrid();
        refreshEmployeeAverageSales();
    }

- Run and inspect
- Add Label
	
	Find
	<script>
		...
	</script>
	
	Add
	function onAverageSalesDataBound(e) {
        var label = $("#EmployeeAverageSalesLabel"),
            data = this.dataSource.aggregates()

        if (data.EmployeeSales) {
            label.text(kendo.toString(data.EmployeeSales.average, "c2"));
        } else {
            label.text(kendo.toString(0, "c2"));
        }
    }

	Find
	@(Html.Kendo().Chart<QuickStart.Models.MonthlySalesByEmployeeViewModel>()
							...
	                        .AutoBind(false)
	)
	
	Append
	@(Html.Kendo().Chart<QuickStart.Models.MonthlySalesByEmployeeViewModel>()
							...
	                        .AutoBind(false)
                        	.Events(e => e.DataBound("onAverageSalesDataBound"))
	)

### Quarter to date sales chart

- Stop project

- Under Views/Home add a new empty partial view _QuarterToDateSales.cshtml

	Add
	@(Html.Kendo().Chart<QuickStart.Models.QuarterToDateSalesViewModel>()
                        .Name("EmployeeQuarterSales")
                        .HtmlAttributes(new { style = "height:30px;" })
                        .Tooltip(false)
                        .Series(series =>
                        {
                            series.Bullet(m => m.Current, m => m.Target);
                        })
                        .Legend(leg => leg.Visible(false))
                        .CategoryAxis(cat => cat.Labels(lab => lab.Visible(false))
                            .MajorGridLines(m => m.Visible(false)).Visible(false))
                        .ValueAxis(val => val.Numeric()
                            .Labels(lab => lab.Visible(false))
                            .MajorGridLines(m => m.Visible(false))
                            .MajorTicks(mT => mT.Visible(false)))
                        .DataSource(ds => ds
                            .Read(read => read.Action("EmployeeQuarterSales", "Home")
                            .Data("getEmployeeFilter"))
                        )
                        .AutoBind(false)
	)

- Create a controller action
- open controllers/HomeController.cs

	Add
	public ActionResult EmployeeQuarterSales(int employeeId, DateTime statsTo)
    {
        DateTime startDate = statsTo.AddMonths(-3);

        var result = EmployeeQuarterSalesQuery(employeeId, statsTo, startDate);

        return Json(result, JsonRequestBehavior.AllowGet);
    }

-open views/home/index.cshtml

	Find
	<!-- QTD Sales Chart -->
	@Html.Placehold(430, 120, "Chart")
	
	Replace
	<!-- QTD Sales Chart -->
    @Html.Partial("_QuarterToDateSales")

	Find
	<script>
		...
	</script>

	Add
    function refreshEmployeeQuarterSales() {
        var employeeQuarterSales = $("#EmployeeQuarterSales").data("kendoChart");
        employeeQuarterSales.dataSource.read();
    }

	Find
    function onCriteriaChange() {
        updateEmployeeAvatar();
        refreshGrid();
        refreshEmployeeAverageSales();
    }

	Append
    function onCriteriaChange() {
        updateEmployeeAvatar();
        refreshGrid();
        refreshEmployeeAverageSales();
        refreshEmployeeQuarterSales();
    }

- Run and inspect
- Add Label
	
	Find
	<script>
		...
	</script>

	
	Add
    function onQuarterSalesDataBound(e) {
        var data = this.dataSource.at(0);
        $("#EmployeeQuarterSalesLabel").text(kendo.toString(data.Current, "c2"));
    }

	Find
	@(Html.Kendo().Chart<QuickStart.Models.MonthlySalesByEmployeeViewModel>()
							...
	                        .AutoBind(false)
	)
	
	Append
	@(Html.Kendo().Chart<QuickStart.Models.MonthlySalesByEmployeeViewModel>()
							...
	                        .AutoBind(false)
                        	.Events(e => e.DataBound("onQuarterSalesDataBound"))
	)
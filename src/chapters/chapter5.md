## Kendo ListView

In this chapter...

### This ListView control

- Stop project if running
- Open Home/Index.cshtml

	Find
    <!-- Employee List View -->
    
	Remove ul placeholder
	<nav id="employee-list">
            <h3>Team members</h3>
            <!-- Employee List View -->

    </nav>

	Add
	<!-- Employee List View -->
	@(Html.Kendo().ListView<Employee>()
                .Name("EmployeesList")
                .ClientTemplateId("EmployeeItemTemplate")
                .TagName("ul")
                .DataSource(dataSource =>
                {
                    dataSource.Read(read => read.Action("EmployeesList_Read", "Home"));
                    dataSource.PageSize(9);
                })
                .Selectable(s => s.Mode(ListViewSelectionMode.Single))
    )

- Discuss Kendo Templates
- Add Template

	Find 
	<!-- Kendo Templates -->
	<!-- /Kendo Templates -->
	Add
	
	<!-- Kendo Templates -->
	<script type="text/x-kendo-tmpl" id="EmployeeItemTemplate">
	    <li class="employee">
	        <div>
	            <img src="@(Url.Content("~/content/employees/"))#:EmployeeId#-t.png" />
	            <span> #: FullName #</span>
	        </div>
	    </li>
	</script>	
	<!-- /Kendo Templates -->

- Add Read Action
- Open Controllers/HomeController.cs

	Add to top of code

	using Kendo.Mvc.UI;
	using Kendo.Mvc.Extensions;

	
	Add to Class

    public ActionResult EmployeesList_Read([DataSourceRequest]DataSourceRequest request)
    {
        var employees = db.Employees.OrderBy(e => e.FirstName);
        return Json(employees.ToDataSourceResult(request, ModelState), JsonRequestBehavior.AllowGet);
    }

- Run and explore


### Adding a list view

### Kendo Templates


## Client Side

In this chapter...

### Working with client side events

- Select the first employee when data is bound

	Find
	<!-- Employee List View -->
	@(Html.Kendo().ListView<Employee>()
			...
        	.Selectable(s => s.Mode(ListViewSelectionMode.Single))
	)


	Append
	@(Html.Kendo().ListView<Employee>()
		...
		.Selectable(s => s.Mode(ListViewSelectionMode.Single))
		.Events(e => e.DataBound("onListDataBound"))
	)

	Result
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
            .Events(e => e.DataBound("onListDataBound"))
	)

- Add JavaScript

	Find 
	@section Scripts {
	    <script>
	        //Custom Scripts
	    </script>
	}

	Add
	@section Scripts {
	    <script>
	        //Custom Scripts
			function onListDataBound(e) {
		        this.select($(".employee:first"));
		    }
	    </script>
	}

- Refresh page

# N. Kendo Datasource

- Get Selected Employee
- Update Kendo Template

	Find	
	<!-- Employee List View -->
	@(Html.Kendo().ListView<Employee>()
			...
        	.Selectable(s => s.Mode(ListViewSelectionMode.Single))
            .Events(e => e.DataBound("onListDataBound"))
	)

	Add
		@(Html.Kendo().ListView<Employee>()
			...
        	.Selectable(s => s.Mode(ListViewSelectionMode.Single))
            .Events(e => e.DataBound("onListDataBound"))
					.Change("onCriteriaChange"))
	)

	Find
	<!-- Kendo Templates -->
		...
	<!-- /Kendo Templates -->
	
	Add
	<!-- Kendo Templates -->
	<script type="text/x-kendo-tmpl" id="employeeAvatarTemplate">
	    <img src="@(Url.Content("~/content/employees/"))#:EmployeeId#.png" />
	    <span>#:FullName#</span>
	</script>

	Find
	<script>
		...
    </script>
	
	Add
	function getSelectedEmployee() {
    	var employeeList = $("#EmployeesList").data("kendoListView"),
		employee = employeeList.dataSource.getByUid(employeeList.select().attr("data-uid"));
		return employee;
	}

	function updateEmployeeAvatar() {
        var employee = getSelectedEmployee(),
            template = kendo.template($("#employeeAvatarTemplate").html());

        //apply template
        $("#employee-about").html(template(employee));
    }

	function onCriteriaChange() {
        updateEmployeeAvatar();
	}

- Refresh page
- Watch template update with selected list item
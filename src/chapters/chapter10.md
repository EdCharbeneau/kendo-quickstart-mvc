## Go Responsive

In this chapter...

### Responsive Grid

- Run project
- Inspect responsive behavior
- Modify grid for responsive
- Open Views/Invoice/Index.cshtml

	Find

	.Columns(columns =>
    {
        columns.Bound(c => c.CustomerName);
        columns.Bound(c => c.OrderDate).Format("{0:MM/dd/yyyy}");
        columns.Bound(c => c.ProductName);
        columns.Bound(c => c.UnitPrice);
        columns.Bound(c => c.Quantity);
        columns.Bound(c => c.Salesperson);
    })
	
	Update
	.Columns(columns =>
    {
        columns.Bound(c => c.CustomerName).MinScreenWidth(900);
        columns.Bound(c => c.OrderDate).Format("{0:MM/dd/yyyy}");
        columns.Bound(c => c.ProductName).MinScreenWidth(768);
        columns.Bound(c => c.UnitPrice);
        columns.Bound(c => c.Quantity);
    })

- Refresh page and observe changes

- Responsive Panel

	Find
	<div class="app-wraper">
	    <!-- Menu Panel -->
	    <div class="hidden-xs" style="float:left;">

	Update
	<div class="app-wraper">
		<!-- Menu Panel -->
	    @(Html.Kendo().ResponsivePanel().Name("menuPanel").Breakpoint(768).Content(
	    @<div>

	Find
	<!-- /Menu Panel -->

	Append
    ))
    <!-- /Menu Panel -->

	Find
	<section id="app-title-bar" class="row">
	    <div class="col-sm-3">
	        <h1 class="title">@ViewBag.Title</h1>
	    </div>
	</section>
	
	Append	
	<div class="hamburger">
	    <!-- toggle button for responsive panel, hidden on large screens -->
	    @(Html.Kendo().Button()
	                .Name("menuPanelOpen")
	                .Content("menu")
	                .Icon("hbars")
	                .HtmlAttributes(new { @class = "k-rpanel-toggle" })
	    )
	</div>

- Add CSS background
- Open site.css

	Find
	/* Top Bar */

	.hamburger {
	    position: absolute;
	    top: 5px;
	    left: 5px;
	}

	Find
	/* Side Panel */	
	
	Add
	#menuPanel {
	    background-color: #fff;
	    padding: 10px;
	    z-index: 3;
	}

- Try responsive panel
- Add close button

	Find
	<!-- Menu Panel -->
    @(Html.Kendo().ResponsivePanel().Name("menuPanel").Breakpoint(768).Content(
    @<div>
        <h3>Report Range</h3>

	Update
	@(Html.Kendo().ResponsivePanel().Name("menuPanel").Breakpoint(768).Content(
    @<div>
        <div class="text-right">
            @(Html.Kendo().Button()
               .Name("menuPanelClose")
               .Content("Close")
               .Icon("close")
               .HtmlAttributes(new { @class = "k-rpanel-toggle" })
            )
        </div>
        <h3>Report Range</h3>

- Refresh and retry


### Responsive Panel
## ASP.NET MVC wrappers (UI helpers) vs Kendo UI widgets

In this chapter...

From client-side point of view, the vanilla HTML/JavaScript Kendo UI widgets and their ASP.NET MVC wrappers (UI helpers) represent the same thing and provide the same capabilities.
However, there are some implementation specifics to consider before choosing one product over the other.

The **UI for ASP.NET MVC** helpers (Kendo UI MVC wrappers) provide the following benefits:

- Create widgets with no HTML and JavaScript coding. JavaScript knowledge will still be required to use client-side methods and events.
- <a href="/kendo-ui/aspnet-mvc/helpers/grid/server-binding">Server-side data binding</a> and in some cases, server-side rendering.
- Use the <a href="/kendo-ui/aspnet-mvc/helpers/grid/ajax-binding">ToDataSourceResult</a> extension method to bind Kendo UI widgets to server-side collections and
perform data operations (paging, sorting, filtering, grouping).
- Integration with some ASP.NET MVC features, e.g. <a href="/kendo-ui/aspnet-mvc/helpers/menu/overview#security-trimming">security trimming</a> or
<a href="/kendo-ui/aspnet-mvc/globalization#localized-user-interface">localization with resource files</a> or <a href="/kendo-ui/aspnet-mvc/helpers/grid/editor-templates">editor templates</a>.
- Support for unobtrusive validation based on Data Annotation attributes.
- CRUD operations implementation simplicity - support for CRUD operations will require less coding, compared to when client-side Kendo UI is used.
In the latter case, you will have to repeat the model and data field configuration in the JavaScript code.
- Visual Studio intellisense for the server-side configuration syntax.
- <a href="/kendo-ui/aspnet-mvc/vs-integration/introduction">Visual Studio Extensions</a> for automatic creation of new Kendo UI ASP.NET MVC applications, or adding Kendo UI to existing projects,
or automatic updating of the Kendo UI version.
- Use <a href="/kendo-ui/aspnet-mvc/scaffolding">Scaffolding</a> to generate widget declarations and related controller action methods.

The vanilla HTML/JavaScript Kendo UI widgets (<strong>Kendo UI Professional</strong>) provide the following benefits:</p>

- Complete server platform independence. The widgets can be used with any web server and server framework (<strong>including ASP.NET MVC</strong>).
For example, you can build Single Page Applications with any RESTful back-end.
- Full control over the initialization scripts' placement - server wrappers render the widgets' initialization scripts right after the widget's HTML output.
Even if you use <a href="/kendo-ui/aspnet-mvc/fundamentals#deferred-initialization">deferred initialization</a>, the scripts will still be in the View.
When using plain (non-wrapper) Kendo UI widgets, you write the initialization scripts yourself and can move them to external script files.
- Integration with <a href="/kendo-ui/framework/mvvm/overview">MVVM</a>, <a href="/kendo-ui/AngularJS/introduction">AngularJS</a> and <a href="/kendo-ui/framework/spa/overview">Single Page Application</a> development patterns.
Server wrappers are not intended to be used with those.
- <a href="/kendo-ui/vs-intellisense">Visual Studio Intellisense</a> for the client-side API.

- Finally, there is no problem to use UI for ASP.NET MVC helpers and vanilla Kendo UI widgets **at the same time on the same page**, if a specific scenario requires that.
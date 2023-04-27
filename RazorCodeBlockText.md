# Razor Block
```razor
@page "/"
@using static LivingMessiah.Web.Services.Auth0
@using Page = LivingMessiah.Web.Links.Home
@using LivingMessiah.Web.SmartEnums

<PageTitle>@Page.PageTitle</PageTitle>

<HomeImage></HomeImage>


<div class="@MediaQuery.XsOrSm.DivClass">
	<SectionMain />
	<br />

	<SidebarButtons IsXsOrSm="true" />
	<ShabbatMeter />
	<div class="d-print-none">
		<SocialMediaButtons></SocialMediaButtons>
	</div>

</div>

<div class="card mb-3 border-warning">
	<div class="card-header">
		<h4 class="card-title text-center"><code>BibleBook.List.ToList()</code></h4>
	</div>
	<div class="card-body">
		<TableTemplate Items="BibleBook.List.ToList()">
			<TableHeader>
				<th class="text-info">Id</th>
				<th class="text-primary">Name</th>
				<th class="text-danger">Abrv</th>
				<th class="text-warning">Group</th>
			</TableHeader>
			<RowTemplate>
				<td>@context.Value</td>
				<td>@context.Name</td>
				<td>@context.Abrv</td>
				<td>@context.BookGroupEnum</td>
			</RowTemplate>
		</TableTemplate>
	</div>
	<p class="card-footer text-center"><code><small>BibleBook.List.Count</small></code>: @BibleBook.List.Count</p>
</div>

<div class="card mb-3 border-warning">
	<div class="card-header">
		<h4 class="card-title text-center"><code>item.Dump of BibleBook.List.OrderBy(o => o.Value)</code></h4>
	</div>

	<div class="card-body">

		@foreach (var item in BibleBook.List.OrderBy(o => o.Value))
		{
			<ul class="list-inline">
				<li class="list-inline-item">@item.Dump</li>
			</ul>
		}

	</div>
</div>

```

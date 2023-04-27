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
```

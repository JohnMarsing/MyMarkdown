# Dynamic Components


## DynamicComponent needed for BlzSrvFlxSrl


## `MasterList.razor.cs`
- Can I make the following `switch` statement into a 
- C:\Users\JohnM\source\repos\fluxor\BlazorServerFluxorSerilog\BlzSrvFlxSrl\BlzSrvFlxSrl\Features\SpecialEvents\MasterList.razor.cs
- [Working with the Blazor DynamicComponent](https://www.daveabrock.com/2021/04/08/blazor-dynamic-component/)
- [Search Blazor](https://www.bing.com/search?q=blazor+DynamicComponent+switch)

```csharp
using Microsoft.AspNetCore.Components;
using BlzSrvFlxSrl.Shared;
using Blazored.Modal.Services;

namespace BlzSrvFlxSrl.Features.SpecialEvents;

public partial class MasterList
{
	[Inject] public ILogger<MasterList>? Logger { get; set; }
	[Inject] private IState<State>? SpecialEventsState { get; set; }
	[Inject] public IDispatcher? Dispatcher { get; set; }

	[Parameter, EditorRequired] public bool IsXsOrSm { get; set; }

	[CascadingParameter] IModalService Modal { get; set; } = default!;

	private async Task OnCallBackEvent(CallBackEventArgs args)
	{		
		//await Task.Delay(0);
		int id = args.Id;

		string crudName;
		if (args.Crud == null)
		{
			Logger!.LogWarning(string.Format("inside: {0}; args.Crud: == null", nameof(MasterList) + "!" + nameof(OnCallBackEvent)));
			crudName = "Display";
		}
		else
		{
			crudName = args.Crud.Name;
		}

		switch (crudName)
		{
			case "Add":
				AddActionHandler();
				break;

			case "Edit":
				EditActionHandler(id);
				break;

			case "Display":
				DisplayActionHandler(id);
				break;

			case "Delete":
				await DeleteConfirmationHandler(id, "title");
				break;

			case "Repopulate":
				PopulateActionHandler();
				break;

			default:
				break;
		}
	}

	void AddActionHandler()
	{
		Logger!.LogDebug(string.Format("inside: {0}", nameof(MasterList) + "!" + nameof(AddActionHandler)));
		Dispatcher?.Dispatch(new Add_Action());
	}

	void PopulateActionHandler()
	{
		Logger!.LogDebug(string.Format("...{0}; Date Range: {1} to {2}"
			, nameof(MasterList) + "!" + nameof(PopulateActionHandler), SpecialEventsState!.Value.DateBegin, SpecialEventsState!.Value.DateEnd));
		Dispatcher?.Dispatch(new GetListWithDates_Action(
			SpecialEventsState!.Value.DateBegin, SpecialEventsState.Value.DateEnd));
	}

	void EditActionHandler(int id)
	{
		Logger!.LogDebug(string.Format("inside: {0}; id:{1}", nameof(MasterList) + "!" + nameof(EditActionHandler), id));
		Dispatcher?.Dispatch(new Get_Action(id, Enums.AddEditDisplay.Edit));
	}

	void DisplayActionHandler(int id)
	{
		Logger!.LogDebug(string.Format("inside: {0}; id:{1}", nameof(MasterList) + "!" + nameof(DisplayActionHandler), id));
		Dispatcher?.Dispatch(new Get_Action(id, Enums.AddEditDisplay.Display));
	}

	private async Task DeleteConfirmationHandler(int id, string title)
	{
		Logger!.LogDebug(string.Format("...{0}; id:{1}, title: {2}", nameof(MasterList) + "!" + nameof(DeleteConfirmationHandler), id, title));
		var parameters = new ModalParameters { { nameof(ConfirmDeleteModal.Message)
				, $"Special Event {title}" } };
		var modal = Modal.Show<ConfirmDeleteModal>("Confirmation Required", parameters);
		var result = await modal.Result;
		if (result.Confirmed)
		{
			Dispatcher?.Dispatch(new Delete_Action(id));
			Dispatcher?.Dispatch(new GetListWithDates_Action(
				SpecialEventsState!.Value.DateBegin, SpecialEventsState.Value.DateEnd));
		}
	}

}
```


## MhbWasmSQLite FavoriteVerses SearchToolbar

```razor
@using MhbWasmSQLite.Client.Features.FavoriteVerses.EnumsFV
@using MhbWasmSQLite.Client.Features.FavoriteVerses

<BlazoredTypeahead SearchMethod="SearchVerses"
									 @bind-Value="SelectedVerse"
									 EnableDropDown="true"
									 MinimumLength="2"
									 placeholder="Search favorite verse...">
	<SelectedTemplate Context="mycontext">
		@mycontext!.VerseNameAbrv
	</SelectedTemplate>
	<HelpTemplate>
		Please enter at least 2 characters to perform a search.
	</HelpTemplate>
	<ResultTemplate Context="mycontext">
		@mycontext.VerseName
	</ResultTemplate>
</BlazoredTypeahead>

@if (SelectedVerse != null)
{
	<div class="card mt-2">
		<div class="card-header h3 px-2">@SelectedVerse!.VerseName</div>

		<div class="card-body">
			<h6 class="card-title float-end fst-italic text-black-50">@SelectedVerse!.VerseDescription</h6>
		</div>

		<div class="card-body bg-warning">
			<p class="card-text fs-5 ">
				@((MarkupString)@SelectedVerse!.ConcatenatedVerses!)
			</p>
		</div>

		<div class="card-body">
			<h5 class="card-title">Comments</h5>
			<p class="card-text fs-6 ml-2">
				@((MarkupString)@SelectedVerse!.Commentary!)
			</p>

			@if (SelectedVerse!.HasExtraInfo)
			{
				<button type="button" class="btn btn-outline-info"
						@onclick="@(e => ButtonClick(SelectedVerse))">
					Details
				</button>

				@if (selectedType is not null)
				{
					if (SelectedVerse!.BegId == SelectedVerse!.BegId)
					{
						<div class="border border-primary my-3 p-2">
							<DynamicComponent Type="@selectedType" />
						</div>
					}
				}
			}
		</div>
	</div>
}
```

```csharp
using MhbWasmSQLite.Client.Components.BookChapterToolbar.Enums;
using MhbWasmSQLite.Client.Features.FavoriteVerses.EnumsFV;
using Microsoft.AspNetCore.Components;

namespace MhbWasmSQLite.Client.Features.FavoriteVerses.SearchToolbar;

public partial class Typeahead 
{
	protected override void OnInitialized()
	{
		base.OnInitializedAsync();
	}

	private EnumsFV.Verses? SelectedVerse;

	private async Task<IEnumerable<EnumsFV.Verses>> SearchVerses(string searchText)
	{
		return await Task.FromResult(EnumsFV.Verses.List
			.Where(x => x.VerseNameAbrv.ToLower().Contains(searchText.ToLower()))
			.OrderBy(o => o.Value));
	}

	private Type? selectedType;
	private void ButtonClick(Verses verse)
	{
		selectedType = verse.Name?.ToString()?.Length > 0
			? Type.GetType($"MhbWasmSQLite.Client.Features.FavoriteVerses.DetailContent.{verse.Name}")
			: null;
	}

}
```



## MS Doc Example
```razor
@page "/dynamiccomponent-example-3"

<h1><code>DynamicComponent</code> Component Example 3</h1>

<p>
  <label>
    Select your transport:
    <select @onchange="OnDropdownChange">
      <option value="">Select a value</option>
      <option value="@nameof(RocketLab2)">Rocket Lab</option>
      <option value="@nameof(SpaceX2)">SpaceX</option>
      <option value="@nameof(UnitedLaunchAlliance2)">ULA</option>
      <option value="@nameof(VirginGalactic2)">Virgin Galactic</option>
    </select>
  </label>
</p>

@if (selectedType is not null)
{
  <div class="border border-primary my-1 p-1">
    <DynamicComponent Type="@selectedType"
                    Parameters="@Components[selectedType.Name].Parameters" />
  </div>
}

<p>
  msg: @message
</p>

@code {
  private Type? selectedType;
  private string? message;

  private Dictionary<string, ComponentMetadata> Components
  {
    get
    {
      return new Dictionary<string, ComponentMetadata>()
      {
        {
          "RocketLab2",
          new ComponentMetadata
          {
            Name = "Rocket Lab",
            Parameters =
              new()
              {
                  {
                      "OnClickCallback",
                      EventCallback.Factory.Create<MouseEventArgs>(
                          this, ShowDTMessage)
                  }
              }
          }
        },
        {
            "VirginGalactic2",
            new ComponentMetadata
            {
                Name = "Virgin Galactic",
                Parameters =
                    new()
                    {
                        {
                            "OnClickCallback",
                            EventCallback.Factory.Create<MouseEventArgs>(
                                this, ShowDTMessage)
                        }
                    }
            }
        },
        {
            "UnitedLaunchAlliance2",
            new ComponentMetadata
            {
                Name = "ULA",
                Parameters =
                    new()
                    {
                        {
                            "OnClickCallback",
                            EventCallback.Factory.Create<MouseEventArgs>(
                                this, ShowDTMessage)
                        }
                    }
            }
        },
        {
            "SpaceX2",
            new ComponentMetadata
            {
                Name = "SpaceX",
                Parameters =
                    new()
                    {
                        {
                            "OnClickCallback",
                            EventCallback.Factory.Create<MouseEventArgs>(
                                this, ShowDTMessage)
                        }
                    }
            }
        }
      };
    }
  }

  private void OnDropdownChange(ChangeEventArgs e)
  {
    selectedType = e.Value?.ToString()?.Length > 0 ?
        Type.GetType($"MhbWasmSQLite.Client.Shared.{e.Value}") : null;
  }

  private void ShowDTMessage(MouseEventArgs e) =>
      message = $"The current DT is: {DateTime.Now}.";
}
```

---


## Pass parameters
- [docs](https://learn.microsoft.com/en-us/aspnet/core/blazor/components/dynamiccomponent?view=aspnetcore-7.0#pass-parameters)

If dynamically-rendered components have component parameters, pass them into the DynamicComponent as an IDictionary<string, object>. The string is the name of the parameter, and the object is the parameter's value.

The following example configures a component metadata object (ComponentMetadata) to s


## Avoid catch-all parameters
Avoid the use of catch-all parameters. If catch-all parameters are used, every explicit parameter on DynamicComponent effectively is a reserved word that you can't pass to a dynamic child. Any new parameters passed to DynamicComponent are a breaking change, as they start shadowing child component parameters that happen to have the same name. It's unlikely that the caller always knows a fixed set of parameter names to pass to all possible dynamic children.
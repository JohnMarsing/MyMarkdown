# LoadingComponent and Status
- Use this to wrap a component that makes an `async` call and might List that is the source of the data may be null initially
- Manages turning the spinner `TurnSpinnerOff` on or off.
- Uses a `RenderFragment` called `ChildContent` the content of your UI Component
- Uses anoter `RenderFragment` called `LoadingTemplate` is the object (e.g. data list) that that LoadingComponent needs to check whether it 'null` or not.
- Locations Shared\

## LoadingComponent.razor
- 
```razor
@if (IsLoading)
{
  if (LoadingTemplate != null)
  {
    @LoadingTemplate
  }
  else
  {
    if (TurnSpinnerOff == false)
    {
      <div class="spinner-border"></div>
      <span style="display: inline-block; vertical-align: super">@LoadingText</span>
    }
  }
}
else
{
  @ChildContent
}
```

```csharp
@code{
  [Parameter] public bool IsLoading { get; set; }
  [Parameter]  public string LoadingText { get; set; } = "Loading...";
  [Parameter]  public bool TurnSpinnerOff { get; set; } = false;
  [Parameter]  public RenderFragment? LoadingTemplate { get; set; }
  [Parameter]  public RenderFragment? ChildContent { get; set; }
}
```

## Example usage: `PsalmsAndProverbsList`

#### PsalmsAndProverbsList.razor
```razor
<LoadingComponent IsLoading="PsalmsAndProverbsList==null" TurnSpinnerOff="@TurnSpinnerOff">
...
</LoadingComponent>
```

#### PsalmsAndProverbsList.razor.cs
- `TurnSpinnerOff` is set in the `try` `catch` `finally` 
- This code also shows `Logger` and `Toast`
  - Bonus: Enhanced error messsage using  **Links** `page`


```csharp
  using Page = LivingMessiah.Web.Links.PsalmsAndProverbs;

//...
  private const string Message = $"Failed to load page {Page.Index}, Class!Method:{nameof(PsalmsAndProverbs)}!{nameof(OnInitializedAsync)}";

  protected bool TurnSpinnerOff;

  protected override async Task OnInitializedAsync()
  {
    try
    {
      TurnSpinnerOff = false;
      PsalmsAndProverbsList = await svc!.GetPsalmsAndProverbsList();
    }
    catch (Exception ex)
    {
      Logger!.LogWarning(ex, Message);
      Toast!.ShowError(Message);
    }
    finally
    {
      TurnSpinnerOff = true;
    }
  }
```

# `Status` Enum
- An alternative to `LoadingComponent` is `Status`
- Better name might be `StatusUI` or `UiStatus`

### Psalms\Index.razor
```razor
@switch (_status)
{
  case Status.Loading:
    <p class="text-warning"><em>Loading...</em></p>
    break;

  case Status.Loaded:
    <table class="table">
...
  case Status.Error:
    <p class="text-danger"><em>Could not load because of an Error</em></p>
    <p class="p-2">@_msg</p>
    break;

  default:
    break;
}    
```


### Psalms\Index.razor.cs
```csharp
public partial class Index
{
  protected Status _status;
  protected string _msg = string.Empty;

  protected override async Task OnInitializedAsync()
  {
    try
    {
      _status = Status.Loading;
      PsalmsList = await db!.GetPsalms();
      _status = Status.Loaded;
    }
    catch (Exception ex)
    {
      _status = Status.Error;
      _msg = $"Error calling {nameof(db.GetPsalms)}"; 
      Logger!.LogError(ex, $"Failed to load page {nameof(Index)}");
    }
  }

```


### Psalms\Status.cs
```csharp
public enum Status
{
  Loading,
  Loaded,
  Error
}
```
 

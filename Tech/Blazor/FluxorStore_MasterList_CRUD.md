- [Special Events MasterList / Detail component](#special-events-masterlist--detail-component)
  - [Solution Explorer](#solution-explorer)
  - [Screen shot](#screen-shot)
- [Components](#components)
- [`Index.razor.cs`](#indexrazorcs)
  - [`Index.razor`](#indexrazor)
    - [contained components](#contained-components)
  - [`Index.razor`](#indexrazor-1)
- [1. `PageHeader.razor.cs`](#1-pageheaderrazorcs)
- [2. MasterList Table (MdOrLgOrXL)](#2-masterlist-table-mdorlgorxl)
  - [MasterList ActionButtons](#masterlist-actionbuttons)
  - [`MasterList` XsOrSm Grid](#masterlist-xsorsm-grid)
- [3. ShowMasterIndex](#3-showmasterindex)
  - [`ShowMasterIndex.cs`](#showmasterindexcs)
- [4 Form](#4-form)
- [5 `DisplayCard`](#5-displaycard)
  - [DisplayCard razor](#displaycard-razor)
    - [DisplayCard code behind](#displaycard-code-behind)
  - [Delete Modal](#delete-modal)
  - [Add / Edit / Display](#add--edit--display)
- [FormVM](#formvm)
  - [`FormVM.cs`](#formvmcs)
  - [`SaveCancelConstants`](#savecancelconstants)
- [6 `ToasterSpecialEvents.razor.cs`](#6-toasterspecialeventsrazorcs)
- [`FluxorStore.cs`](#fluxorstorecs)
- [| Part 			  | Notes](#-part----notes)
  - [1. Actions](#1-actions)
    - [Actions as viewed in Solution Explorer](#actions-as-viewed-in-solution-explorer)
    - [the code](#the-code)
  - [`GetList()`](#getlist)
  - [External References...](#external-references)
  - [`GetItem()`](#getitem)
  - [`Submit()`](#submit)
  - [`Delete()`](#delete)
  - [`MasterList!ReturnedCrud()`](#masterlistreturnedcrud)
      - [CallBack example](#callback-example)
  - [2. State](#2-state)
    - [State as viewed in Solution Explorer](#state-as-viewed-in-solution-explorer)
  - [3. Feature](#3-feature)
  - [4. Reducers](#4-reducers)
    - [Reducers as viewed in Solution Explorer](#reducers-as-viewed-in-solution-explorer)
  - [5. Effects](#5-effects)
    - [Effects as viewed in Solution Explorer](#effects-as-viewed-in-solution-explorer)
- [Other Components](#other-components)
  - [`YouTubeButton.razor`](#youtubebuttonrazor)
- [Other NOT YET USED](#other-not-yet-used)
  - [`EditMarkdownVM.cs`](#editmarkdownvmcs)
- [Data](#data)
- [Enums](#enums)

# Special Events MasterList / Detail component
- This uses Vertical Slice Architecture and the folder that these component lives in is **SpecialEvents**
- On the [Living Messiah](https://LivingMessiah.com/) website there are normal weekly events that occur every week and periodically an extraordinary event occurs which needs to be shown on the website in a timely fashion.  The Special Event component keeps track of this types of events, see [Living Messiah](https://LivingMessiah.com/UpcomingEvents)

You could in theory, fairly easily I would think, swap out the MasterList and put in it's place a `BlazoredTypeahead` component or, instead of the TableTemplate in MasterList, use a Grid component

## Solution Explorer
- Inspired by Vertical Architecture

![alt text](FluxorStore_MasterList_CRUD/00-Solution-Explorer.jpg "Title")


## Screen shot

Index page showing a html Table
![alt text](FluxorStore_MasterList_CRUD/80-Special-Events-Index-TableTemplate.jpg "Title")

Index page showing a bootstrap card
![alt text](FluxorStore_MasterList_CRUD/81-Special-Events-Index-Card.jpg "Title")

Index page transitioning into a EditForm (Edit mode)
![alt text](FluxorStore_MasterList_CRUD/82-Special-Events-Index-Form-Edit.jpg "Title")



# Components
Here's a list of the major components found under the SpecialEvents :file_folder:

# `Index.razor.cs`

Screen shot showing where `<PageHeader />` and `<MasterList />` are located
![alt text](FluxorStore_MasterList_CRUD/02-MasterList-Table-screen-shot-with-razor-code.jpg "Title")


Transition from...
![alt text](FluxorStore_MasterList_CRUD/51-Transition-from-Index-to-Detail.jpg "Title")

Transition to...
![alt text](FluxorStore_MasterList_CRUD/52-Transition-from-Index-to-Detail.jpg "Title")

## `Index.razor`
like all `Index.razor` files this is no different in that it contains and orchestrates other components. It passes down to them Media Query info (`IsXsOrSm`) and based on the value of the `VisibleComponent` determines what component to show.

### contained components
 1. `PageHeader` 
 2. `MasterList`
 3. `ShowMasterIndex`
 4. `Form`
 5. `DisplayCard`
 6. `ToasterSpecialEvennts`

## `Index.razor` 
Razor markup highlighting how the index is laid out, how it's using `State` to show / hide different components
![alt text](FluxorStore_MasterList_CRUD/01-Index-page-SpecialEvents-razor-and-code-behind.jpg "Title")


# 1. `PageHeader.razor.cs`
PageHeader shows the top section of the Index page along with some dynamic content that shown on the right. It contains the `<PageTitle />` component which is found in `Index.razor` components that have the `@page` directive.  

The markup that's on the right side is dynamic based on the values found in `PageHeaderVM`. `PageHeaderVM` gets changed based on the Fluxor actions `Set_PageHeader_For_Index_Action` or `Set_PageHeader_For_Detail_Action`.  

The **Index** action is the same and populates the VM by calling `Constants.GetPageHeaderForIndexVM()`  The **Detail** is dependent on the results of **MasterList!ReturnedCrud**

![alt text](FluxorStore_MasterList_CRUD/PageHeader.jpg "Title")

# 2. MasterList Table (MdOrLgOrXL)
- MdOrLgOrXL i.e. `IsXsOrSm="false"`

ToDo: add <TableTemplate>

## MasterList ActionButtons
![alt text](FluxorStore_MasterList_CRUD/03-MasterList-and-ActionButtons-and-Crud-enum.jpg "Title")

## `MasterList` XsOrSm Grid
- `IsXsOrSm="true"`

![alt text](FluxorStore_MasterList_CRUD/20-MasterList-XsOrSm-card-screen-shot.jpg "Title")


# 3. ShowMasterIndex

## `ShowMasterIndex.cs`

This button is found in `Index.razor` and is a toggle that becomes visible only when the `MasterList` is NOT shown. This occurs when the user selects one of the rows of the master list by clicking the Add/Edit/Display button

A `Dispatcher!.Dispatch` to `Set_PageHeader_For_Index_Action` is called to make the index visible
![alt text](FluxorStore_MasterList_CRUD/91-ShowMasterIndex.jpg "Title")



# 4 Form
A collage of the `Form.razor` component
![alt text](FluxorStore_MasterList_CRUD/21-Form-Edit-Mode.jpg "Title")

# 5 `DisplayCard`
![alt text](FluxorStore_MasterList_CRUD/23-DisplayCard.jpg "Title")

## DisplayCard razor
![alt text](FluxorStore_MasterList_CRUD/24-DisplayCard-razor.jpg "Title")

### DisplayCard code behind
![alt text](FluxorStore_MasterList_CRUD/25-DisplayCard-code-behind.jpg "Title")

## Delete Modal
![alt text](FluxorStore_MasterList_CRUD/60-Delete-Modal-Screen-Shot.jpg "Title")

## Add / Edit / Display
![alt text](FluxorStore_MasterList_CRUD/70-AddEditDisplay-and-VisibleComponent-Enums.jpg "Title")

# FormVM

## `FormVM.cs`
![alt text](FluxorStore_MasterList_CRUD/71-FormVM.jpg "Title")

## `SaveCancelConstants`
![alt text](FluxorStore_MasterList_CRUD/72-SaveCancelConstants.jpg "Title")


# 6 `ToasterSpecialEvents.razor.cs`

This is a cool component because the toast related stuff is centralized and done though **dispataching** (I got this code from Eric King, [see](https://github.com/eric-king/BlazorWithFluxor)).  It's also done in context to **SpecialEvents**, so if I had another slice in my vertical architecture, it would have it's own component.  


Place this in the `Index.razor` of SpecialEvents
![alt text](FluxorStore_MasterList_CRUD/90-Index-page-ToasterSpecialEvents.jpg "Title")

ToConsider: If you wanted to, you could change the user's profile to set the degree of toast verbosity.

- the only thing in `ToasterSpecialEvents.razor` is...
  - `@inherits FluxorComponent`
  - the rest is code behind

```csharp
using Blazored.Toast.Services;
using Microsoft.AspNetCore.Components;

namespace BlzSrvFlxSrl.Features.SpecialEvents;

public partial class ToasterSpecialEvents
{
  [Inject] public IToastService? Toast { get; set; }
  protected override void OnInitialized()
  {
    //SubscribeToAction<Get_List_Success_Action>(Get_List_Success_Toast); // Too much chatter
    SubscribeToAction<Get_List_Warning_Action>(Get_List_Warning_Toast);
    SubscribeToAction<Get_List_Failure_Action>(Get_List_Failure_Toast);

    SubscribeToAction<Get_Item_Success_Action>(Get_Item_Success_Toast);
    SubscribeToAction<Get_Item_Warning_Action>(Get_Item_Warning_Toast);
    SubscribeToAction<Get_Item_Failure_Action>(Get_Item_Failure_Toast);

    SubscribeToAction<Submitted_Response_Success_Action>(Submitted_Response_Success_Toast);
    SubscribeToAction<Submitted_Response_Failure_Action>(Submitted_Response_Failure_Toast);

    SubscribeToAction<DeleteSuccess_Action>(DeleteSuccess_Toast);
    SubscribeToAction<DeleteFailure_Action>(DeleteFailure_Toast);

    //SubscribeToAction<PageHeader_Action>(PageHeader_Toast);
    //SubscribeToAction<SetDateRange_Action>(SetDateRange_Toast);
    base.OnInitialized();
  }

  //private void Get_List_Success_Toast(Get_List_Success_Action action) => Toast!.ShowInfo($"Got list of {action.SpecialEvents.Count} records");

  private void Get_List_Warning_Toast(Get_List_Warning_Action action) => Toast!.ShowWarning($"No records found");
  private void Get_List_Failure_Toast(Get_List_Failure_Action action) => Toast!.ShowError($"{action.ErrorMessage}");

  private void Get_Item_Success_Toast(Get_Item_Success_Action action) => Toast!.ShowInfo($"Got {action.FormVM!.Title!}");
  private void Get_Item_Warning_Toast(Get_Item_Warning_Action action) => Toast!.ShowWarning($"{action.WarningMessage}");
  private void Get_Item_Failure_Toast(Get_Item_Failure_Action action) => Toast!.ShowError($"{action.ErrorMessage}");

  private void Submitted_Response_Success_Toast(Submitted_Response_Success_Action action) => Toast!.ShowSuccess($"{action.SuccessMessage}");
  private void Submitted_Response_Failure_Toast(Submitted_Response_Failure_Action action) => Toast!.ShowError($"Form submit error; ErrorMessage: {action.ErrorMessage}");
  private void DeleteSuccess_Toast(DeleteSuccess_Action action) => Toast!.ShowSuccess(action.SuccessMessage);
  private void DeleteFailure_Toast(DeleteFailure_Action action) => Toast!.ShowError($"Failed to delete Special Events; {action.ErrorMessage}");

  //private void PageHeader_Toast(PageHeader_Action action) => Toast!.ShowInfo($"PageHeader Title: {action.Title}");
  //private void SetDateRange_Toast(SetDateRange_Action action) => Toast!.ShowInfo($"Selected Date Range: {action.DateBegin.ToString("yyyy-MM-dd")} to {action.DateEnd.ToString("yyyy-MM-dd")}");
}
```


# `FluxorStore.cs`
The Fluxor Store consists 5 parts

 #    | Part 			  | Notes                              			
 :--: |:------------|:----------------------------------------
 1 | **Actions** 		| List of c#  `record`s 
 2 | **State** 			| a c#  `record` that has all the members which define the state at any given time.
 3 | **Feature** 		| Initializes `State` via overriding `GetInitialState()` (and `GetName()` whatever that does)
 4 | **Reducers** 	| a class of `[ReducerMethod]` `static` methods that changes the state using `with`. These are what get **`Dispatch`ed**
 5 | **Effects** 		| a class of [EffectMethod] methods that are external and, effectively, replace the need for **Services**

## 1. Actions

### Actions as viewed in Solution Explorer
![alt text](FluxorStore_MasterList_CRUD/00-Solution-Explorer-FluxorStore-Actions.jpg "Title")


### the code
```csharp
// 1.1 GetList() actions
public record Get_List_Success_Action(List<Data.vwSpecialEvent> SpecialEvents);
public record Get_List_Success_Action(List<SpecialEvent> SpecialEvents);  
public record Get_List_Warning_Action(string WarningMessage);             
public record Get_List_Failure_Action(string ErrorMessage);               

// 1.2 GetItem() actions
public record Get_Item_Action(int Id, Enums.FormMode? FormMode);               

public record Get_Item_Success_Action(FormVM? FormVM); 
  public record Edit_Action(int Id);    // FormMode == FormMode.Edit
  public record Display_Action(int Id); // FormMode != FormMode.Edit

public record Get_Item_Warning_Action(string WarningMessage);     
public record Get_Item_Failure_Action(string ErrorMessage);       

// 1.3 Actions related to Form Submission 
public record Submitting_Request_Action(FormVM FormVM, Enums.FormMode? FormMode);  
public record Submitted_Response_Success_Action(string SuccessMessage); 
public record Submitted_Response_Failure_Action(string ErrorMessage);   

// 1.4 Actions related to the MasterList (specifically the ActionButtons!EventCallback)
public record Add_Action();           

// 1.5 Delete() actions
public record Delete_Action(int Id);
public record DeleteSuccess_Action(string SuccessMessage);  
public record DeleteFailure_Action(string ErrorMessage);    


// 1.6 PageHeader actions
public record Set_PageHeader_For_Index_Action(PageHeaderVM PageHeaderVM);
public record Set_PageHeader_For_Detail_Action(
        string Title, string Icon, string Color, int Id);

```


**1.1 Get_List_...**

## `GetList()` 
![alt text](FluxorStore_MasterList_CRUD/40-Effect-Get-List.jpg "Title")

## External References...

- Form.razor.cs
  - HandleValidSubmit 

- MasterList.razor.cs
  - OnInitialized if (SpecialEventsState!.Value.SpecialEventList is null) i.e.first time
  - ReturnedCrud case nameof(Enums.Crud.Delete)
  - ReturnedCrud case Repopulate


**1.2 Get_Item_...**

**Get_Item_Action**


## `GetItem()` 
![alt text](FluxorStore_MasterList_CRUD/41-Effect-Get-Item.jpg "Title")

**1.3 Actions related to Form Submission**
## `Submit()` 
![alt text](FluxorStore_MasterList_CRUD/42-Effect-Submit.jpg "Title")



**1.5 Actions related deletions**
## `Delete()` 
![alt text](FluxorStore_MasterList_CRUD/43-Effect-Delete.jpg "Title")


**1.6 PageHeader actions**

## `MasterList!ReturnedCrud()` 

#### CallBack example
```razor
<ActionButtons 
  OnCrudActionSelected="@ReturnedCrud" 
	ParmCrud="Enums.Crud.Add" 
	IsXsOrSm="@IsXsOrSm" 
	Id="0" />
```

```csharp
  private async Task ReturnedCrud(CrudAndIdArgs args)
  {
    switch (args.Crud.Name)
    {
      case nameof(Enums.Crud.Add):
        Dispatcher!.Dispatch(new Add_Action());
        Dispatcher!.Dispatch(new Set_PageHeader_For_Detail_Action(args.Crud.Name, args.Crud!.Icon, args.Crud!.Color, args.Id));
        break;
// ...        
```

Expanded code from above showing `Set_PageHeader_For_Detail_Action` being dispatched for **Add**, **Edit** and **Display** scenarios
![alt text](FluxorStore_MasterList_CRUD/50-Set_PageHeader_For_Detail_Action.jpg "Title")


## 2. State

### State as viewed in Solution Explorer
![alt text](FluxorStore_MasterList_CRUD/00-Solution-Explorer-FluxorStore-State.jpg "Title")


The code
```csharp

public record State
{
  public DateTimeOffset? DateBegin { get; init; }
  public DateTimeOffset? DateEnd { get; init; }
  public Enums.VisibleComponent? VisibleComponent { get; init; }
  public Enums.FormMode? FormMode { get; init; }
  public string? SuccessMessage { get; init; }
  public string? WarningMessage { get; init; }
  public string? ErrorMessage { get; init; }
  public FormVM? FormVM { get; init; }
  public List<Data.vwSpecialEvent>? SpecialEventList { get; init; }
  public PageHeaderVM? PageHeaderVM { get; init; }
}
```

Comments about `DateBegin` and `DateEnd`
```csharp
/*

ToDo: DateBegin and DateEnd could be... 
  1) simplified as a DateRange object
  2) be initially set in an injected service found in Index.razor.
      the advantage being that it eliminates the parameters of `Get_List_Action(State!.Value.DateBegin, State.Value.DateEnd)`

*/
```


## 3. Feature  
```csharp
public class FeatureImplementation : Feature<State>
{
  public override string GetName() => "SpecialEvents"; // You have to put something here I think ???

  protected override State GetInitialState()
  {
    return new State
    {
      DateBegin = DateTime.Parse(Constants.DateRange.Start),
      DateEnd = DateTime.Parse(Constants.DateRange.End),
      FormMode = null,
      VisibleComponent = Enums.VisibleComponent.MasterList,
      SuccessMessage = string.Empty,
      WarningMessage = string.Empty,
      ErrorMessage = string.Empty,
      PageHeaderVM = Constants.GetPageHeaderForIndexVM(),
      FormVM = new FormVM()
    };
  }
}
```

## 4. Reducers

### Reducers as viewed in Solution Explorer
![alt text](FluxorStore_MasterList_CRUD/00-Solution-Explorer-FluxorStore-Reducers.jpg "Title")

A template
```csharp
`public static class Reducers`
```

Some pseudo code of the reducers

**Get_List_Success_Action**
```csharp
VisibleComponent = Enums.VisibleComponent.MasterList,
WarningMessage = string.Empty,
ErrorMessage = string.Empty,
SpecialEventList = action.SpecialEvents
```

**Get_List_Warning_Action**
```csharp
VisibleComponent = Enums.VisibleComponent.MasterList,
WarningMessage = action.WarningMessage
```

**Get_List_Failure_Action action**
```csharp
ErrorMessage = action.ErrorMessage
```

**Get_Item_Success_Action action**
FormVM = action.FormVM

**Get_Item_Failure_Action**
```csharp
VisibleComponent = Enums.VisibleComponent.MasterList,
ErrorMessage = action.ErrorMessage
```

**Submitting_Request_Action**
```csharp
FormMode = action.FormMode
```

**On_Submitted_Response_Success**
```csharp
VisibleComponent = Enums.VisibleComponent.MasterList,
SuccessMessage = ""
```

**Submitted_Response_Failure_Action**
```csharp
ErrorMessage = action.ErrorMessage,
VisibleComponent = Enums.VisibleComponent.MasterList
```

**Add_Action**
```csharp
VisibleComponent = Enums.VisibleComponent.AddEditForm,
FormMode = Enums.FormMode.Add,
FormVM = new FormVM()
```

**Edit_Action**
```csharp
VisibleComponent = Enums.VisibleComponent.AddEditForm,
FormMode = Enums.FormMode.Edit,
```

**Display_Action**
```csharp
VisibleComponent = Enums.VisibleComponent.DisplayCard
```

**Delete_Action**
```csharp
VisibleComponent = Enums.VisibleComponent.MasterList,
```

**Set_PageHeader_For_Index_Action**
```csharp
VisibleComponent = Enums.VisibleComponent.MasterList,
PageHeaderVM = Constants.GetPageHeaderForIndexVM()
```

**Set_PageHeader_For_Detail_Action**
```csharp
PageHeaderVM = new PageHeaderVM 
  { Title = action.Title, Icon = action.con, Color = action.Color, Id = action.Id }
```

## 5. Effects

### Effects as viewed in Solution Explorer

![alt text](FluxorStore_MasterList_CRUD/00-Solution-Explorer-FluxorStore-Effects.jpg "Title")

# Other Components


## `YouTubeButton.razor`
This component button used by the `MasterList.razor`.  It's a `<div class="card-footer"></div>` when the media query is `IsXsOrSm` and  a `<td></td>` in the `TableTemplate` when not `IsXsOrSm`.


![alt text](FluxorStore_MasterList_CRUD/73-YouTube-Button.jpg "Title")



# Other NOT YET USED
## `EditMarkdownVM.cs`
This is allows the user to edit the description field which is a markdown enabled control

- ToDo: Implement this
- ToDo: Need to convert to **FluentValidation**

```csharp
public class EditMarkdownVM
{
    [Required]
    [Key]
    public int Id { get; set; }

    public string? Title { get; set; }

    //[DataType(DataType.MultilineText)]
    //[Required(ErrorMessage = "A description is required")]
    //[MinLength(10, ErrorMessage = "Please enter at least 10 characters based on HTML.")]
    //[MaxLength(3000, ErrorMessage = "Please enter no more than at least 3,000... were not writing a novel.")]
    [DisplayName("Markdown Description")]
    public string? Description { get; set; }
}
```

# Data 
- `Repository.cs`, `vwSpecialEvent.cs` 
- Not Shown, see code


# Enums
- `Crud.cs`, `FormMode.cs`, `SpecialEventType.cs` and `VisibleComponent`
-  Not Shown, see code


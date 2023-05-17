
# Special Events

# FluxStore.cs

## 1. Actions
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
![alt text](FluxorStore_MasterList_CRUD/50-Set_PageHeader_For_Detail_Action.jpg "Title")

Transition from...
![alt text](FluxorStore_MasterList_CRUD/51-Transition-from-Index-to-Detail.jpg "Title")

Transition to...
![alt text](FluxorStore_MasterList_CRUD/52-Transition-from-Index-to-Detail.jpg "Title")


## 2. State
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

## 3. Feature  
```csharp
public class FeatureImplementation : Feature<State>
{
	public override string GetName() => "SpecialEvents"; // You have to put something here???

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

# More Stuff

## Index
![alt text](FluxorStore_MasterList_CRUD/01-Index-page-SpecialEvents-razor-and-code-behind.jpg "Title")

## MasterList Table (MdOrLgOrXL)
![alt text](FluxorStore_MasterList_CRUD/02-MasterList-Table-screen-shot-with-razor-code.jpg "Title")

## MasterList ActionButtons
![alt text](FluxorStore_MasterList_CRUD/03-MasterList-and-ActionButtons-and-Crud-enum.jpg "Title")

## MasterList XsOrSm Grid
![alt text](FluxorStore_MasterList_CRUD/20-MasterList-XsOrSm-card-screen-shot.jpg "Title")

## Form
![alt text](FluxorStore_MasterList_CRUD/21-Form-Edit-Mode.jpg "Title")

## DisplayCard
![alt text](FluxorStore_MasterList_CRUD/23-DisplayCard.jpg "Title")

### DisplayCard razor
![alt text](FluxorStore_MasterList_CRUD/24-DisplayCard-razor.jpg "Title")

### DisplayCard code behind
![alt text](FluxorStore_MasterList_CRUD/25-DisplayCard-code-behind.jpg "Title")

## Delete Modal
![alt text](FluxorStore_MasterList_CRUD/60-Delete-Modal-Screen-Shot.jpg "Title")

## Add / Edit / Display
![alt text](FluxorStore_MasterList_CRUD/70-AddEditDisplay-and-VisibleComponent-Enums.jpg "Title")

# Other

## `FormVM.cs`
![alt text](FluxorStore_MasterList_CRUD/71-FormVM.jpg "Title")

## `SaveCancelConstants`
![alt text](FluxorStore_MasterList_CRUD/72-SaveCancelConstants.jpg "Title")

## `YouTubeButton.razor` Component
![alt text](FluxorStore_MasterList_CRUD/73-YouTube-Button.jpg "Title")
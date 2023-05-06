# Tuples
Useful for returning more than one parameter

##  `RegistrationVM.cs`
```csharp
public class RegistrationVM
{
  public int AttendanceBitwise { get; set; }
  public DateTime[] AttendanceDateList { get; set; }
  public DateTime[] AttendanceDateList2ndMonth { get; set; }
)
```

## `RegistrationEditService.cs`

- LivingMessiahBlazor\LivingMessiah.Web\Pages\Sukkot\Components\

```csharp
  RegistrationVM VM = new();
  var tuple = Helper.GetAttendanceDatesArray(VM.AttendanceBitwise);
  VM.AttendanceDateList = tuple.week1;
  VM.AttendanceDateList2ndMonth = tuple.week2;
```

## `Helper.cs`
```csharp
using System;
using System.Linq;

namespace LivingMessiah.Web.Pages.Sukkot.Enums;

public class Helper
{
  public static (DateTime[] week1, DateTime[]? week2) GetAttendanceDatesArray(int attendanceBitwise)
  {
    if (!Enums.DateRangeType.Attendance.HasSecondMonth)
    {
      return (AttendanceDate.FromValue(attendanceBitwise).Select(s => s.Date).ToArray(), null);
    }
    else
    {
      return
        (AttendanceDate.FromValue(attendanceBitwise).Where(w => w.Week == 1).Select(s => s.Date).ToArray(),
         AttendanceDate.FromValue(attendanceBitwise).Where(w => w.Week == 2).Select(s => s.Date).ToArray());
    }
  }
}

```

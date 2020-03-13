---
versionFrom: 8.0.0
---

# DateTime

`Alias: Umbraco.DateTime`

`Returns: DateTime`

Displays a calendar UI for selecting dates which are saved as a DateTime value.

## Data Type Definition Example

![Data Type Definition Example](images/date-time-v8.png)

There are two settings available for manipulating the DateTime property.


One is to set a format. By default the format of the date in the Umbraco backoffice will be `YYYY-MM-DD HH:mm:ss`, but you can change this to something else. See [MomentJS.com](https://momentjs.com/) for the supported formats.

The second setting is "Offset time". When enabling this setting the displayed time will be offset with the servers timezone. This can be useful in cases where an editor is in a different timezone than the hosted server.

## Content Example

![Content Example](images/date-picker-v8.png)

## MVC View Example - displays a datetime

### With Modelsbuilder

```csharp
@Model.DatePicker
```

### Without Modelsbuilder

```csharp
@Model.Value("datePicker")
```

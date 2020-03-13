---
versionFrom: 8.0.0
---

# Numeric

`Alias: Umbraco.Integer`

`Returns: Integer`

Numeric is an HTML input control for entering numbers. Since it's a standard HTML element the options and behaviour are all controlled by the browser and therefore is beyond the control of Umbraco.

## Data Type Definition Example

![Numeric Data Type Definition](images/numeric-datatype.png)

### Minimum
This allows you to set up a minimum value. If you will always need a minimum value of 10 this is where you set it up and whenever you use the datatype the value will always start at 10. It's not possible to change the value to anything lower than 10. Only higher values will be accepted.

### Step Size
This allows you to control by how much value should be allowed to increase/decrease when clicking the up/down arrows. If you try to enter a value that does not match with the step setting then it will not be accepted.

### Maximum
This allows you to set up a maximum value. If you will always need a maximum value of 100 this is where you set it up. It's not possible to change the value to anything higher than 100. Only lower values will be accepted.

## Settings

## Content Example

![Numeric Content Definition](images/numeric-content.png)


## MVC View Examples

### Rendering the output casting to an int
By casting the output as an int it's possible for you to do mathematical operations with the value.

```csharp
@{
    int students = Model.HasValue("students") ? Model.Value<int>("students") : 0;
    int teachers = Model.HasValue("teachers") ? Model.Value<int>("teachers") : 0;
    int totalTravellers = students + teachers;

    <p>@totalTravellers</p>
}
```

### Rendering the output casting to a string
You can also render the output by casting it to a string, which means you will not be able to do mathematical operations

```csharp
@{
    if(Model.HasValue("students")){
        <p>@(Model.Value<string>("students"))</p>
    }
}
```

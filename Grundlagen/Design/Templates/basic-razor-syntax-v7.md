---
versionFrom: 7.0.0
---

# Basic Razor Syntax

_Shows how to perform common logical tasks in Razor like if/else, foreach loops, switch statements and using the @ character to separate code and markup._

## The @ symbol

The @ symbol is used in Razor initiate code, and tell the compiler where to start interpreting code, instead of return the contents of the file as text. Using a single character for this separation, results in cleaner, compact code which is easier to read.

```csharp
@*Writing a value inside a html element *@

<p>@Model.Content.Name</p>

@*Inside an attribute*@
<a href="@Model.Content.Url">@Model.Content.Name</a>

@*Using it to start logical structures*@
@if(somethingIsTrue){
    <p>Write stuff</p>
}

@foreach(var item in Model.Content.Children){
    <li>@item.Name</li>
}
```

## Embedding comments in razor
Commenting your code is important, use comments to explain what the code does. `@* *@` indicates a comment, which will not be visible in the rendered output.

```csharp
@*Here we check if the name is equal to foobar*@
@if(Model.Content.Name == "foobar"){
    @foreach(var child in Model.Content.Children){
        @* here we write stuff for each child page *@
        <p>write stuff</p>
    }
}
```

## If/else
If/else statements performs one task if a condition is true, and another is the condition is not true

```csharp
@if(Model.Name == "home"){
    <p>This is the homepage!</p>
}

@if(Model.NodeTypeAlias == "TextPage"){
    <p>this is a textpage</p>
} else{
    <p>this ia NOT a textpage</p>
}
```

## Foreach loops
A foreach loop goes through a collection of items, typically a collection of pages and performs an action for each item

```csharp
@foreach(var item in Model.Content.Children){
    <p>The item name is: @Item.Name</p>
}
```

## Switch block
A Switch block is used when testing a large number of conditions

```csharp
@switch(Model.Content.WeekDay){
case "Monday":
    "<p>It is Monday</p>;
    break;
case "Tuesday":
    "<p>It is Tuesday</p>";
    break;
case "Wednesday":
    "<p>It is Wednesday</p>";
    break;
default:
    "<p>It's some day of the week</p>"
    break;
}
```

### More information
- [More examples](../../../Reference/Templating/Mvc/examples)
- [Querying](../../../Reference/Querying)

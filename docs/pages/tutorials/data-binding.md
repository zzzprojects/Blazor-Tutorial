# Data Binding

In Blazor, you can bind data to both components and DOM elements using the `bind` attribute. 

 - Data binding is one of the most powerful features of software development technologies. 
 - It is the connection bridge between View and the business logic of the application. 

In Blazor, you can bind data in the following ways:

 - One-way Data Binding
 - Two-way Data Binding
 - Event Binding

## One-way Data Binding

In other frameworks such as Angular, one-way data binding is also known as interpolation. 

 - In one-way binding, we need to pass property or variable name along with `@`, i.e., @Title (here Title is either the property or variable). 
 - In the following example, we have done one-way binding with different variables.

```csharp
@page "/one-way-data-binding"

<!-- Use this button to trigger changes in the source values -->
<button onclick="@ChangeValues">Change values</button>

<p>Counter: @Count</p>

@if (ShowWarning)
{
    <p style="background-color: red; padding: 5px">Warning!</p>
}

<p style="background-color: @Background; color=white; padding: 5px">Notification</p>

<ul>
    @foreach (var number in Numbers)
    {
        <li>@number</li>
    }
</ul>

@functions {
    private int Count { get; set; } = 0;
    private bool ShowWarning { get; set; } = true;
    private string Background { get; set; } = "red";
    private List<int> Numbers { get; set; } = new List<int> { 1, 2, 3 };

    private void ChangeValues()
    {
        Count++;
        ShowWarning = !ShowWarning;
        Background = Background == "red" ? "green" : "red";
        Numbers.Add(Numbers.Max() + 1);
    }
}

```

## Two-way Data Binding

Blazor also supports two-way data binding by using `bind` attribute. Currently, Blazor supports only the following data types for two-way data binding.

 - string
 - int
 - DateTime
 - Enum
 - bool

If you need other types (e.g. decimal), you need to provide getter and setter from/to a supported type.

The following example demonstrates different two-way binding scenarios. 

```csharp
@page "/two-way-data-binding"

<p>
    Enter your name: <input type="text" bind="@Name" /><br />
    What is your age? <input type="number" bind="@Age" /><br />
    What is your salery? <input type="number" bind="@Salary" /><br />

    What is your birthday (culture-invariant default format)? <input type="text" bind="@Birthday" /><br />
    What is your birthday (German date format)? <input type="text" bind="@Birthday" format-value="dd.MM.yyyy" /><br />

    Are you a manager? <input type="checkbox" bind="@IsManager" /><br />

    <select id="select-box" bind="@TypeOfEmployee">
        <option value=@EmployeeType.Employee>@EmployeeType.Employee.ToString()</option>
        <option value=@EmployeeType.Contractor>@EmployeeType.Contractor.ToString()</option>
        <option value=@EmployeeType.Intern>@EmployeeType.Intern.ToString()</option>
    </select>
</p>

<h2>Hello, @Name!</h2>

<p>You are @Age years old. You are born on the @Birthday. You are @TypeOfEmployee.</p>

@if (IsManager)
{
    <p>You are a manager!</p>
}

    <p>Your salary is $@Salary</p>

@functions {
private enum EmployeeType { Employee, Contractor, Intern };
private EmployeeType TypeOfEmployee { get; set; } = EmployeeType.Employee;

private string Name { get; set; } = "Mark";
private bool IsManager { get; set; } = true;
private static int Age { get; set; } = 26;
public DateTime Birthday { get; set; } = DateTime.Today.AddYears(-Age);

public decimal Salary { get; set; } = (decimal) 2800.5;
}

```

You can find more information about two-way binding on [Blazor Github](https://github.com/aspnet/Blazor/issues/409).

## Event Binding

Event binding is quite limited in Blazor. Currently, only `onclick` and `onchange` are supported. You find more details in the [Blazor GitHub](https://github.com/aspnet/Blazor/issues/503).

```csharp
@page "/event-binding"

<h3>Event Binding</h3> <br /> <br />
<button onclick=@ButtonClicked>Event Binding Example</button>  <br /> <br />
<button onclick=@(() => Message = "Inline:Button Clicked")>Inline Event Binding</button> <br /> <br />
<p>Message: @Message</p>

@functions {
    private string Message { get; set; } = "";
    void ButtonClicked()
    {
        Message = "Button Clicked";
    }
}
```
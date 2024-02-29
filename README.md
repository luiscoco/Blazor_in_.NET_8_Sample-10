# Blazor in .NET 8: Sample 10

Let's explore error handling and user feedback mechanisms in Blazor.

Example: Form with Validation and Error Display

Features Demonstrated:

Blazor Error Boundaries: Capturing errors within components and providing fallback UI.

Validation: Enforcing input rules and providing clear error messages.

User Feedback: Displaying contextual error messages and success notifications.

Setup

Project: Create a new Blazor Server project or reuse an existing one. Let's name it "FormValidationDemo".

Components

ErrorBoundary.razor (Inside Shared folder)

Razor CSHTML
@code { 
    [Parameter] public RenderFragment ChildContent { get; set; }
    [Parameter] public RenderFragment<Exception> ErrorContent { get; set; } 

    private Exception exception;

    protected override Task OnErrorAsync(Exception ex)
    {
        exception = ex;
        StateHasChanged();
        return base.OnErrorAsync(ex);
    }
}
Usa el código con precaución.

ValidatedForm.razor

Razor CSHTML
@page "/validated-form"

<ErrorBoundary ErrorContent="@ErrorContent">
    <EditForm Model="@model" OnValidSubmit="@HandleValidSubmit">
        <DataAnnotationsValidator />

        <div class="form-group">
            <label for="name">Name:</label>
            <InputText id="name" class="form-control" @bind-Value="@model.Name" />
            <ValidationMessage For="@(() => model.Name)" />
        </div>

        <button type="submit" class="btn btn-primary">Submit</button>
    </EditForm>
</ErrorBoundary>

@code {
    private FormModel model = new FormModel();

    private RenderFragment<Exception> ErrorContent = (exception) => (builder) =>
    {
        builder.AddMarkupContent(0, "<h3>An error occurred!</h3>");
    }; 

    private void HandleValidSubmit()
    {
        // Submission logic
    }
}
Usa el código con precaución.

FormModel.cs (Inside a Models folder)

C#
public class FormModel
{
    [Required]
    [StringLength(50, ErrorMessage = "Name is too long.")] 
    public string Name { get; set; }

    //  ... (More properties with validation rules) 
} 
Usa el código con precaución.

Explanation

ErrorBoundary: Wraps components, catches errors, and renders ErrorContent.

ValidatedForm: The form uses EditForm, DataAnnotationsValidator, and ValidationMessage components for built-in validation.

ErrorContent: A placeholder for now. You could provide a generic error message or more context-specific details.

Next Steps

Make the ErrorBoundary component more sophisticated; display user-friendly, customized error messages.

Use libraries like FluentValidation for complex validation scenarios.

Create visually appealing success or error notifications (using toast messages or modal popups).




# 🎯 Master Data Annotations & Fluent API From A to Z

## A Comprehensive Guide from Beginner to Advanced Level

---

## 📖 Table of Contents

1. [Introduction](#1-introduction)
2. [Prerequisites](#2-prerequisites)
3. [Chapter 1: Understanding Data Annotations](#3-chapter-1-understanding-data-annotations)
4. [Chapter 2: Validation Attributes](#4-chapter-2-validation-attributes)
5. [Chapter 3: Display Attributes](#5-chapter-3-display-attributes)
6. [Chapter 4: Database Schema Attributes](#6-chapter-4-database-schema-attributes)
7. [Chapter 5: Understanding Fluent API](#7-chapter-5-understanding-fluent-api)
8. [Chapter 6: Fluent API - Property Configuration](#8-chapter-6-fluent-api---property-configuration)
9. [Chapter 7: Fluent API - Relationships](#9-chapter-7-fluent-api---relationships)
10. [Chapter 8: Fluent API - Keys and Indexes](#10-chapter-8-fluent-api---keys-and-indexes)
11. [Chapter 9: Fluent API - Inheritance Mapping](#11-chapter-9-fluent-api---inheritance-mapping)
12. [Chapter 10: Advanced Scenarios](#12-chapter-10-advanced-scenarios)
13. [Chapter 11: Custom Validation](#13-chapter-11-custom-validation)
14. [Chapter 12: Data Annotations vs Fluent API](#14-chapter-12-data-annotations-vs-fluent-api)
15. [Chapter 13: Best Practices](#15-chapter-13-best-practices)
16. [Chapter 14: Common Mistakes](#16-chapter-14-common-mistakes)
17. [Quick Reference Cheatsheet](#17-quick-reference-cheatsheet)
18. [Additional Resources](#18-additional-resources)

---

## 1. Introduction

### What are Data Annotations?

Data Annotations are attributes in .NET that you can apply to classes and properties to add metadata. This metadata serves multiple purposes: it controls how properties are displayed in the UI, enforces validation rules, and configures how Entity Framework maps classes to database tables. Data Annotations provide a declarative approach to configuration, keeping the metadata directly on the model classes where it's easy to see and maintain.

The System.ComponentModel.DataAnnotations namespace contains the core validation and display attributes, while System.ComponentModel.DataAnnotations.Schema contains database-specific attributes. These attributes are recognized by ASP.NET MVC for client-side and server-side validation, by Entity Framework for database mapping, and by various UI frameworks for display formatting.

### What is Fluent API?

Fluent API is a programmatic approach to configuring Entity Framework models. Instead of using attributes on your classes, you configure the model in the OnModelCreating method of your DbContext. This approach provides more flexibility and power than Data Annotations, allowing configurations that aren't possible with attributes alone. The "fluent" name comes from the method chaining pattern that makes the configuration readable and expressive.

Fluent API configurations are centralized in one place, separate from your entity classes. This keeps your entity classes clean and focused on domain concerns rather than infrastructure configuration. Fluent API also allows you to configure third-party entity classes that you cannot modify with attributes, and it provides the only way to configure certain advanced scenarios like many-to-many relationships with payload, composite keys, and inheritance strategies.

### Why Learn Both?

Understanding both Data Annotations and Fluent API is essential for .NET developers. Data Annotations are simpler and more discoverable, making them ideal for common scenarios and quick prototyping. Fluent API provides the full power of Entity Framework configuration, necessary for complex real-world applications. In practice, many applications use both approaches together—Data Annotations for validation and display, Fluent API for advanced database configuration.

Learning both approaches gives you the flexibility to choose the right tool for each situation. You'll understand when Data Annotations are sufficient and when you need the additional power of Fluent API. This knowledge enables you to build maintainable applications that follow best practices while meeting complex business requirements.

### What You Will Learn

This comprehensive guide takes you from the fundamentals through advanced scenarios. You'll start by understanding all Data Annotations attributes and their purposes. Then you'll learn Fluent API configurations for properties, relationships, keys, indexes, and inheritance. Advanced chapters cover custom validation, complex configurations, and best practices. By the end, you'll have complete mastery of both approaches.

---

## 2. Prerequisites

### Required Knowledge

Before diving into Data Annotations and Fluent API, you should have a solid understanding of C# programming, including classes, properties, and attributes. Knowledge of object-oriented programming concepts like inheritance and interfaces will help you understand more advanced configurations. Basic understanding of relational databases, including tables, columns, primary keys, foreign keys, and indexes, provides the context for why these configurations matter.

Familiarity with Entity Framework Core is essential, as Fluent API is primarily used with EF Core for database mapping. Understanding how EF Core conventions work helps you appreciate when and why to override them with explicit configuration. Knowledge of ASP.NET MVC validation will help you understand how Data Annotations integrate with web applications.

### Development Environment

You'll need a .NET development environment with Visual Studio 2022 or Visual Studio Code. The .NET SDK (version 6, 7, or 8) must be installed. A SQL Server instance (LocalDB, Express, or full SQL Server) is needed for testing database configurations. Entity Framework Core tools are required for creating and applying migrations.

### NuGet Packages

The following NuGet packages are commonly used when working with Data Annotations and Fluent API:

```xml
<PackageReference Include="Microsoft.EntityFrameworkCore" Version="8.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer" Version="8.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="8.0.0" />
<PackageReference Include="System.ComponentModel.Annotations" Version="5.0.0" />
```

---

## 3. Chapter 1: Understanding Data Annotations

### The Anatomy of Data Annotations

Data Annotations are .NET attributes that you apply to classes and properties using square bracket syntax. Each attribute is a class that inherits from System.Attribute and can accept parameters to customize its behavior. Attributes can be combined on a single property to apply multiple configurations simultaneously.

```csharp
public class Product
{
    [Key]
    public int Id { get; set; }
    
    [Required]
    [StringLength(100, MinimumLength = 2)]
    [Display(Name = "Product Name")]
    public string Name { get; set; }
}
```

### Attribute Targets

Data Annotations can be applied to different targets depending on their purpose. Most attributes target properties, but some can be applied to classes or even entire assemblies. The AttributeUsage attribute on each attribute class determines where it can be applied.

```csharp
// Property-level attribute
public class Product
{
    [Required] // Applied to property
    public string Name { get; set; }
}

// Class-level attribute
[Table("Products", Schema = "Inventory")]
public class Product
{
    public int Id { get; set; }
}

// Assembly-level attribute (rare)
[assembly: AssemblyTitle("My Application")]
```

### How Data Annotations Work

When you apply Data Annotations to your classes, multiple frameworks recognize and use them. ASP.NET MVC uses validation attributes during model binding to validate user input, automatically generating client-side validation attributes for JavaScript validation. The display attributes influence how scaffolding generates views and how HTML helpers render labels and display templates.

Entity Framework Core examines Data Annotations during model creation to determine the database schema. The model builder reads attributes and translates them into database configurations. However, EF Core also has default conventions that apply when no attributes are present—understanding these conventions helps you know when explicit configuration is necessary.

### Namespaces

Data Annotations are organized across several namespaces, each serving a specific purpose:

```csharp
// Core validation and display attributes
using System.ComponentModel.DataAnnotations;

// Database schema attributes
using System.ComponentModel.DataAnnotations.Schema;

// Additional data types
using System.ComponentModel; // For [Bindable], [Browsable], etc.
```

### The Role of Metadata

Data Annotations serve as metadata that describes aspects of your model beyond just the data structure. This metadata describes constraints (validation), presentation (display), and persistence (database mapping). Keeping this metadata on the model class follows the "single source of truth" principle—the model carries its own description.

However, this tight coupling between the model and its metadata can become problematic in complex applications. A model class used for multiple purposes (database entity, API DTO, view model) might need different metadata in different contexts. This is where separating concerns through view models and Fluent API becomes valuable.

---

## 4. Chapter 2: Validation Attributes

### Overview of Validation Attributes

Validation attributes enforce business rules on your data, ensuring that values meet specific criteria before being accepted. These attributes work seamlessly with ASP.NET MVC's validation system, providing both server-side and client-side validation. Entity Framework also validates entities before saving changes to the database, throwing exceptions when validation fails.

All validation attributes inherit from ValidationAttribute, which provides the foundation for validation logic. The attribute can provide an error message directly, or you can use resource files for localization. Custom validation messages can include placeholders that are filled with context-specific values.

### [Required] - Ensuring Values Exist

The Required attribute ensures that a property has a value. For reference types (strings, classes), this means the value cannot be null. For value types (int, DateTime), which cannot be null, Required ensures the value is not the default (0 for int, 1/1/0001 for DateTime). Use nullable value types (int?, DateTime?) if you want to make value types truly optional.

```csharp
public class Customer
{
    public int Id { get; set; }
    
    [Required(ErrorMessage = "Customer name is required")]
    [StringLength(100)]
    public string Name { get; set; }
    
    [Required]
    [EmailAddress]
    public string Email { get; set; }
    
    [Required]
    [Phone]
    public string PhoneNumber { get; set; }
    
    // Nullable - not required
    public string? Notes { get; set; }
    
    // Value type with Required - cannot be 0
    [Required]
    public int Age { get; set; }
    
    // Nullable value type - optional
    public int? OptionalAge { get; set; }
    
    // Required with AllowEmptyStrings (default is false)
    [Required(AllowEmptyStrings = false)]
    public string Description { get; set; }
}
```

### [StringLength] - Text Length Constraints

StringLength validates the maximum length of a string and optionally a minimum length. This attribute only works with string properties and provides both database column length configuration and runtime validation. The database column is created as NVARCHAR(maxLength) in SQL Server.

```csharp
public class Product
{
    public int Id { get; set; }
    
    // Maximum 100 characters
    [StringLength(100)]
    public string Name { get; set; }
    
    // Between 10 and 500 characters
    [StringLength(500, MinimumLength = 10, 
        ErrorMessage = "Description must be between {2} and {1} characters")]
    public string Description { get; set; }
    
    // Exact length
    [StringLength(10, MinimumLength = 10,
        ErrorMessage = "SKU must be exactly 10 characters")]
    public string SKU { get; set; }
    
    // Short name with specific constraints
    [StringLength(50, MinimumLength = 3,
        ErrorMessage = "The {0} must be between {2} and {1} characters.")]
    [Display(Name = "Short Name")]
    public string ShortName { get; set; }
}
```

### [MaxLength] and [MinLength] - Collection and String Length

MaxLength and MinLength are similar to StringLength but can also be applied to collections (arrays, lists). MaxLength affects database column size for strings (NVARCHAR(max)) but doesn't provide a specific length like StringLength. Use StringLength for strings when you want precise database column sizing.

```csharp
public class Blog
{
    public int Id { get; set; }
    
    // MaxLength for string
    [MaxLength(4000, ErrorMessage = "Content cannot exceed 4000 characters")]
    public string Content { get; set; }
    
    // MaxLength for collection
    [MaxLength(5, ErrorMessage = "Maximum 5 tags allowed")]
    public List<string> Tags { get; set; }
    
    // MinLength
    [MinLength(1, ErrorMessage = "At least one category is required")]
    public List<string> Categories { get; set; }
    
    // Combined min and max for collection
    [MinLength(1)]
    [MaxLength(10)]
    public List<string> Keywords { get; set; }
}
```

### [Range] - Numeric Range Validation

Range validates that a numeric value falls within specified minimum and maximum bounds. The attribute works with numeric types including int, double, decimal, and their nullable counterparts. You can also validate DateTime values with Range using string representations.

```csharp
public class Product
{
    public int Id { get; set; }
    
    // Integer range
    [Range(0, 100, ErrorMessage = "Discount must be between {1} and {2} percent")]
    public int DiscountPercentage { get; set; }
    
    // Decimal range
    [Range(0.01, 100000.00, ErrorMessage = "Price must be between {1:C} and {2:C}")]
    public decimal Price { get; set; }
    
    // Double range
    [Range(0.0, 5.0, ErrorMessage = "Rating must be between {1} and {2}")]
    public double Rating { get; set; }
    
    // Integer range - quantity
    [Range(0, int.MaxValue, ErrorMessage = "Quantity cannot be negative")]
    public int StockQuantity { get; set; }
    
    // DateTime range using string format
    [Range(typeof(DateTime), "2024-01-01", "2025-12-31", 
        ErrorMessage = "Date must be in 2024 or 2025")]
    public DateTime OrderDate { get; set; }
    
    // Positive values only
    [Range(0.01, double.MaxValue, ErrorMessage = "Amount must be positive")]
    public decimal Amount { get; set; }
}
```

### [RegularExpression] - Pattern Matching

RegularExpression validates that a string value matches a specified regex pattern. This is powerful for complex validation rules like product codes, serial numbers, or custom identifier formats. The pattern uses standard .NET regular expression syntax.

```csharp
public class InventoryItem
{
    public int Id { get; set; }
    
    // Product code format: PROD-XXXX-XXXX (X = alphanumeric)
    [RegularExpression(@"^PROD-[A-Z0-9]{4}-[A-Z0-9]{4}$",
        ErrorMessage = "Product code must be in format PROD-XXXX-XXXX")]
    public string ProductCode { get; set; }
    
    // Serial number: SN followed by 8 digits
    [RegularExpression(@"^SN\d{8}$",
        ErrorMessage = "Serial number must be SN followed by 8 digits")]
    public string SerialNumber { get; set; }
    
    // Only letters and spaces
    [RegularExpression(@"^[a-zA-Z\s]+$",
        ErrorMessage = "Name can only contain letters and spaces")]
    public string Name { get; set; }
    
    // Alphanumeric with hyphens and underscores
    [RegularExpression(@"^[a-zA-Z0-9_-]+$",
        ErrorMessage = "Username can only contain letters, numbers, hyphens, and underscores")]
    public string Username { get; set; }
    
    // Hex color code
    [RegularExpression(@"^#([A-Fa-f0-9]{6}|[A-Fa-f0-9]{3})$",
        ErrorMessage = "Color must be a valid hex color code (e.g., #FF0000)")]
    public string ColorCode { get; set; }
    
    // Postal code (US format)
    [RegularExpression(@"^\d{5}(-\d{4})?$",
        ErrorMessage = "Invalid ZIP code format")]
    public string PostalCode { get; set; }
    
    // Credit card number (basic format check)
    [RegularExpression(@"^\d{13,19}$",
        ErrorMessage = "Invalid credit card number")]
    public string CreditCardNumber { get; set; }
}
```

### [EmailAddress] - Email Validation

EmailAddress validates that a property contains a valid email address format. Internally, it uses a regular expression to check the format. While useful for basic validation, real-world email validation should also include domain verification.

```csharp
public class User
{
    public int Id { get; set; }
    
    // Basic email validation
    [EmailAddress(ErrorMessage = "Invalid email address")]
    public string Email { get; set; }
    
    // Combined with Required
    [Required]
    [EmailAddress]
    [Display(Name = "Email Address")]
    public string PrimaryEmail { get; set; }
    
    // Optional email
    [EmailAddress]
    public string? SecondaryEmail { get; set; }
}
```

### [Phone] - Phone Number Validation

Phone validates phone number formats. The attribute uses a regex pattern that accommodates various international phone number formats, including optional country codes, area codes, and common formatting characters like parentheses, spaces, and hyphens.

```csharp
public class Contact
{
    public int Id { get; set; }
    
    [Required]
    [Phone(ErrorMessage = "Invalid phone number")]
    [Display(Name = "Phone Number")]
    public string PhoneNumber { get; set; }
    
    // Mobile phone
    [Phone]
    [Display(Name = "Mobile")]
    public string? MobileNumber { get; set; }
    
    // Fax (less common but still uses phone format)
    [Phone]
    public string? FaxNumber { get; set; }
}
```

### [Url] - URL Validation

Url validates that a property contains a valid URL format. The URL must include a protocol (http, https, ftp, etc.) to be considered valid. This is useful for validating website links, API endpoints, or resource URLs.

```csharp
public class SocialProfile
{
    public int Id { get; set; }
    
    [Url(ErrorMessage = "Please enter a valid URL")]
    [Display(Name = "Website")]
    public string WebsiteUrl { get; set; }
    
    [Url]
    [Display(Name = "LinkedIn Profile")]
    public string? LinkedInUrl { get; set; }
    
    // Combined with other attributes
    [Required]
    [Url]
    [StringLength(200)]
    [Display(Name = "Profile Picture URL")]
    public string ProfilePictureUrl { get; set; }
}
```

### [CreditCard] - Credit Card Validation

CreditCard validates credit card number formats. It performs a Luhn algorithm check to verify the number could be a valid credit card. Note that this only validates the format—it doesn't verify the card actually exists or is valid for transactions.

```csharp
public class PaymentMethod
{
    public int Id { get; set; }
    
    [CreditCard(ErrorMessage = "Invalid credit card number")]
    [Display(Name = "Card Number")]
    public string CardNumber { get; set; }
    
    // Combined validation
    [Required]
    [CreditCard]
    [StringLength(19)] // Allow for spaces/dashes in display format
    public string PrimaryCard { get; set; }
}
```

### [Compare] - Property Comparison

Compare validates that two properties have matching values. This is commonly used for password confirmation fields. The attribute takes the name of the other property to compare against.

```csharp
public class RegisterViewModel
{
    public int Id { get; set; }
    
    [Required]
    [StringLength(100, MinimumLength = 8)]
    [DataType(DataType.Password)]
    public string Password { get; set; }
    
    [Compare("Password", ErrorMessage = "Password and confirmation do not match")]
    [DataType(DataType.Password)]
    [Display(Name = "Confirm Password")]
    public string ConfirmPassword { get; set; }
    
    // Email confirmation
    [Required]
    [EmailAddress]
    public string Email { get; set; }
    
    [Compare("Email", ErrorMessage = "Email addresses do not match")]
    [Display(Name = "Confirm Email")]
    public string ConfirmEmail { get; set; }
}
```

### [DataType] - Semantic Data Types

DataType specifies the type of data a property represents, affecting how it's displayed and edited. This doesn't provide validation but influences UI rendering. Common data types include Password, Date, Time, DateTime, PhoneNumber, EmailAddress, Url, Currency, and MultilineText.

```csharp
public class UserProfile
{
    public int Id { get; set; }
    
    [DataType(DataType.Password)]
    public string Password { get; set; }
    
    [DataType(DataType.EmailAddress)]
    public string Email { get; set; }
    
    [DataType(DataType.PhoneNumber)]
    public string Phone { get; set; }
    
    [DataType(DataType.Url)]
    public string Website { get; set; }
    
    [DataType(DataType.Date)]
    [DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}")]
    public DateTime BirthDate { get; set; }
    
    [DataType(DataType.Time)]
    public DateTime StartTime { get; set; }
    
    [DataType(DataType.DateTime)]
    public DateTime CreatedAt { get; set; }
    
    [DataType(DataType.Currency)]
    public decimal Balance { get; set; }
    
    [DataType(DataType.MultilineText)]
    public string Biography { get; set; }
    
    [DataType(DataType.Upload)]
    public byte[] ProfilePicture { get; set; }
    
    [DataType(DataType.ImageUrl)]
    public string AvatarUrl { get; set; }
    
    [DataType(DataType.PostalCode)]
    public string PostalCode { get; set; }
}
```

### [FileExtensions] - File Type Validation

FileExtensions validates that a filename has an allowed extension. This is useful when accepting file uploads to restrict the types of files users can submit.

```csharp
public class Document
{
    public int Id { get; set; }
    
    [FileExtensions(Extensions = "pdf,doc,docx,xls,xlsx",
        ErrorMessage = "Only PDF and Office documents are allowed")]
    public string FileName { get; set; }
    
    // Image files
    [Required]
    [FileExtensions(Extensions = "jpg,jpeg,png,gif,webp",
        ErrorMessage = "Only image files (JPG, PNG, GIF, WebP) are allowed")]
    public string ImageFileName { get; set; }
}
```

### Combining Multiple Validation Attributes

Real-world properties often require multiple validation attributes working together. The order of attributes doesn't affect execution—all validations run and all failures are collected.

```csharp
public class RegistrationViewModel
{
    [Required(ErrorMessage = "Username is required")]
    [StringLength(50, MinimumLength = 3, 
        ErrorMessage = "Username must be between {2} and {1} characters")]
    [RegularExpression(@"^[a-zA-Z][a-zA-Z0-9_-]*$",
        ErrorMessage = "Username must start with a letter and contain only letters, numbers, hyphens, or underscores")]
    [Display(Name = "Username")]
    public string Username { get; set; }
    
    [Required]
    [StringLength(100, MinimumLength = 8, 
        ErrorMessage = "Password must be at least {2} characters")]
    [RegularExpression(@"^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]",
        ErrorMessage = "Password must contain uppercase, lowercase, number, and special character")]
    [DataType(DataType.Password)]
    public string Password { get; set; }
    
    [Required]
    [EmailAddress(ErrorMessage = "Please enter a valid email address")]
    [StringLength(256)]
    public string Email { get; set; }
    
    [Required]
    [Range(18, 120, ErrorMessage = "You must be at least {1} years old")]
    [Display(Name = "Age")]
    public int Age { get; set; }
    
    [Required]
    [Phone]
    [Display(Name = "Phone Number")]
    public string PhoneNumber { get; set; }
    
    [Url]
    [StringLength(500)]
    [Display(Name = "Website (optional)")]
    public string? Website { get; set; }
}
```

---

## 5. Chapter 3: Display Attributes

### [Display] - Comprehensive Display Configuration

The Display attribute is the most versatile display attribute, providing a name, description, order, and grouping for properties. It's used by ASP.NET MVC for generating labels and by scaffolding templates for creating views.

```csharp
public class ProductViewModel
{
    public int Id { get; set; }
    
    // Simple display name
    [Display(Name = "Product Name")]
    public string Name { get; set; }
    
    // Full configuration
    [Display(
        Name = "Product Description",
        Description = "A detailed description of the product for customers",
        Order = 2,
        GroupName = "Product Information",
        Prompt = "Enter a detailed description",
        ShortDisplayName = "Description",
        AutoGenerateField = true,
        AutoGenerateFilter = true)]
    public string Description { get; set; }
    
    // With resource localization
    [Display(
        Name = "ProductName_Name",
        Description = "ProductName_Description",
        ResourceType = typeof(Resources.ProductResources))]
    public string LocalizedName { get; set; }
    
    // Order affects display order in generated views
    [Display(Name = "Price", Order = 3)]
    public decimal Price { get; set; }
    
    [Display(Name = "Category", Order = 1)]
    public string Category { get; set; }
}
```

### [DisplayFormat] - Formatting Values

DisplayFormat controls how property values are formatted for display. It's especially useful for dates, numbers, and currency values. Use with ApplyFormatInEditMode to apply formatting in edit mode as well.

```csharp
public class Order
{
    public int Id { get; set; }
    
    // Date formatting
    [DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
    [DataType(DataType.Date)]
    public DateTime OrderDate { get; set; }
    
    // Date and time
    [DisplayFormat(DataFormatString = "{0:yyyy-MM-dd HH:mm}")]
    public DateTime CreatedAt { get; set; }
    
    // Currency formatting
    [DisplayFormat(DataFormatString = "{0:C2}")]
    public decimal TotalAmount { get; set; }
    
    // Percentage
    [DisplayFormat(DataFormatString = "{0:P2}")]
    public decimal TaxRate { get; set; }
    
    // Custom number format
    [DisplayFormat(DataFormatString = "{0:N2}")]
    public decimal Weight { get; set; }
    
    // Phone number format
    [DisplayFormat(DataFormatString = "{0:(###) ###-####}")]
    public string PhoneNumber { get; set; }
    
    // Handle null/empty values
    [DisplayFormat(NullDisplayText = "Not specified", ConvertEmptyStringToNull = true)]
    public string? Notes { get; set; }
    
    // Boolean display
    [DisplayFormat(DataFormatString = "{0:Yes;No;Maybe}")]
    public bool? IsConfirmed { get; set; }
    
    // Apply format in edit mode
    [DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
    public DateTime? ExpirationDate { get; set; }
    
    // HTML encode (default is true)
    [DisplayFormat(HtmlEncode = false)]
    public string FormattedContent { get; set; }
}
```

### [DisplayName] - Simple Display Name

DisplayName is a simpler alternative to Display when you only need to set the display name. It's been available since earlier versions of .NET and is recognized by many frameworks.

```csharp
public class Category
{
    public int Id { get; set; }
    
    [DisplayName("Category Name")]
    public string Name { get; set; }
    
    [DisplayName("Parent Category")]
    public int? ParentCategoryId { get; set; }
    
    // DisplayName vs Display
    [DisplayName("Description")] // Simple
    public string Description { get; set; }
    
    [Display(Name = "Detailed Description", Description = "Full product description")] // Full
    public string DetailedDescription { get; set; }
}
```

### [DisplayColumn] - Related Entity Display

DisplayColumn specifies which property of a related entity to use for display in scaffolded views. This is applied at the class level and affects how foreign key relationships are displayed.

```csharp
[DisplayColumn("FullName")]
public class Customer
{
    public int Id { get; set; }
    
    public string FirstName { get; set; }
    
    public string LastName { get; set; }
    
    // This will be displayed when Customer is shown in a dropdown
    public string FullName => $"{FirstName} {LastName}";
    
    public string Email { get; set; }
}

[DisplayColumn("Name")]
public class Category
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Code { get; set; }
}

// When displaying an Order with a Customer reference,
// the Customer's FullName property will be shown
public class Order
{
    public int Id { get; set; }
    
    public Customer Customer { get; set; }
    // Dropdown will show Customer.FullName instead of Customer.ToString()
}
```

### [ScaffoldColumn] - Controlling Scaffolding

ScaffoldColumn controls whether a property is included in scaffolded views. Set to false to hide properties that shouldn't appear in generated forms and displays.

```csharp
public class Product
{
    public int Id { get; set; }
    
    // This will be included in scaffolding
    public string Name { get; set; }
    
    // This will be hidden from scaffolded views
    [ScaffoldColumn(false)]
    public byte[] RowVersion { get; set; }
    
    [ScaffoldColumn(false)]
    public DateTime CreatedAt { get; set; }
    
    [ScaffoldColumn(false)]
    public string CreatedBy { get; set; }
    
    // Calculated property - don't scaffold
    [ScaffoldColumn(false)]
    public decimal TaxAmount => Price * 0.1m;
    
    public decimal Price { get; set; }
}
```

### [ReadOnly] - Preventing Editing

ReadOnly marks a property as read-only in UI frameworks. While it doesn't prevent programmatic modification, it signals to scaffolding and UI components that the property shouldn't be edited.

```csharp
public class Order
{
    public int Id { get; set; }
    
    [ReadOnly(true)]
    public string OrderNumber { get; set; }
    
    [ReadOnly(true)]
    [DisplayFormat(DataFormatString = "{0:C2}")]
    public decimal TotalAmount { get; set; } // Calculated, not editable
    
    [ReadOnly(true)]
    public DateTime CreatedAt { get; set; }
    
    [ReadOnly(true)]
    public string Status { get; set; }
    
    // Editable property
    public string? Notes { get; set; }
}
```

### [HiddenInput] - Hidden Form Fields

HiddenInput indicates that a property should be rendered as a hidden input field. This is useful for properties that need to be included in forms but not displayed, like IDs or timestamps.

```csharp
public class ProductViewModel
{
    [HiddenInput]
    public int Id { get; set; }
    
    [HiddenInput(DisplayValue = false)]
    public int CategoryId { get; set; }
    
    // DisplayValue = true shows the value as read-only text plus hidden field
    [HiddenInput(DisplayValue = true)]
    public string ProductCode { get; set; }
    
    [HiddenInput]
    public byte[] RowVersion { get; set; } // For concurrency
    
    // Regular fields
    public string Name { get; set; }
    public decimal Price { get; set; }
}
```

### [UIHint] - Custom Display Templates

UIHint specifies the name of a custom display or editor template to use for rendering the property. This allows you to create custom templates for specific data types or display requirements.

```csharp
public class Article
{
    public int Id { get; set; }
    
    // Use custom "WysiwygEditor" template
    [UIHint("WysiwygEditor")]
    [DataType(DataType.MultilineText)]
    public string Content { get; set; }
    
    // Use "ColorPicker" template
    [UIHint("ColorPicker")]
    public string ThemeColor { get; set; }
    
    // Use "MarkdownEditor" template
    [UIHint("MarkdownEditor")]
    public string MarkdownContent { get; set; }
    
    // Use "Rating" template (stars)
    [UIHint("Rating")]
    [Range(1, 5)]
    public int Rating { get; set; }
    
    // Use "FileUpload" template
    [UIHint("FileUpload")]
    public string AttachmentPath { get; set; }
    
    // Use "DatePicker" template
    [UIHint("DatePicker")]
    [DataType(DataType.Date)]
    public DateTime PublishDate { get; set; }
}

// The templates would be placed in:
// Views/Shared/EditorTemplates/WysiwygEditor.cshtml
// Views/Shared/EditorTemplates/ColorPicker.cshtml
// Views/Shared/DisplayTemplates/Rating.cshtml
```

### [Editable] - Edit Control

Editable controls whether a property is editable in data-binding scenarios. Unlike ReadOnly, this attribute affects data-binding controls in addition to scaffolding.

```csharp
public class Customer
{
    public int Id { get; set; }
    
    [Editable(true)]
    public string Name { get; set; }
    
    [Editable(false)]
    public DateTime CreatedAt { get; set; } // System-set, not user-editable
    
    [Editable(false)]
    public string CreatedBy { get; set; }
    
    [Editable(false)]
    public int OrderCount { get; set; } // Calculated field
    
    [Editable(true)]
    public string Email { get; set; }
}
```

---

## 6. Chapter 4: Database Schema Attributes

### [Table] - Table Configuration

The Table attribute specifies the database table name and optionally the schema for an entity class. Use this when you want the table name to differ from the class name or when organizing tables into schemas.

```csharp
// Simple table name
[Table("Products")]
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
}

// Table with schema
[Table("Products", Schema = "Inventory")]
public class Product
{
    public int Id { get; set; }
}

// Multiple schemas for organization
[Table("Users", Schema = "Security")]
public class User
{
    public int Id { get; set; }
}

[Table("AuditLogs", Schema = "System")]
public class AuditLog
{
    public int Id { get; set; }
}

[Table("Orders", Schema = "Sales")]
public class Order
{
    public int Id { get; set; }
}

[Table("Customers", Schema = "CRM")]
public class Customer
{
    public int Id { get; set; }
}

// Historical naming convention
[Table("tbl_ProductMaster")]
public class Product
{
    public int Id { get; set; }
}
```

### [Column] - Column Configuration

Column specifies the column name, order, and type name for a property. Use this when you want the column name to differ from the property name or need to specify a specific database type.

```csharp
[Table("Products")]
public class Product
{
    // Column with custom name
    [Column("ProductID")]
    public int Id { get; set; }
    
    // Column with type specification
    [Column("ProductName", TypeName = "nvarchar(200)")]
    public string Name { get; set; }
    
    // Column with order (for complex types)
    [Column("Description", Order = 2)]
    public string Description { get; set; }
    
    // Specific SQL Server types
    [Column(TypeName = "money")]
    public decimal Price { get; set; }
    
    [Column(TypeName = "date")]
    public DateTime CreatedDate { get; set; }
    
    [Column(TypeName = "datetime2")]
    public DateTime LastModified { get; set; }
    
    [Column(TypeName = "time")]
    public TimeSpan OpenTime { get; set; }
    
    [Column(TypeName = "varchar(50)")]
    public string SKU { get; set; }
    
    [Column(TypeName = "char(10)")]
    public string FixedCode { get; set; }
    
    [Column(TypeName = "decimal(18,4)")]
    public decimal PreciseValue { get; set; }
    
    [Column(TypeName = "varbinary(max)")]
    public byte[] FileData { get; set; }
    
    // JSON column (SQL Server 2019+)
    [Column(TypeName = "json")]
    public string JsonData { get; set; }
    
    // XML column
    [Column(TypeName = "xml")]
    public string XmlData { get; set; }
}
```

### [Key] - Primary Key Configuration

Key marks one or more properties as the primary key. Entity Framework convention automatically detects properties named "Id" or "{ClassName}Id" as primary keys, but Key is needed for other naming conventions or composite keys.

```csharp
// Simple primary key (convention would handle this)
public class Product
{
    [Key]
    public int Id { get; set; }
}

// Primary key with non-standard name
public class Product
{
    [Key]
    public int ProductCode { get; set; }
}

// Composite primary key
public class OrderItem
{
    [Key]
    [Column(Order = 1)]
    public int OrderId { get; set; }
    
    [Key]
    [Column(Order = 2)]
    public int ProductId { get; set; }
    
    public int Quantity { get; set; }
    public decimal UnitPrice { get; set; }
}

// GUID primary key
public class User
{
    [Key]
    public Guid UserId { get; set; }
}
```

### [DatabaseGenerated] - Auto-Generated Values

DatabaseGenerated specifies how the database generates values for a property. Options include None (no auto-generation), Identity (auto-increment for numeric keys), and Computed (calculated by the database).

```csharp
public class Product
{
    // Identity column (auto-increment)
    [Key]
    [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
    public int Id { get; set; }
    
    // Computed column
    [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
    public string FullName { get; set; } // Computed: FirstName + ' ' + LastName
    
    // No database generation (set by application)
    [DatabaseGenerated(DatabaseGeneratedOption.None)]
    public Guid ProductCode { get; set; } // Application generates GUID
    
    // RowVersion for concurrency
    [Timestamp]
    [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
    public byte[] RowVersion { get; set; }
    
    // Created date set by application
    [DatabaseGenerated(DatabaseGeneratedOption.None)]
    public DateTime CreatedAt { get; set; }
    
    // Modified date set by database trigger
    [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
    public DateTime? ModifiedAt { get; set; }
}

// Sequence-generated value (PostgreSQL/Oracle)
public class Invoice
{
    [Key]
    [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
    public int Id { get; set; }
    
    // Invoice number from custom sequence
    [DatabaseGenerated(DatabaseGeneratedOption.None)]
    public string InvoiceNumber { get; set; }
}
```

### [ForeignKey] - Foreign Key Relationships

ForeignKey specifies which property is the foreign key for a navigation property, or which navigation property a foreign key belongs to. This is needed when the foreign key property name doesn't follow convention.

```csharp
public class Order
{
    public int Id { get; set; }
    public DateTime OrderDate { get; set; }
    
    // Navigation property
    public Customer Customer { get; set; }
    
    // Foreign key property (convention would handle this)
    public int CustomerId { get; set; }
}

public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    
    // Navigation property
    [ForeignKey("CategoryIdentifier")]
    public Category Category { get; set; }
    
    // Foreign key with non-conventional name
    [ForeignKey("Category")]
    public int CategoryIdentifier { get; set; }
}

public class OrderItem
{
    public int Id { get; set; }
    
    // Multiple ways to specify foreign key
    
    // Option 1: On navigation property
    [ForeignKey("OrderId")]
    public Order Order { get; set; }
    public int OrderId { get; set; }
    
    // Option 2: On foreign key property
    public Product Product { get; set; }
    [ForeignKey("Product")]
    public int ProductId { get; set; }
}

// Self-referencing relationship
public class Category
{
    public int Id { get; set; }
    public string Name { get; set; }
    
    [ForeignKey("ParentCategoryId")]
    public Category ParentCategory { get; set; }
    public int? ParentCategoryId { get; set; }
    
    public ICollection<Category> SubCategories { get; set; }
}
```

### [InverseProperty] - Bidirectional Relationships

InverseProperty specifies the inverse navigation property when there are multiple relationships between the same entities. This resolves ambiguity in relationship configuration.

```csharp
public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
    
    // User created these posts
    public ICollection<Post> AuthoredPosts { get; set; }
    
    // User edited these posts
    public ICollection<Post> EditedPosts { get; set; }
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    
    // Post author
    [InverseProperty("AuthoredPosts")]
    public User Author { get; set; }
    public int AuthorId { get; set; }
    
    // Post editor
    [InverseProperty("EditedPosts")]
    public User Editor { get; set; }
    public int? EditorId { get; set; }
}

// Another example: Order with multiple addresses
public class Order
{
    public int Id { get; set; }
    
    [InverseProperty("BillingOrders")]
    public Address BillingAddress { get; set; }
    public int BillingAddressId { get; set; }
    
    [InverseProperty("ShippingOrders")]
    public Address ShippingAddress { get; set; }
    public int ShippingAddressId { get; set; }
}

public class Address
{
    public int Id { get; set; }
    
    public ICollection<Order> BillingOrders { get; set; }
    public ICollection<Order> ShippingOrders { get; set; }
}
```

### [Required] - Database Not Null

When applied to entity properties, Required affects database schema by making the column NOT NULL. It also configures the relationship as required for navigation properties.

```csharp
public class Product
{
    public int Id { get; set; }
    
    // NOT NULL column
    [Required]
    public string Name { get; set; }
    
    // Required foreign key (NOT NULL)
    [Required]
    public int CategoryId { get; set; }
    
    // Required navigation
    [Required]
    public Category Category { get; set; }
    
    // Optional (nullable)
    public string? Description { get; set; }
}
```

### [MaxLength] and [MinLength] - Column Size

For database schema, MaxLength determines the column size for string properties. StringLength is preferred for strings because it provides validation in addition to schema configuration.

```csharp
public class Product
{
    public int Id { get; set; }
    
    // NVARCHAR(100)
    [MaxLength(100)]
    public string Name { get; set; }
    
    // NVARCHAR(MAX)
    [MaxLength]
    public string Description { get; set; }
    
    // NVARCHAR(500) with validation
    [StringLength(500)]
    public string ShortDescription { get; set; }
    
    // Specific size
    [MaxLength(2000)]
    public string Content { get; set; }
}
```

### [Timestamp] - Row Version for Concurrency

Timestamp marks a property as a row version column for optimistic concurrency control. The property must be a byte array, and the database automatically updates it on each modification.

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
    
    // Row version for concurrency
    [Timestamp]
    public byte[] RowVersion { get; set; }
}

// Usage in update:
public async Task<IActionResult> Update(Product product)
{
    try
    {
        _context.Update(product);
        await _context.SaveChangesAsync();
        return RedirectToAction("Index");
    }
    catch (DbUpdateConcurrencyException)
    {
        // Handle concurrency conflict
        if (!ProductExists(product.Id))
        {
            return NotFound();
        }
        else
        {
            // Show conflict resolution UI
            ModelState.AddModelError("", "The record was modified by another user.");
            return View(product);
        }
    }
}
```

### [ConcurrencyCheck] - Concurrency Tokens

ConcurrencyCheck marks properties to be included in concurrency checking. Unlike Timestamp, which uses a single row version, ConcurrencyCheck includes specific columns in the WHERE clause during updates.

```csharp
public class Product
{
    public int Id { get; set; }
    
    [ConcurrencyCheck]
    public string Name { get; set; }
    
    [ConcurrencyCheck]
    public decimal Price { get; set; }
    
    public string Description { get; set; }
}

// The generated UPDATE will include:
// WHERE Id = @p0 AND Name = @p1 AND Price = @p2
```

### [NotMapped] - Excluding from Database

NotMapped excludes a property or class from being mapped to the database. Use this for computed properties, properties that don't need persistence, or classes that shouldn't have their own table.

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
    
    // Not stored in database
    [NotMapped]
    public decimal PriceWithTax => Price * 1.1m;
    
    [NotMapped]
    public string DisplayName => $"{Name} - {Price:C}";
    
    [NotMapped]
    public bool IsSelected { get; set; } // UI-only property
    
    [NotMapped]
    public IFormFile? ImageFile { get; set; } // Upload file, not stored
    
    // Calculated at runtime
    [NotMapped]
    public int StockStatus => StockQuantity switch
    {
        0 => 0, // Out of stock
        < 10 => 1, // Low stock
        _ => 2 // In stock
    };
    
    public int StockQuantity { get; set; }
}

// NotMapped on class - base class without table
[NotMapped]
public abstract class BaseEntity
{
    public DateTime CreatedAt { get; set; }
    public string CreatedBy { get; set; }
    public DateTime? ModifiedAt { get; set; }
    public string ModifiedBy { get; set; }
}

// Derived classes get their own tables with BaseEntity columns
public class Product : BaseEntity
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```

### [Index] - Creating Indexes (EF Core 5+)

In EF Core 5 and later, the Index attribute creates database indexes. This provides a declarative way to define indexes directly on entity classes.

```csharp
// Single-column index
[Index(nameof(Email))]
public class User
{
    public int Id { get; set; }
    
    public string Email { get; set; }
    public string Username { get; set; }
}

// Multiple indexes
[Index(nameof(Email), IsUnique = true)]
[Index(nameof(Username), IsUnique = true)]
public class User
{
    public int Id { get; set; }
    
    public string Email { get; set; }
    public string Username { get; set; }
}

// Composite index
[Index(nameof(LastName), nameof(FirstName))]
public class Customer
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
}

// Index with name and options
[Index(nameof(Code), Name = "IX_Products_Code", IsUnique = true)]
public class Product
{
    public int Id { get; set; }
    public string Code { get; set; }
}

// AllProperties index on multiple columns
[Index(nameof(CategoryId), nameof(Name), IsUnique = true)]
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int CategoryId { get; set; }
}
```

---

## 7. Chapter 5: Understanding Fluent API

### What is Fluent API?

Fluent API is Entity Framework Core's programmatic approach to model configuration. Instead of decorating classes with attributes, you configure the model in the OnModelCreating method of your DbContext. The term "fluent" refers to the method chaining pattern that makes the code readable and discoverable through IntelliSense.

Fluent API provides capabilities beyond what Data Annotations can achieve. While attributes are limited to what can be expressed declaratively on classes and properties, Fluent API can configure any aspect of the model, including complex scenarios like many-to-many relationships with payload, owned types, query filters, and inheritance strategies.

### OnModelCreating Method

The OnModelCreating method is where all Fluent API configuration occurs. Override this method in your DbContext to configure entities, properties, relationships, and the overall model structure.

```csharp
public class ApplicationDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }
    public DbSet<Category> Categories { get; set; }
    public DbSet<Customer> Customers { get; set; }
    public DbSet<Order> Orders { get; set; }
    
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
    
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);
        
        // All Fluent API configuration goes here
        ConfigureProducts(modelBuilder);
        ConfigureCategories(modelBuilder);
        ConfigureCustomers(modelBuilder);
        ConfigureOrders(modelBuilder);
        
        // Or apply configuration from separate classes
        modelBuilder.ApplyConfigurationsFromAssembly(typeof(ApplicationDbContext).Assembly);
    }
    
    private void ConfigureProducts(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Product>(entity =>
        {
            entity.ToTable("Products", "Inventory");
            entity.HasKey(e => e.Id);
            entity.Property(e => e.Name).IsRequired().HasMaxLength(200);
            // More configuration...
        });
    }
}
```

### ModelBuilder and EntityTypeBuilder

The ModelBuilder is the entry point for Fluent API configuration. It provides methods to configure entities (Entity<T>), globally apply configurations, and set model-wide defaults. The EntityTypeBuilder returned by Entity<T> provides all configuration options for a specific entity type.

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    // Get entity type builder
    var productEntity = modelBuilder.Entity<Product>();
    
    // Configure using method chaining
    productEntity
        .ToTable("Products", schema: "Inventory")
        .HasKey(p => p.Id);
    
    productEntity
        .Property(p => p.Name)
        .IsRequired()
        .HasMaxLength(200)
        .HasColumnName("ProductName");
    
    // Alternative: inline configuration
    modelBuilder.Entity<Product>(entity =>
    {
        entity.ToTable("Products", "Inventory");
        entity.HasKey(e => e.Id);
        
        entity.Property(e => e.Name)
            .IsRequired()
            .HasMaxLength(200);
        
        entity.Property(e => e.Price)
            .HasPrecision(18, 2);
        
        entity.HasOne(e => e.Category)
            .WithMany(c => c.Products)
            .HasForeignKey(e => e.CategoryId);
    });
}
```

### Separating Configuration into Classes

For better organization, especially in larger applications, move entity configurations into separate classes implementing IEntityTypeConfiguration<T>.

```csharp
// Separate configuration class
public class ProductConfiguration : IEntityTypeConfiguration<Product>
{
    public void Configure(EntityTypeBuilder<Product> builder)
    {
        builder.ToTable("Products", "Inventory");
        
        builder.HasKey(p => p.Id);
        
        builder.Property(p => p.Name)
            .IsRequired()
            .HasMaxLength(200)
            .HasColumnName("ProductName");
        
        builder.Property(p => p.Price)
            .HasPrecision(18, 2);
        
        builder.Property(p => e.Description)
            .HasMaxLength(2000)
            .IsRequired(false);
        
        builder.HasOne(p => p.Category)
            .WithMany(c => c.Products)
            .HasForeignKey(p => p.CategoryId)
            .OnDelete(DeleteBehavior.Restrict);
        
        builder.HasIndex(p => p.Name)
            .HasDatabaseName("IX_Products_Name");
        
        builder.HasIndex(p => new { p.CategoryId, p.Name })
            .IsUnique()
            .HasDatabaseName("IX_Products_Category_Name");
    }
}

// Apply in OnModelCreating
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    // Apply specific configuration
    modelBuilder.ApplyConfiguration(new ProductConfiguration());
    
    // Or apply all configurations from assembly
    modelBuilder.ApplyConfigurationsFromAssembly(typeof(ApplicationDbContext).Assembly);
}
```

### Why Use Fluent API?

Fluent API provides several advantages over Data Annotations for database configuration. It keeps your entity classes clean and focused on domain concerns. It allows configuration of classes you don't own or can't modify. It provides access to all EF Core capabilities, including those not expressible with attributes. It enables better organization through separate configuration classes.

Data Annotations remain valuable for validation and display purposes, which Fluent API doesn't address. A common pattern is to use Data Annotations for validation/display and Fluent API for database mapping, keeping each approach focused on its strengths.

---

## 8. Chapter 6: Fluent API - Property Configuration

### Basic Property Configuration

Properties are configured using the Property method on EntityTypeBuilder. This returns a PropertyBuilder that provides methods for configuring all aspects of the column mapping.

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Product>(entity =>
    {
        // Basic property configuration
        entity.Property(p => p.Name)
            .IsRequired()                          // NOT NULL
            .HasMaxLength(200)                      // NVARCHAR(200)
            .HasColumnName("ProductName");          // Column name
        
        // Optional property
        entity.Property(p => p.Description)
            .IsRequired(false)                      // NULL
            .HasMaxLength(2000);
        
        // With default value
        entity.Property(p => p.IsActive)
            .HasDefaultValue(true);
        
        entity.Property(p => p.CreatedAt)
            .HasDefaultValueSql("GETDATE()");
        
        // Computed column
        entity.Property(p => p.DisplayName)
            .HasComputedColumnSql("[Name] + ' - $' + CAST([Price] AS VARCHAR(20))");
    });
}
```

### Column Type Configuration

Fluent API provides fine-grained control over column types, including precision for decimals, max length for strings, and specific database types.

```csharp
modelBuilder.Entity<Product>(entity =>
{
    // String with max length
    entity.Property(p => p.Name)
        .HasMaxLength(200)
        .IsUnicode(false);                // VARCHAR instead of NVARCHAR
    
    // Unicode string
    entity.Property(p => p.Description)
        .IsUnicode(true)                   // NVARCHAR
    
    // Decimal with precision
    entity.Property(p => p.Price)
        .HasPrecision(18, 4);             // DECIMAL(18,4)
    
    // Alternative usingHasColumnType
    entity.Property(p => p.Price)
        .HasColumnType("decimal(18, 4)");
    
    // Specific SQL Server types
    entity.Property(p => e.Amount)
        .HasColumnType("money");
    
    entity.Property(p => p.CreatedDate)
        .HasColumnType("date");
    
    entity.Property(p => p.CreatedAt)
        .HasColumnType("datetime2(7)");
    
    entity.Property(p => p.OpenTime)
        .HasColumnType("time");
    
    entity.Property(p => p.BinaryData)
        .HasColumnType("varbinary(max)");
    
    entity.Property(p => p.JsonData)
        .HasColumnType("json");           // SQL Server 2019+
    
    // Fixed-length strings
    entity.Property(p => p.Code)
        .HasColumnType("char(10)");
    
    entity.Property(p => p.ISBN)
        .HasColumnType("nchar(13)");
});
```

### Value Generation Configuration

Control how values are generated for properties, including identity columns, sequences, and computed columns.

```csharp
modelBuilder.Entity<Product>(entity =>
{
    // Identity column (auto-increment)
    entity.Property(p => p.Id)
        .ValueGeneratedOnAdd();
    
    // Computed on add (e.g., GUID)
    entity.Property(p => p.Code)
        .ValueGeneratedOnAdd()
        .HasDefaultValueSql("NEWID()");
    
    // Computed on add or update
    entity.Property(p => p.LastModified)
        .ValueGeneratedOnAddOrUpdate()
        .HasDefaultValueSql("GETDATE()");
    
    // No value generation (application sets it)
    entity.Property(p => p.OrderNumber)
        .ValueGeneratedNever();
    
    // Computed column
    entity.Property(p => p.TotalPrice)
        .HasComputedColumnSql("[Price] * [Quantity]");
    
    // Computed column stored
    entity.Property(p => p.TotalPrice)
        .HasComputedColumnSql("[Price] * [Quantity]", stored: true);
    
    // Sequence for invoice numbers
    entity.Property(p => p.InvoiceNumber)
        .HasDefaultValueSql("NEXT VALUE FOR InvoiceNumberSequence");
});

// Create sequence separately
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.HasSequence<int>("InvoiceNumberSequence", schema: "Sales")
        .StartsAt(1000)
        .IncrementsBy(1);
    
    modelBuilder.Entity<Invoice>(entity =>
    {
        entity.Property(i => i.Number)
            .HasDefaultValueSql("NEXT VALUE FOR Sales.InvoiceNumberSequence");
    });
}
```

### Default Values

Configure default values for columns that apply when no value is provided during insert.

```csharp
modelBuilder.Entity<Product>(entity =>
{
    // Constant default value
    entity.Property(p => p.IsActive)
        .HasDefaultValue(true);
    
    entity.Property(p => p.StockQuantity)
        .HasDefaultValue(0);
    
    // SQL default value
    entity.Property(p => p.CreatedAt)
        .HasDefaultValueSql("GETDATE()");
    
    entity.Property(p => p.CreatedAtUtc)
        .HasDefaultValueSql("SYSUTCDATETIME()");
    
    // Null default
    entity.Property(p => p.Notes)
        .HasDefaultValue(null);
    
    // Default for GUID
    entity.Property(p => p.UniqueId)
        .HasDefaultValueSql("NEWID()");
    
    // Default for date only
    entity.Property(p => p.ExpireDate)
        .HasDefaultValueSql("DATEADD(year, 1, GETDATE())");
});
```

### Concurrency Tokens

Configure properties for optimistic concurrency control using either row versions or individual concurrency tokens.

```csharp
modelBuilder.Entity<Product>(entity =>
{
    // Row version (timestamp)
    entity.Property(p => p.RowVersion)
        .IsRowVersion()
        .IsConcurrencyToken();
    
    // Or using alternative syntax
    entity.Property(p => p.Version)
        .HasColumnType("rowversion")
        .ValueGeneratedOnAddOrUpdate()
        .IsConcurrencyToken();
    
    // Individual concurrency tokens
    entity.Property(p => p.Name)
        .IsConcurrencyToken();
    
    entity.Property(p => p.Price)
        .IsConcurrencyToken();
});
```

### Property Annotations and Comments

Add documentation to your database schema through comments and annotations.

```csharp
modelBuilder.Entity<Product>(entity =>
{
    // Column comment (SQL Server)
    entity.Property(p => p.Price)
        .HasComment("The retail price of the product in USD");
    
    entity.Property(p => p.SKU)
        .HasComment("Stock Keeping Unit - unique identifier for inventory tracking");
    
    // Annotations for metadata
    entity.Property(p => p.Name)
        .HasAnnotation("DisplayHint", "ProductName");
});
```

### Property Configuration Methods Reference

| Method | Purpose |
|--------|---------|
| HasColumnName(string) | Sets the column name |
| HasColumnType(string) | Sets the database column type |
| HasMaxLength(int) | Sets max length for strings/byte arrays |
| HasPrecision(int, int) | Sets precision and scale for decimals |
| IsRequired(bool) | Makes column NOT NULL |
| IsUnicode(bool) | Sets Unicode (NVARCHAR) or ANSI (VARCHAR) |
| HasDefaultValue(object) | Sets default value |
| HasDefaultValueSql(string) | Sets SQL default expression |
| HasComputedColumnSql(string) | Creates computed column |
| ValueGeneratedOnAdd() | Auto-generate on insert |
| ValueGeneratedOnUpdate() | Auto-generate on update |
| ValueGeneratedOnAddOrUpdate() | Auto-generate on both |
| ValueGeneratedNever() | No auto-generation |
| IsRowVersion() | Configures as row version |
| IsConcurrencyToken() | Marks as concurrency token |
| HasComment(string) | Adds column comment |
| HasAnnotation(string, object) | Adds metadata annotation |

---

## 9. Chapter 7: Fluent API - Relationships

### Understanding Relationship Configuration

Relationships between entities are configured using navigation properties and foreign keys. Fluent API provides complete control over how relationships are mapped, including cascade delete behavior, foreign key constraints, and relationship multiplicity.

### One-to-Many Relationships

One-to-many is the most common relationship pattern. One entity (the principal) has a collection of related entities (the dependent), while each dependent references one principal.

```csharp
// Basic one-to-many
modelBuilder.Entity<Product>(entity =>
{
    entity.HasOne(p => p.Category)          // Product has one Category
        .WithMany(c => c.Products)          // Category has many Products
        .HasForeignKey(p => p.CategoryId);  // Foreign key property
});

// With cascade delete
modelBuilder.Entity<Order>(entity =>
{
    entity.HasOne(o => o.Customer)
        .WithMany(c => c.Orders)
        .HasForeignKey(o => o.CustomerId)
        .OnDelete(DeleteBehavior.Cascade);  // Delete orders when customer deleted
});

// With restrict delete
modelBuilder.Entity<Product>(entity =>
{
    entity.HasOne(p => p.Category)
        .WithMany(c => c.Products)
        .HasForeignKey(p => p.CategoryId)
        .OnDelete(DeleteBehavior.Restrict); // Prevent deletion if products exist
});

// With set null
modelBuilder.Entity<Order>(entity =>
{
    entity.HasOne(o => o.SalesRep)
        .WithMany(e => e.AssignedOrders)
        .HasForeignKey(o => o.SalesRepId)
        .OnDelete(DeleteBehavior.SetNull);  // Set SalesRepId to null on deletion
});

// With no action
modelBuilder.Entity<OrderItem>(entity =>
{
    entity.HasOne(oi => oi.Product)
        .WithMany(p => p.OrderItems)
        .HasForeignKey(oi => oi.ProductId)
        .OnDelete(DeleteBehavior.NoAction);
});

// Principal entity example
public class Category
{
    public int Id { get; set; }
    public string Name { get; set; }
    
    // Collection navigation
    public ICollection<Product> Products { get; set; }
}

// Dependent entity example
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    
    // Reference navigation
    public Category Category { get; set; }
    
    // Foreign key
    public int CategoryId { get; set; }
}
```

### One-to-One Relationships

One-to-one relationships require explicit configuration because EF Core cannot determine which entity is the principal. Configure the foreign key and uniqueness constraint.

```csharp
// One-to-one with explicit foreign key
modelBuilder.Entity<User>(entity =>
{
    entity.HasOne(u => u.Profile)
        .WithOne(p => p.User)
        .HasForeignKey<UserProfile>(p => p.UserId);
});

// Entities
public class User
{
    public int Id { get; set; }
    public string Username { get; set; }
    
    public UserProfile Profile { get; set; }
}

public class UserProfile
{
    public int Id { get; set; }
    public string Bio { get; set; }
    
    // Both primary key and foreign key
    public int UserId { get; set; }
    public User User { get; set; }
}

// One-to-one with separate foreign key
modelBuilder.Entity<Order>(entity =>
{
    entity.HasOne(o => o.ShippingInfo)
        .WithOne(s => s.Order)
        .HasForeignKey<ShippingInfo>(s => s.OrderId);
});

// Optional one-to-one
modelBuilder.Entity<Customer>(entity =>
{
    entity.HasOne(c => c.Preferences)
        .WithOne(p => p.Customer)
        .HasForeignKey<CustomerPreferences>(p => p.CustomerId)
        .OnDelete(DeleteBehavior.Cascade);
});

// Principal with navigation only
modelBuilder.Entity<Invoice>(entity =>
{
    entity.HasOne(i => i.Payment)
        .WithOne()
        .HasForeignKey<Payment>(p => p.InvoiceId);
});
```

### Many-to-Many Relationships

EF Core 5+ supports direct many-to-many configuration without requiring a join entity. For relationships with payload (additional properties on the join), explicitly configure the join entity.

```csharp
// Simple many-to-many (EF Core 5+)
modelBuilder.Entity<Student>(entity =>
{
    entity.HasMany(s => s.Courses)
        .WithMany(c => c.Students)
        .UsingEntity(j =>
        {
            j.ToTable("StudentCourses");
            j.Property("JoinedAt").HasDefaultValueSql("GETDATE()");
        });
});

public class Student
{
    public int Id { get; set; }
    public string Name { get; set; }
    
    public ICollection<Course> Courses { get; set; }
}

public class Course
{
    public int Id { get; set; }
    public string Title { get; set; }
    
    public ICollection<Student> Students { get; set; }
}

// Many-to-many with payload (explicit join entity)
modelBuilder.Entity<Book>(entity =>
{
    entity.HasMany(b => b.Authors)
        .WithMany(a => a.Books)
        .UsingEntity<BookAuthor>(
            j => j.HasOne(ba => ba.Author)
                .WithMany(a => a.BookAuthors)
                .HasForeignKey(ba => ba.AuthorId),
            j => j.HasOne(ba => ba.Book)
                .WithMany(b => b.BookAuthors)
                .HasForeignKey(ba => ba.BookId),
            j =>
            {
                j.HasKey(ba => new { ba.BookId, ba.AuthorId });
                j.Property(ba => ba.Role).HasMaxLength(50);
                j.Property(ba => ba.ContributionOrder);
            });
});

public class Book
{
    public int Id { get; set; }
    public string Title { get; set; }
    
    public ICollection<Author> Authors { get; set; }
    public ICollection<BookAuthor> BookAuthors { get; set; }
}

public class Author
{
    public int Id { get; set; }
    public string Name { get; set; }
    
    public ICollection<Book> Books { get; set; }
    public ICollection<BookAuthor> BookAuthors { get; set; }
}

public class BookAuthor
{
    public int BookId { get; set; }
    public int AuthorId { get; set; }
    
    public Book Book { get; set; }
    public Author Author { get; set; }
    
    // Payload properties
    public string Role { get; set; }           // "Author", "Co-Author", "Editor"
    public int ContributionOrder { get; set; } // 1 = primary, 2 = secondary, etc.
}
```

### Self-Referencing Relationships

Entities can have relationships to themselves, useful for hierarchical data like categories, employees, and organizational structures.

```csharp
// Self-referencing one-to-many (hierarchical)
modelBuilder.Entity<Category>(entity =>
{
    entity.HasOne(c => c.ParentCategory)
        .WithMany(c => c.SubCategories)
        .HasForeignKey(c => c.ParentCategoryId)
        .OnDelete(DeleteBehavior.Restrict);
});

public class Category
{
    public int Id { get; set; }
    public string Name { get; set; }
    
    public int? ParentCategoryId { get; set; }
    public Category ParentCategory { get; set; }
    
    public ICollection<Category> SubCategories { get; set; }
}

// Self-referencing many-to-many (followers)
modelBuilder.Entity<User>(entity =>
{
    entity.HasMany(u => u.Followers)
        .WithMany(u => u.Following)
        .UsingEntity<Dictionary<string, object>>(
            "UserFollowers",
            j => j.HasOne<User>().WithMany().HasForeignKey("FollowerId"),
            j => j.HasOne<User>().WithMany().HasForeignKey("FollowingId"),
            j => j.HasKey("FollowerId", "FollowingId"));
});

public class User
{
    public int Id { get; set; }
    public string Username { get; set; }
    
    public ICollection<User> Followers { get; set; }  // Users following this user
    public ICollection<User> Following { get; set; }  // Users this user follows
}
```

### Multiple Relationships Between Entities

When two entities have multiple relationships, use the InverseProperty attribute or fluent configuration to distinguish them.

```csharp
// Multiple relationships using Fluent API
modelBuilder.Entity<Order>(entity =>
{
    // Relationship to customer who placed the order
    entity.HasOne(o => o.Customer)
        .WithMany(c => c.Orders)
        .HasForeignKey(o => o.CustomerId)
        .OnDelete(DeleteBehavior.Cascade);
    
    // Relationship to sales representative
    entity.HasOne(o => o.SalesRep)
        .WithMany(e => e.SalesOrders)
        .HasForeignKey(o => o.SalesRepId)
        .OnDelete(DeleteBehavior.SetNull);
    
    // Relationship to shipping manager
    entity.HasOne(o => o.ShippingManager)
        .WithMany(e => e.ShippingOrders)
        .HasForeignKey(o => o.ShippingManagerId)
        .OnDelete(DeleteBehavior.NoAction);
});

public class Order
{
    public int Id { get; set; }
    
    public int CustomerId { get; set; }
    public Customer Customer { get; set; }
    
    public int? SalesRepId { get; set; }
    public Employee SalesRep { get; set; }
    
    public int? ShippingManagerId { get; set; }
    public Employee ShippingManager { get; set; }
}

public class Employee
{
    public int Id { get; set; }
    public string Name { get; set; }
    
    public ICollection<Order> SalesOrders { get; set; }
    public ICollection<Order> ShippingOrders { get; set; }
}
```

### Cascade Delete Behaviors

EF Core supports several cascade delete behaviors, each appropriate for different scenarios:

```csharp
public enum DeleteBehavior
{
    Cascade,      // Delete dependents when principal is deleted
    Restrict,     // Prevent deletion if dependents exist (throws exception)
    SetNull,      // Set foreign key to null in dependents
    ClientSetNull, // Set foreign key to null in tracked entities only
    NoAction      // No action; database constraint determines behavior
}

// Example configurations
modelBuilder.Entity<OrderItem>(entity =>
{
    // Cascade - order items deleted with order
    entity.HasOne(oi => oi.Order)
        .WithMany(o => o.OrderItems)
        .HasForeignKey(oi => oi.OrderId)
        .OnDelete(DeleteBehavior.Cascade);
});

modelBuilder.Entity<Product>(entity =>
{
    // Restrict - can't delete category with products
    entity.HasOne(p => p.Category)
        .WithMany(c => c.Products)
        .HasForeignKey(p => p.CategoryId)
        .OnDelete(DeleteBehavior.Restrict);
});

modelBuilder.Entity<Assignment>(entity =>
{
    // SetNull - unassign tasks when employee deleted
    entity.HasOne(a => a.AssignedTo)
        .WithMany(e => e.Assignments)
        .HasForeignKey(a => a.AssignedToId)
        .OnDelete(DeleteBehavior.SetNull);
});
```

---

## 10. Chapter 8: Fluent API - Keys and Indexes

### Primary Key Configuration

Configure primary keys including single keys, composite keys, and non-conventional key names.

```csharp
// Simple primary key
modelBuilder.Entity<Product>(entity =>
{
    entity.HasKey(p => p.Id);
    
    // Alternative syntax
    entity.Property(p => p.Id).HasColumnName("ProductId");
});

// Composite primary key
modelBuilder.Entity<OrderItem>(entity =>
{
    entity.HasKey(oi => new { oi.OrderId, oi.ProductId });
});

// Non-clustered primary key
modelBuilder.Entity<Product>(entity =>
{
    entity.HasKey(p => p.Id)
        .IsClustered(false);
});

// Primary key with custom name
modelBuilder.Entity<Product>(entity =>
{
    entity.HasKey(p => p.Id)
        .HasName("PK_Products_CustomName");
});

// Anonymous key (shadow property)
modelBuilder.Entity<Product>(entity =>
{
    entity.HasKey("Id");
});
```

### Alternate Keys

Alternate keys serve as unique identifiers besides the primary key and are used as targets for relationships.

```csharp
// Alternate key for unique constraint
modelBuilder.Entity<User>(entity =>
{
    entity.HasAlternateKey(u => u.Email);
    
    // Creates unique index on Email column
});

// Composite alternate key
modelBuilder.Entity<Product>(entity =>
{
    entity.HasAlternateKey(p => new { p.CategoryId, p.SKU });
});

// Alternate key with index configuration
modelBuilder.Entity<User>(entity =>
{
    entity.HasAlternateKey(u => u.Username)
        .HasName("AK_Users_Username");
});

// Using alternate key in relationship
modelBuilder.Entity<Order>(entity =>
{
    entity.HasOne(o => o.Customer)
        .WithMany(c => c.Orders)
        .HasPrincipalKey(c => c.Email)  // Uses Email as FK target instead of Id
        .HasForeignKey(o => o.CustomerEmail);
});
```

### Index Configuration

Indexes improve query performance. Configure single-column, composite, unique, and filtered indexes.

```csharp
// Single-column index
modelBuilder.Entity<Product>(entity =>
{
    entity.HasIndex(p => p.Name);
});

// Named index
modelBuilder.Entity<Product>(entity =>
{
    entity.HasIndex(p => p.Name)
        .HasDatabaseName("IX_Products_Name");
});

// Unique index
modelBuilder.Entity<User>(entity =>
{
    entity.HasIndex(u => u.Email)
        .IsUnique();
});

// Composite index
modelBuilder.Entity<Customer>(entity =>
{
    entity.HasIndex(c => new { c.LastName, c.FirstName });
});

// Unique composite index
modelBuilder.Entity<Product>(entity =>
{
    entity.HasIndex(p => new { p.CategoryId, p.SKU })
        .IsUnique()
        .HasDatabaseName("UX_Products_Category_SKU");
});

// Index with filter (partial index)
modelBuilder.Entity<User>(entity =>
{
    entity.HasIndex(u => u.Email)
        .IsUnique()
        .HasFilter("[Email] IS NOT NULL");
});

// Soft delete filter
modelBuilder.Entity<Product>(entity =>
{
    entity.HasIndex(p => p.Name)
        .HasFilter("[IsDeleted] = 0");
});

// Index with included columns (SQL Server)
modelBuilder.Entity<Product>(entity =>
{
    entity.HasIndex(p => p.CategoryId)
        .IncludeProperties(p => new { p.Name, p.Price });
});

// Clustered index (non-primary key)
modelBuilder.Entity<OrderItem>(entity =>
{
    entity.HasIndex(oi => oi.OrderId)
        .IsClustered();
});

// Fill factor for index
modelBuilder.Entity<Product>(entity =>
{
    entity.HasIndex(p => p.Name)
        .HasFillFactor(80); // 80% fill factor
});

// Multiple indexes
modelBuilder.Entity<Product>(entity =>
{
    entity.HasIndex(p => p.Name);
    entity.HasIndex(p => p.SKU).IsUnique();
    entity.HasIndex(p => p.CategoryId);
    entity.HasIndex(p => new { p.CategoryId, p.Price });
});
```

### Index Methods Reference

| Method | Purpose |
|--------|---------|
| HasIndex(expression) | Creates an index |
| HasDatabaseName(string) | Sets the index name in database |
| IsUnique(bool) | Makes index unique |
| HasFilter(string) | Adds WHERE clause filter |
| IsClustered(bool) | Makes index clustered |
| IncludeProperties(...) | Includes additional columns (SQL Server) |
| HasFillFactor(int) | Sets fill factor for index |

---

## 11. Chapter 9: Fluent API - Inheritance Mapping

### Table Per Hierarchy (TPH)

TPH is the default inheritance strategy in EF Core. All types in a hierarchy are stored in a single table with a discriminator column to identify the type.

```csharp
// Base class
public abstract class Payment
{
    public int Id { get; set; }
    public decimal Amount { get; set; }
    public DateTime PaymentDate { get; set; }
}

// Derived classes
public class CreditCardPayment : Payment
{
    public string CardNumber { get; set; }
    public string CardHolderName { get; set; }
    public DateTime ExpiryDate { get; set; }
}

public class PayPalPayment : Payment
{
    public string PayPalEmail { get; set; }
    public string TransactionId { get; set; }
}

public class BankTransferPayment : Payment
{
    public string BankName { get; set; }
    public string AccountNumber { get; set; }
    public string RoutingNumber { get; set; }
}

// TPH Configuration
modelBuilder.Entity<Payment>(entity =>
{
    entity.ToTable("Payments");
    
    entity.Property(p => p.Amount).HasPrecision(18, 2);
    
    entity.HasDiscriminator<string>("PaymentType")
        .HasValue<CreditCardPayment>("CreditCard")
        .HasValue<PayPalPayment>("PayPal")
        .HasValue<BankTransferPayment>("BankTransfer");
    
    // Discriminator column configuration
    entity.Property("PaymentType")
        .HasMaxLength(20)
        .IsRequired();
});

// Or using default discriminator (class names)
modelBuilder.Entity<Payment>(entity =>
{
    entity.HasDiscriminator();
    // Uses "Discriminator" column with class names as values
});
```

### Table Per Type (TPT)

TPT stores each type in its own table. Base class properties go in the base table, and derived class properties go in separate tables linked by foreign key.

```csharp
// TPT Configuration
modelBuilder.Entity<Payment>(entity =>
{
    entity.ToTable("Payments");
});

modelBuilder.Entity<CreditCardPayment>(entity =>
{
    entity.ToTable("CreditCardPayments");
});

modelBuilder.Entity<PayPalPayment>(entity =>
{
    entity.ToTable("PayPalPayments");
});

modelBuilder.Entity<BankTransferPayment>(entity =>
{
    entity.ToTable("BankTransferPayments");
});

// This creates:
// Payments table: Id, Amount, PaymentDate
// CreditCardPayments table: Id (FK to Payments), CardNumber, CardHolderName, ExpiryDate
// PayPalPayments table: Id (FK to Payments), PayPalEmail, TransactionId
// BankTransferPayments table: Id (FK to Payments), BankName, AccountNumber, RoutingNumber
```

### Table Per Concrete Type (TPC)

TPC (EF Core 7+) stores each concrete type in its own complete table, duplicating base class properties in each table. There's no base table.

```csharp
// TPC Configuration (EF Core 7+)
modelBuilder.Entity<Payment>(entity =>
{
    entity.UseTpcMappingStrategy();
});

// Or explicit table configuration
modelBuilder.Entity<Payment>(entity =>
{
    entity.UseTpcMappingStrategy();
    entity.Property(p => p.Amount).HasPrecision(18, 2);
});

modelBuilder.Entity<CreditCardPayment>(entity =>
{
    entity.ToTable("CreditCardPayments");
    // Contains: Id, Amount, PaymentDate, CardNumber, CardHolderName, ExpiryDate
});

modelBuilder.Entity<PayPalPayment>(entity =>
{
    entity.ToTable("PayPalPayments");
    // Contains: Id, Amount, PaymentDate, PayPalEmail, TransactionId
});

modelBuilder.Entity<BankTransferPayment>(entity =>
{
    entity.ToTable("BankTransferPayments");
    // Contains: Id, Amount, PaymentDate, BankName, AccountNumber, RoutingNumber
});
```

### Choosing an Inheritance Strategy

Each strategy has trade-offs:

| Strategy | Tables | Performance | Complexity | Best For |
|----------|--------|-------------|------------|----------|
| TPH | 1 table | Best for reads | Simple | Most common scenarios |
| TPT | N tables | Slower (joins) | Moderate | Shared base with few derived |
| TPC | N tables | Fast inserts | Moderate | Few shared queries |

```csharp
// Example choosing TPH for common case
modelBuilder.Entity<Payment>(entity =>
{
    // TPH is default, but let's be explicit
    entity.UseTphMappingStrategy();
    
    entity.HasDiscriminator<string>("PaymentType")
        .HasValue<CreditCardPayment>("CC")
        .HasValue<PayPalPayment>("PP")
        .HasValue<BankTransferPayment>("BT");
});

// Example choosing TPT for regulatory requirements
modelBuilder.Entity<Transaction>(entity =>
{
    entity.UseTptMappingStrategy();
    // Each transaction type in separate table for audit compliance
});
```

---

## 12. Chapter 10: Advanced Scenarios

### Owned Types (Value Objects)

Owned types are entities that don't have their own identity—they're owned by another entity. This implements the value object pattern from domain-driven design.

```csharp
// Owned type
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public string State { get; set; }
    public string PostalCode { get; set; }
    public string Country { get; set; }
}

// Entity with owned type
public class Customer
{
    public int Id { get; set; }
    public string Name { get; set; }
    
    public Address BillingAddress { get; set; }
    public Address ShippingAddress { get; set; }
}

// Configuration
modelBuilder.Entity<Customer>(entity =>
{
    entity.OwnsOne(c => c.BillingAddress, ba =>
    {
        ba.WithOwner();
        ba.Property(a => a.Street).HasMaxLength(200);
        ba.Property(a => a.City).HasMaxLength(100);
        // Column names: BillingAddress_Street, BillingAddress_City, etc.
        
        // Custom column names
        ba.Property(a => a.Street).HasColumnName("BillingStreet");
        ba.Property(a => a.City).HasColumnName("BillingCity");
        ba.Property(a => a.State).HasColumnName("BillingState");
        ba.Property(a => a.PostalCode).HasColumnName("BillingPostalCode");
    });
    
    entity.OwnsOne(c => c.ShippingAddress, sa =>
    {
        sa.Property(a => a.Street).HasColumnName("ShippingStreet");
        sa.Property(a => a.City).HasColumnName("ShippingCity");
        sa.Property(a => a.State).HasColumnName("ShippingState");
        sa.Property(a => a.PostalCode).HasColumnName("ShippingPostalCode");
    });
});

// Owned type collection
public class Order
{
    public int Id { get; set; }
    public ICollection<OrderItem> Items { get; set; }
}

public class OrderItem
{
    public string ProductName { get; set; }
    public int Quantity { get; set; }
    public decimal UnitPrice { get; set; }
}

modelBuilder.Entity<Order>(entity =>
{
    entity.OwnsMany(o => o.Items, oi =>
    {
        oi.WithOwner().HasForeignKey("OrderId");
        oi.Property<int>("Id").ValueGeneratedOnAdd();
        oi.HasKey("Id");
    });
});

// Owned type in separate table
modelBuilder.Entity<Customer>(entity =>
{
    entity.OwnsOne(c => c.BillingAddress, ba =>
    {
        ba.ToTable("CustomerAddresses");
        ba.WithOwner().HasForeignKey("CustomerId");
        ba.Property<int>("Id").ValueGeneratedOnAdd();
        ba.HasKey("Id");
    });
});
```

### Shadow Properties

Shadow properties are properties defined in the model but not on the entity class. They're useful for infrastructure concerns like audit fields that shouldn't be part of the domain model.

```csharp
// Shadow property definition
modelBuilder.Entity<Product>(entity =>
{
    entity.Property<DateTime>("CreatedAt");
    entity.Property<string>("CreatedBy");
    entity.Property<DateTime?>("ModifiedAt");
    entity.Property<string>("ModifiedBy");
});

// Setting shadow property values
public override int SaveChanges()
{
    var entries = ChangeTracker.Entries();
    
    foreach (var entry in entries.Where(e => e.State == EntityState.Added))
    {
        entry.Property("CreatedAt").CurrentValue = DateTime.UtcNow;
        entry.Property("CreatedBy").CurrentValue = GetCurrentUserId();
    }
    
    foreach (var entry in entries.Where(e => e.State == EntityState.Modified))
    {
        entry.Property("ModifiedAt").CurrentValue = DateTime.UtcNow;
        entry.Property("ModifiedBy").CurrentValue = GetCurrentUserId();
    }
    
    return base.SaveChanges();
}

// Querying shadow properties
var products = await _context.Products
    .OrderBy(p => EF.Property<DateTime>(p, "CreatedAt"))
    .ToListAsync();

// Or using the property in a where clause
var recentProducts = await _context.Products
    .Where(p => EF.Property<DateTime>(p, "CreatedAt") > DateTime.UtcNow.AddDays(-7))
    .ToListAsync();
```

### Global Query Filters

Query filters are automatically applied to all queries for an entity, useful for soft delete, multi-tenancy, and other cross-cutting concerns.

```csharp
// Soft delete filter
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public bool IsDeleted { get; set; }
}

modelBuilder.Entity<Product>(entity =>
{
    entity.HasQueryFilter(p => !p.IsDeleted);
});

// All queries automatically filter out deleted products
var products = await _context.Products.ToListAsync();
// Generates: SELECT * FROM Products WHERE IsDeleted = 0

// Ignore filter for specific query
var allProducts = await _context.Products
    .IgnoreQueryFilters()
    .ToListAsync();

// Multi-tenancy filter
public class TenantEntity
{
    public int Id { get; set; }
    public int TenantId { get; set; }
}

modelBuilder.Entity<TenantEntity>(entity =>
{
    entity.HasQueryFilter(e => e.TenantId == _currentTenantId);
});

// Using a field for dynamic filter value
private readonly int _currentTenantId;

public ApplicationDbContext(DbContextOptions options, ITenantProvider tenantProvider)
    : base(options)
{
    _currentTenantId = tenantProvider.GetCurrentTenantId();
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<TenantEntity>(entity =>
    {
        entity.HasQueryFilter(e => e.TenantId == _currentTenantId);
    });
}
```

### Data Seeding

Seed initial data as part of the model configuration. This data is managed by migrations.

```csharp
modelBuilder.Entity<Category>(entity =>
{
    entity.HasData(
        new Category { Id = 1, Name = "Electronics", Description = "Electronic devices and accessories" },
        new Category { Id = 2, Name = "Clothing", Description = "Apparel and fashion items" },
        new Category { Id = 3, Name = "Books", Description = "Books and publications" },
        new Category { Id = 4, Name = "Home & Garden", Description = "Home improvement and garden supplies" },
        new Category { Id = 5, Name = "Sports", Description = "Sports equipment and accessories" }
    );
});

modelBuilder.Entity<Product>(entity =>
{
    entity.HasData(
        new Product { Id = 1, Name = "Laptop", Price = 999.99m, CategoryId = 1 },
        new Product { Id = 2, Name = "Smartphone", Price = 699.99m, CategoryId = 1 },
        new Product { Id = 3, Name = "T-Shirt", Price = 19.99m, CategoryId = 2 }
    );
});

// Anonymous type seeding (doesn't require entity modification)
modelBuilder.Entity<Category>()
    .HasData(new { Id = 1, Name = "Electronics" });

// Seed from JSON file
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    var categoriesJson = File.ReadAllText("Data/categories.json");
    var categories = JsonSerializer.Deserialize<List<Category>>(categoriesJson);
    modelBuilder.Entity<Category>().HasData(categories);
}
```

### Table Splitting

Table splitting maps multiple entities to the same database table, useful when you want to separate a large entity into smaller logical units.

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
    
    public ProductDetails Details { get; set; }
}

public class ProductDetails
{
    public int Id { get; set; }
    public string Description { get; set; }
    public string Specifications { get; set; }
    public string WarrantyInfo { get; set; }
    
    public Product Product { get; set; }
}

modelBuilder.Entity<Product>(entity =>
{
    entity.ToTable("Products");
    entity.HasKey(p => p.Id);
    entity.HasOne(p => p.Details)
        .WithOne(d => d.Product)
        .HasForeignKey<ProductDetails>(d => d.Id);
});

modelBuilder.Entity<ProductDetails>(entity =>
{
    entity.ToTable("Products");  // Same table as Product
});

// Both entities map to the same table:
// Products: Id, Name, Price, Description, Specifications, WarrantyInfo
```

### Entity Splitting

Entity splitting maps a single entity to multiple tables, useful for separating frequently accessed data from rarely accessed data.

```csharp
public class Customer
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
    public string PhoneNumber { get; set; }
    public string Street { get; set; }
    public string City { get; set; }
    public string State { get; set; }
    public string PostalCode { get; set; }
}

modelBuilder.Entity<Customer>(entity =>
{
    entity.SplitToTable(
        "Customers", 
        tableBuilder =>
        {
            tableBuilder.Property(c => c.Id);
            tableBuilder.Property(c => c.Name);
            tableBuilder.Property(c => c.Email);
            tableBuilder.Property(c => c.PhoneNumber);
        });
    
    entity.SplitToTable(
        "CustomerAddresses",
        tableBuilder =>
        {
            tableBuilder.Property(c => c.Id);
            tableBuilder.Property(c => c.Street);
            tableBuilder.Property(c => c.City);
            tableBuilder.Property(c => c.State);
            tableBuilder.Property(c => c.PostalCode);
        });
});

// Creates two tables:
// Customers: Id, Name, Email, PhoneNumber
// CustomerAddresses: Id, Street, City, State, PostalCode
```

---

## 13. Chapter 11: Custom Validation

### Creating Custom Validation Attributes

When built-in validation attributes don't meet your needs, create custom validation attributes by inheriting from ValidationAttribute.

```csharp
// Simple custom validation attribute
public class MinimumAgeAttribute : ValidationAttribute
{
    private readonly int _minimumAge;
    
    public MinimumAgeAttribute(int minimumAge)
    {
        _minimumAge = minimumAge;
        ErrorMessage = $"You must be at least {minimumAge} years old.";
    }
    
    protected override ValidationResult IsValid(object value, ValidationContext validationContext)
    {
        if (value == null)
        {
            return ValidationResult.Success;
        }
        
        if (value is DateTime birthDate)
        {
            var age = DateTime.Today.Year - birthDate.Year;
            if (birthDate.Date > DateTime.Today.AddYears(-age))
            {
                age--;
            }
            
            if (age < _minimumAge)
            {
                return new ValidationResult(ErrorMessage);
            }
        }
        
        return ValidationResult.Success;
    }
}

// Usage
public class User
{
    [MinimumAge(18)]
    [DataType(DataType.Date)]
    public DateTime BirthDate { get; set; }
}

// Custom validation with multiple properties
public class DateRangeAttribute : ValidationAttribute
{
    private readonly string _startProperty;
    private readonly string _endProperty;
    
    public DateRangeAttribute(string startProperty, string endProperty)
    {
        _startProperty = startProperty;
        _endProperty = endProperty;
    }
    
    protected override ValidationResult IsValid(object value, ValidationContext validationContext)
    {
        var startProperty = validationContext.ObjectType.GetProperty(_startProperty);
        var endProperty = validationContext.ObjectType.GetProperty(_endProperty);
        
        if (startProperty == null || endProperty == null)
        {
            return new ValidationResult($"Unknown properties: {_startProperty} or {_endProperty}");
        }
        
        var startValue = (DateTime?)startProperty.GetValue(validationContext.ObjectInstance);
        var endValue = (DateTime?)endProperty.GetValue(validationContext.ObjectInstance);
        
        if (startValue.HasValue && endValue.HasValue && endValue <= startValue)
        {
            return new ValidationResult(ErrorMessage ?? "End date must be after start date");
        }
        
        return ValidationResult.Success;
    }
}

// Usage
public class Event
{
    public DateTime StartDate { get; set; }
    
    [DateRange("StartDate", "EndDate", ErrorMessage = "End date must be after start date")]
    public DateTime EndDate { get; set; }
}
```

### IValidatableObject for Complex Validation

Implement IValidatableObject when validation involves multiple properties or requires access to services.

```csharp
public class RegisterViewModel : IValidatableObject
{
    [Required]
    public string Username { get; set; }
    
    [Required]
    [EmailAddress]
    public string Email { get; set; }
    
    [Required]
    [StringLength(100, MinimumLength = 8)]
    public string Password { get; set; }
    
    [Required]
    public string ConfirmPassword { get; set; }
    
    public DateTime? BirthDate { get; set; }
    
    public IEnumerable<ValidationResult> Validate(ValidationContext validationContext)
    {
        var results = new List<ValidationResult>();
        
        // Password match validation
        if (Password != ConfirmPassword)
        {
            results.Add(new ValidationResult(
                "Password and confirmation password do not match.",
                new[] { nameof(ConfirmPassword) }));
        }
        
        // Age validation
        if (BirthDate.HasValue)
        {
            var age = DateTime.Today.Year - BirthDate.Value.Year;
            if (BirthDate.Value.Date > DateTime.Today.AddYears(-age))
            {
                age--;
            }
            
            if (age < 18)
            {
                results.Add(new ValidationResult(
                    "You must be at least 18 years old.",
                    new[] { nameof(BirthDate) }));
            }
        }
        
        // Username validation
        if (Username?.ToLower().Contains("admin") == true)
        {
            results.Add(new ValidationResult(
                "Username cannot contain 'admin'.",
                new[] { nameof(Username) }));
        }
        
        return results;
    }
}
```

### Custom Validation with Services

For validation that requires database access or other services, inject the service and use it in validation.

```csharp
// Using dependency injection with validation
public class UniqueEmailAttribute : ValidationAttribute
{
    protected override ValidationResult IsValid(object value, ValidationContext validationContext)
    {
        var dbContext = (ApplicationDbContext)validationContext.GetService(typeof(ApplicationDbContext));
        
        if (value == null || dbContext == null)
        {
            return ValidationResult.Success;
        }
        
        var email = value.ToString();
        var exists = dbContext.Users.Any(u => u.Email == email);
        
        if (exists)
        {
            return new ValidationResult(ErrorMessage ?? "Email is already registered.");
        }
        
        return ValidationResult.Success;
    }
}

// Usage
public class RegisterViewModel
{
    [Required]
    [EmailAddress]
    [UniqueEmail(ErrorMessage = "This email is already in use.")]
    public string Email { get; set; }
}

// Better approach: Validate in controller/service
public async Task<IActionResult> Register(RegisterViewModel model)
{
    if (!ModelState.IsValid)
    {
        return View(model);
    }
    
    // Service-level validation
    if (await _userService.EmailExistsAsync(model.Email))
    {
        ModelState.AddModelError("Email", "Email is already registered.");
        return View(model);
    }
    
    // Continue with registration...
}
```

### Client-Side Validation for Custom Attributes

To enable client-side validation for custom attributes, implement IClientModelValidator.

```csharp
public class MaximumFileSizeAttribute : ValidationAttribute, IClientModelValidator
{
    private readonly int _maxFileSizeInBytes;
    
    public MaximumFileSizeAttribute(int maxFileSizeInMB)
    {
        _maxFileSizeInBytes = maxFileSizeInMB * 1024 * 1024;
        ErrorMessage = $"File size cannot exceed {maxFileSizeInMB}MB.";
    }
    
    protected override ValidationResult IsValid(object value, ValidationContext validationContext)
    {
        if (value is IFormFile file)
        {
            if (file.Length > _maxFileSizeInBytes)
            {
                return new ValidationResult(ErrorMessage);
            }
        }
        
        return ValidationResult.Success;
    }
    
    public void AddValidation(ClientModelValidationContext context)
    {
        MergeAttribute(context.Attributes, "data-val", "true");
        MergeAttribute(context.Attributes, "data-val-maxfilesize", ErrorMessage);
        MergeAttribute(context.Attributes, "data-val-maxfilesize-maxsize", _maxFileSizeInBytes.ToString());
    }
    
    private void MergeAttribute(IDictionary<string, string> attributes, string key, string value)
    {
        if (!attributes.ContainsKey(key))
        {
            attributes.Add(key, value);
        }
    }
}

// JavaScript adapter (add to your site)
$.validator.addMethod('maxfilesize', function (value, element, params) {
    if (element.files.length === 0) return true;
    return element.files[0].size <= parseInt(params);
});

$.validator.unobtrusive.adapters.add('maxfilesize', ['maxsize'], function (options) {
    options.rules['maxfilesize'] = options.params.maxsize;
    options.messages['maxfilesize'] = options.message;
});
```

---

## 14. Chapter 12: Data Annotations vs Fluent API

### Comparison Summary

| Aspect | Data Annotations | Fluent API |
|--------|------------------|------------|
| Location | On entity class | In DbContext |
| Discoverability | High (visible on class) | Lower (separate location) |
| Learning curve | Easier | Steeper |
| Validation | Yes | No |
| Display configuration | Yes | No |
| Database configuration | Limited | Complete |
| Third-party entities | No | Yes |
| Complex scenarios | Limited | Full support |
| Maintainability | Good for small apps | Better for large apps |

### When to Use Data Annotations

Data Annotations are ideal when your application has straightforward requirements and you prefer keeping configuration close to the model. Use Data Annotations for validation rules, display formatting, and simple database configuration. They work well for smaller projects where all configuration is obvious from looking at the model class.

```csharp
// Good use of Data Annotations
public class ProductViewModel
{
    [Key]
    public int Id { get; set; }
    
    [Required(ErrorMessage = "Product name is required")]
    [StringLength(200, MinimumLength = 3)]
    [Display(Name = "Product Name")]
    public string Name { get; set; }
    
    [StringLength(2000)]
    [DataType(DataType.MultilineText)]
    public string Description { get; set; }
    
    [Required]
    [Range(0.01, 100000)]
    [DisplayFormat(DataFormatString = "{0:C}")]
    public decimal Price { get; set; }
    
    [Display(Name = "Category")]
    public int CategoryId { get; set; }
}
```

### When to Use Fluent API

Fluent API is necessary for complex configurations and provides complete control over the database mapping. Use Fluent API when you need to configure third-party entities, implement inheritance strategies, configure owned types, define global query filters, or when you want to keep your entity classes clean and focused on domain concerns.

```csharp
// Fluent API for complex configuration
public class ProductConfiguration : IEntityTypeConfiguration<Product>
{
    public void Configure(EntityTypeBuilder<Product> builder)
    {
        builder.ToTable("Products", "Inventory");
        
        builder.HasKey(p => p.Id);
        
        builder.Property(p => p.Name)
            .IsRequired()
            .HasMaxLength(200)
            .HasColumnName("ProductName");
        
        builder.Property(p => p.Price)
            .HasPrecision(18, 2);
        
        builder.Property(p => p.RowVersion)
            .IsRowVersion();
        
        builder.HasOne(p => p.Category)
            .WithMany(c => c.Products)
            .HasForeignKey(p => p.CategoryId)
            .OnDelete(DeleteBehavior.Restrict);
        
        builder.HasIndex(p => new { p.CategoryId, p.SKU })
            .IsUnique()
            .HasFilter("[SKU] IS NOT NULL");
        
        builder.HasQueryFilter(p => !p.IsDeleted);
        
        builder.OwnsOne(p => p.Dimensions, d =>
        {
            d.Property(x => x.Length).HasColumnName("Length");
            d.Property(x => x.Width).HasColumnName("Width");
            d.Property(x => x.Height).HasColumnName("Height");
        });
    }
}
```

### Combining Both Approaches

The recommended approach is to use Data Annotations for validation and display (which Fluent API doesn't support) and Fluent API for database mapping. This leverages the strengths of both approaches while avoiding duplication.

```csharp
// Entity with Data Annotations for validation/display
public class Product
{
    public int Id { get; set; }
    
    [Required(ErrorMessage = "Product name is required")]
    [StringLength(200, MinimumLength = 2)]
    [Display(Name = "Product Name")]
    public string Name { get; set; }
    
    [StringLength(2000)]
    [DataType(DataType.MultilineText)]
    public string Description { get; set; }
    
    [Required]
    [Range(0.01, 100000, ErrorMessage = "Price must be between {1} and {2}")]
    [DisplayFormat(DataFormatString = "{0:C}", ApplyFormatInEditMode = true)]
    public decimal Price { get; set; }
    
    [Required]
    [Display(Name = "Category")]
    public int CategoryId { get; set; }
    
    public Category Category { get; set; }
    
    // Not mapped - for display only
    [NotMapped]
    [Display(Name = "Price with Tax")]
    public decimal PriceWithTax => Price * 1.1m;
}

// Fluent API for database mapping
public class ProductConfiguration : IEntityTypeConfiguration<Product>
{
    public void Configure(EntityTypeBuilder<Product> builder)
    {
        builder.ToTable("Products", "Inventory");
        
        builder.Property(p => p.Name)
            .HasColumnName("ProductName")
            .IsRequired();
        
        builder.Property(p => p.Price)
            .HasPrecision(18, 2)
            .HasDefaultValue(0);
        
        builder.Property(p => p.CreatedAt)
            .HasDefaultValueSql("GETDATE()");
        
        builder.HasOne(p => p.Category)
            .WithMany(c => c.Products)
            .HasForeignKey(p => p.CategoryId)
            .OnDelete(DeleteBehavior.Restrict);
        
        builder.HasIndex(p => p.Name);
        builder.HasIndex(p => new { p.CategoryId, p.Name }).IsUnique();
    }
}
```

### Configuration Precedence

When both Data Annotations and Fluent API configure the same aspect, Fluent API takes precedence. This allows you to override attribute-based configuration without modifying the entity class.

```csharp
// Entity with Data Annotation
public class Product
{
    [MaxLength(100)]  // Will be overridden
    public string Name { get; set; }
}

// Fluent API overrides
modelBuilder.Entity<Product>(entity =>
{
    entity.Property(p => p.Name).HasMaxLength(200);  // This wins
});
```

---

## 15. Chapter 13: Best Practices

### Separate Configuration Classes

For maintainability, organize Fluent API configurations into separate classes implementing IEntityTypeConfiguration<T>. This keeps your DbContext clean and makes configurations easier to find and modify.

```csharp
// Good: Separate configuration classes
public class ProductConfiguration : IEntityTypeConfiguration<Product>
{
    public void Configure(EntityTypeBuilder<Product> builder)
    {
        // All product configuration in one place
    }
}

public class CategoryConfiguration : IEntityTypeConfiguration<Category>
{
    public void Configure(EntityTypeBuilder<Category> builder)
    {
        // All category configuration in one place
    }
}

// DbContext applies all configurations
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.ApplyConfigurationsFromAssembly(typeof(ApplicationDbContext).Assembly);
}
```

### Keep Entities Focused

Entity classes should focus on domain concerns, not infrastructure configuration. Use view models for validation and display concerns when the entity model differs from what users see.

```csharp
// Good: Clean entity focused on domain
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
    public int CategoryId { get; set; }
    public Category Category { get; set; }
}

// Separate view model for validation/display
public class ProductCreateViewModel
{
    [Required]
    [StringLength(200)]
    public string Name { get; set; }
    
    [Required]
    [Range(0.01, 100000)]
    public decimal Price { get; set; }
    
    [Required]
    [Display(Name = "Category")]
    public int CategoryId { get; set; }
    
    public SelectList Categories { get; set; }
}
```

### Use Meaningful Error Messages

Customize validation error messages to be helpful and user-friendly. Use resource files for localization and placeholders for dynamic values.

```csharp
// Good: Clear, helpful messages
public class Product
{
    [Required(ErrorMessage = "Product name is required. Please enter a descriptive name.")]
    [StringLength(200, MinimumLength = 3, 
        ErrorMessage = "Product name must be between {2} and {1} characters.")]
    public string Name { get; set; }
    
    [Range(0.01, 100000, 
        ErrorMessage = "Price must be a positive value between {1:C} and {2:C}")]
    public decimal Price { get; set; }
}

// Using resources for localization
public class Product
{
    [Required(ErrorMessageResourceType = typeof(Resources.ValidationMessages),
              ErrorMessageResourceName = "RequiredField")]
    [StringLength(200, 
        ErrorMessageResourceType = typeof(Resources.ValidationMessages),
        ErrorMessageResourceName = "StringLength")]
    public string Name { get; set; }
}
```

### Document Complex Configurations

Add comments to explain non-obvious configurations, especially for business rules or database optimizations.

```csharp
modelBuilder.Entity<Product>(entity =>
{
    // Unique constraint ensures only one product per category has a given SKU
    // Business requirement: SKU must be unique within a category but can be reused across categories
    entity.HasIndex(p => new { p.CategoryId, p.SKU })
        .IsUnique()
        .HasFilter("[SKU] IS NOT NULL");
    
    // Cascade delete is restricted because deleting a category with products
    // should require explicit confirmation to prevent accidental data loss
    entity.HasOne(p => p.Category)
        .WithMany(c => c.Products)
        .OnDelete(DeleteBehavior.Restrict);
    
    // Soft delete filter ensures deleted products are excluded from normal queries
    // Use IgnoreQueryFilters() to include deleted products in admin/audit queries
    entity.HasQueryFilter(p => !p.IsDeleted);
});
```

### Consistent Naming Conventions

Use consistent naming for columns, indexes, and constraints. This improves database maintainability and makes generated SQL more readable.

```csharp
modelBuilder.Entity<Product>(entity =>
{
    // Primary key naming convention: PK_{TableName}
    entity.HasKey(p => p.Id)
        .HasName("PK_Products");
    
    // Foreign key naming: FK_{DependentTable}_{PrincipalTable}
    entity.HasOne(p => p.Category)
        .WithMany()
        .HasForeignKey(p => p.CategoryId)
        .HasConstraintName("FK_Products_Categories");
    
    // Index naming: IX_{TableName}_{ColumnName(s)}
    entity.HasIndex(p => p.Name)
        .HasDatabaseName("IX_Products_Name");
    
    // Unique index: UX_{TableName}_{ColumnName(s)}
    entity.HasIndex(p => p.SKU)
        .IsUnique()
        .HasDatabaseName("UX_Products_SKU");
    
    // Default constraint naming: DF_{TableName}_{ColumnName}
    entity.Property(p => p.CreatedAt)
        .HasDefaultValueSql("GETDATE()")
        .HasDefaultValueSql("CONSTRAINT DF_Products_CreatedAt DEFAULT GETDATE()");
});
```

---

## 16. Chapter 14: Common Mistakes

### Mistake 1: Overusing Data Annotations for Database Configuration

While Data Annotations can configure some database aspects, complex scenarios are better handled with Fluent API.

```csharp
// Problematic: Complex configuration with attributes only
[Table("Products")]
public class Product
{
    [Key]
    [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
    public int Id { get; set; }
    
    [Required]
    [StringLength(200)]
    [Column("ProductName", TypeName = "nvarchar(200)")]
    public string Name { get; set; }
    
    [Column(TypeName = "decimal(18,4)")]
    public decimal Price { get; set; }
    
    // Complex relationship - hard to express with attributes
    [ForeignKey("Category")]
    public int CategoryId { get; set; }
    public Category Category { get; set; }
}

// Better: Use Fluent API for database configuration
public class ProductConfiguration : IEntityTypeConfiguration<Product>
{
    public void Configure(EntityTypeBuilder<Product> builder)
    {
        builder.ToTable("Products");
        builder.HasKey(p => p.Id);
        
        builder.Property(p => p.Name)
            .IsRequired()
            .HasMaxLength(200)
            .HasColumnName("ProductName");
        
        builder.Property(p => p.Price)
            .HasPrecision(18, 4);
        
        builder.HasOne(p => p.Category)
            .WithMany(c => c.Products)
            .HasForeignKey(p => p.CategoryId)
            .OnDelete(DeleteBehavior.Restrict);
    }
}
```

### Mistake 2: Not Configuring Cascade Delete

Leaving cascade delete at its default (often Cascade) can lead to unintended data deletion.

```csharp
// Problematic: Default cascade delete might delete unintended records
modelBuilder.Entity<Category>(entity =>
{
    entity.HasMany(c => c.Products)
        .WithOne(p => p.Category);
    // Default is Cascade - deleting category deletes all products!
});

// Better: Explicitly configure cascade behavior
modelBuilder.Entity<Category>(entity =>
{
    entity.HasMany(c => c.Products)
        .WithOne(p => p.Category)
        .OnDelete(DeleteBehavior.Restrict); // Prevents accidental deletion
});
```

### Mistake 3: Missing Indexes on Foreign Keys

Foreign keys should typically be indexed for query performance.

```csharp
// Problematic: Foreign key without index
modelBuilder.Entity<Product>(entity =>
{
    entity.HasOne(p => p.Category)
        .WithMany(c => c.Products)
        .HasForeignKey(p => p.CategoryId);
    // No index on CategoryId - slow joins and lookups
});

// Better: Add index on foreign key
modelBuilder.Entity<Product>(entity =>
{
    entity.HasOne(p => p.Category)
        .WithMany(c => c.Products)
        .HasForeignKey(p => p.CategoryId);
    
    entity.HasIndex(p => p.CategoryId);
});
```

### Mistake 4: Validation Attributes on Entity Classes

Putting validation attributes on entity classes couples domain and presentation concerns. Use view models for validation.

```csharp
// Problematic: Validation on entity
public class Product
{
    public int Id { get; set; }
    
    [Required(ErrorMessage = "Name is required")]  // Presentation concern
    [StringLength(200)]
    public string Name { get; set; }
}

// Better: Clean entity, validation on view model
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }  // Clean entity
}

public class ProductCreateViewModel
{
    [Required(ErrorMessage = "Name is required")]
    [StringLength(200)]
    public string Name { get; set; }
}
```

### Mistake 5: Forgetting to Apply Migrations

Configuration changes require migrations to update the database.

```csharp
// After changing configuration, always create and apply migration
// In Package Manager Console:
// Add-Migration AddProductIndexes
// Update-Database

// Or using CLI:
// dotnet ef migrations add AddProductIndexes
// dotnet ef database update
```

---

## 17. Quick Reference Cheatsheet

### Validation Attributes

| Attribute | Purpose | Example |
|-----------|---------|---------|
| [Required] | Field is required | `[Required]` |
| [StringLength] | String length limits | `[StringLength(100, MinimumLength = 2)]` |
| [MaxLength] | Maximum length | `[MaxLength(500)]` |
| [MinLength] | Minimum length | `[MinLength(10)]` |
| [Range] | Numeric range | `[Range(0, 100)]` |
| [RegularExpression] | Pattern matching | `[RegularExpression(@"^\d+$")]` |
| [EmailAddress] | Email format | `[EmailAddress]` |
| [Phone] | Phone format | `[Phone]` |
| [Url] | URL format | `[Url]` |
| [CreditCard] | Credit card format | `[CreditCard]` |
| [Compare] | Compare two fields | `[Compare("Password")]` |
| [DataType] | Semantic type | `[DataType(DataType.Password)]` |
| [FileExtensions] | File extensions | `[FileExtensions(Extensions = "pdf,doc")]` |

### Display Attributes

| Attribute | Purpose | Example |
|-----------|---------|---------|
| [Display] | Display configuration | `[Display(Name = "User Name")]` |
| [DisplayFormat] | Format values | `[DisplayFormat(DataFormatString = "{0:C}")]` |
| [DisplayName] | Simple display name | `[DisplayName("Price")]` |
| [ScaffoldColumn] | Include in scaffolding | `[ScaffoldColumn(false)]` |
| [ReadOnly] | Read-only field | `[ReadOnly(true)]` |
| [HiddenInput] | Hidden field | `[HiddenInput]` |
| [UIHint] | Custom template | `[UIHint("DatePicker")]` |
| [Editable] | Editable control | `[Editable(false)]` |

### Database Schema Attributes

| Attribute | Purpose | Example |
|-----------|---------|---------|
| [Table] | Table name/schema | `[Table("Products", Schema = "Inventory")]` |
| [Column] | Column configuration | `[Column("ProdName", TypeName = "nvarchar(200)")]` |
| [Key] | Primary key | `[Key]` |
| [ForeignKey] | Foreign key | `[ForeignKey("CategoryId")]` |
| [InverseProperty] | Inverse navigation | `[InverseProperty("Products")]` |
| [DatabaseGenerated] | Auto-generation | `[DatabaseGenerated(DatabaseGeneratedOption.Identity)]` |
| [Timestamp] | Row version | `[Timestamp]` |
| [ConcurrencyCheck] | Concurrency token | `[ConcurrencyCheck]` |
| [NotMapped] | Exclude from database | `[NotMapped]` |
| [Index] | Create index | `[Index(nameof(Name), IsUnique = true)]` |
| [MaxLength] | Column size | `[MaxLength(500)]` |
| [Required] | NOT NULL column | `[Required]` |

### Fluent API - Property Configuration

```csharp
entity.Property(p => p.Name)
    .HasColumnName("ProductName")           // Column name
    .HasColumnType("nvarchar(200)")         // Column type
    .HasMaxLength(200)                      // Max length
    .HasPrecision(18, 4)                    // Decimal precision
    .IsRequired()                           // NOT NULL
    .IsUnicode(false)                       // VARCHAR vs NVARCHAR
    .HasDefaultValue(true)                  // Default value
    .HasDefaultValueSql("GETDATE()")        // SQL default
    .HasComputedColumnSql("A + B")          // Computed column
    .ValueGeneratedOnAdd()                  // Auto-generate on insert
    .ValueGeneratedOnUpdate()               // Auto-generate on update
    .IsRowVersion()                         // Row version
    .IsConcurrencyToken()                   // Concurrency token
    .HasComment("Description");             // Column comment
```

### Fluent API - Relationship Configuration

```csharp
entity.HasOne(p => p.Category)             // Reference navigation
    .WithMany(c => c.Products)              // Collection navigation
    .HasForeignKey(p => p.CategoryId)       // Foreign key property
    .HasPrincipalKey(c => c.Id)             // Principal key (if alternate)
    .IsRequired()                           // Required relationship
    .OnDelete(DeleteBehavior.Cascade);      // Cascade delete

// Delete behaviors
// Cascade     - Delete dependents
// Restrict    - Prevent deletion
// SetNull     - Set FK to null
// NoAction    - No action
```

### Fluent API - Keys and Indexes

```csharp
// Primary key
entity.HasKey(p => p.Id);
entity.HasKey(p => new { p.OrderId, p.ProductId });  // Composite

// Alternate key
entity.HasAlternateKey(p => p.Email);

// Index
entity.HasIndex(p => p.Name);
entity.HasIndex(p => p.Email).IsUnique();
entity.HasIndex(p => new { p.LastName, p.FirstName });
entity.HasIndex(p => p.Email).HasFilter("[Email] IS NOT NULL");
```

---

## 18. Additional Resources

### Official Documentation
- [Entity Framework Core Documentation](https://docs.microsoft.com/en-us/ef/core/)
- [Data Annotations Reference](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations)
- [Fluent API Reference](https://docs.microsoft.com/en-us/ef/core/modeling/)

### Learning Resources
- [Microsoft Learn - Entity Framework Core](https://learn.microsoft.com/en-us/training/modules/persist-data-ef-core/)
- [Entity Framework Core GitHub](https://github.com/dotnet/efcore)
- [EF Core Community Standup](https://www.youtube.com/playlist?list=PLdo4fOcmZ0oX7uTkjYywvS2VFHJ8ANnSs)

### Tools
- [EF Core Power Tools](https://github.com/ErikEJ/EFCorePowerTools) - Visual Studio extension
- [LINQPad](https://www.linqpad.net/) - Query testing
- [dotnet ef CLI](https://docs.microsoft.com/en-us/ef/core/cli/dotnet) - Command-line tools

### Recommended Books
- "Entity Framework Core in Action" by Jon Smith
- "Programming Entity Framework" by Julia Lerman
- "Domain-Driven Design" by Eric Evans

---

## Summary

Congratulations on completing this comprehensive guide to Data Annotations and Fluent API! You now have mastery over both approaches to configuring your .NET applications and Entity Framework Core models. You understand when to use each approach and how they complement each other.

Remember that Data Annotations excel at validation and display configuration, keeping these concerns visible on the model class. Fluent API provides complete control over database mapping, handling complex scenarios that attributes cannot express. Using both together—Data Annotations for validation/display and Fluent API for database configuration—leverages the strengths of each approach.

Continue practicing by building projects that exercise these concepts. Experiment with complex relationships, inheritance mapping, and advanced configurations. As you gain experience, you'll develop intuition for the best approach in each situation.

The knowledge you've gained here—model configuration, validation, relationship mapping—forms the foundation for building robust, maintainable data-driven applications. These skills will serve you throughout your .NET development career.

Happy coding!

---

*This guide was created to help developers master Data Annotations and Fluent API from basics to advanced level. For the most up-to-date information, always refer to the official Microsoft documentation.*

# Practical 3

# Creating Applications with Data (CRUD)

**CRUD (Create, Read, Update, Delete) forms the foundational architecture for managing persistent data records in enterprise web applications. In ASP.NET Core, these four basic data-handling operations map directly to specialized standard HTTP architectural protocols, domain models, and Entity Framework (EF) Core state-tracking mechanisms.**

**Entity Framework (EF) Core is Microsoft's lightweight, open-source, and cross-platform ObjectRelational Mapper (ORM). It enables developers to interact with relational and NoSQL databases using C# objects and LINQ, eliminating the need to write and maintain manual data-access code or raw SQL queries.**

# Student Management System

This practical demonstrates how to create a **CRUD (Create, Read, Update, Delete)** application using:

* ASP.NET MVC 5
* .NET Framework 4.7.2
* Entity Framework 6
* SQL Server LocalDB
* Visual Studio 2019

# Aim

To create a **Student Management System** that allows users to:

1. Add a new student.
2. Display student records.
3. Edit student information.
4. Delete student records.

# Step 1: Create a New Project

1. Open **Visual Studio 2019**.
2. Click **Create a new project**.
3. Select **ASP.NET Web Application (.NET Framework)**.
4. Click **Next**.

# Step 2: Configure Project

Enter:

| Field        | Value                |
| ------------ | -------------------- |
| Project Name | StudentCRUD          |
| Location     | Choose Folder        |
| Framework    | .NET Framework 4.7.2 |

Click **Create**.

# Step 3: Select Template

Choose: **MVC**

Authentication:

**No Authentication**

Click **Create**.

# Step 4: Run the Application

Press:

**Ctrl + F5**

The default MVC application should open.

# Step 5: Install Entity Framework 6

Open:

**Tools → NuGet Package Manager → Package Manager Console**

Execute:

```powershell
Install-Package EntityFramework
```

After installation, the project will have Entity Framework 6.

# Step 6: Create Model Class

Right-click **Models** folder.

Select:

**Add → Class**

Name:

**Student.cs**

Write the following code:

```csharp
using System.ComponentModel.DataAnnotations;

namespace StudentCRUD.Models
{
    public class Student
    {
        [Key]
        public int StudentId { get; set; }

        [Required]
        [StringLength(50)]
        public string Name { get; set; }

        [Range(18, 60)]
        public int Age { get; set; }

        [StringLength(50)]
        public string Course { get; set; }
    }
}
```

# Step 7: Create Database Context Class

Right-click **Models** folder.

Select:

**Add → Class**

Name:

**StudentContext.cs**

Write:

```csharp
using System.Data.Entity;

namespace StudentCRUD.Models
{
    public class StudentContext : DbContext
    {
        public StudentContext()
            : base("StudentConnection")
        {
        }

        public DbSet<Student> Students
        {
            get;
            set;
        }
    }
}
```

# Step 8: Configure Connection String

Open:

**Web.config**

Inside `<configuration>` add:

```xml
<connectionStrings>
    <add name="StudentConnection"
         connectionString="Data Source=(LocalDB)\MSSQLLocalDB;Initial Catalog=StudentDB;Integrated Security=True"
         providerName="System.Data.SqlClient"/>
</connectionStrings>
```

# Step 9: Enable Migrations

Open **Package Manager Console**.

Execute:

```powershell
Enable-Migrations
```

You will see:

**Migrations folder created.**

# Step 10: Create Migration

Execute:

```powershell
Add-Migration InitialCreate
```

# Step 11: Create Database

Execute:

```powershell
Update-Database
```

The database **StudentDB** and **Students** table will be created automatically.

# Step 12: Verify Database

Open:

**View → SQL Server Object Explorer**

Expand:

**(localdb)\MSSQLLocalDB**

Then:

**StudentDB → Tables → dbo.Students**

# Step 13: Create Controller using Scaffolding

Right-click **Controllers**.

Select:

**Add → Controller**

Choose:

**MVC 5 Controller with views, using Entity Framework**

Click **Add**.

# Step 14: Configure Scaffold

Fill:

| Option             | Value              |
| ------------------ | ------------------ |
| Model Class        | Student            |
| Data Context Class | StudentContext     |
| Controller Name    | StudentsController |

Click **Add**.

Visual Studio automatically creates the controller and CRUD views.

The following files are created:

* `StudentsController.cs`
* `Create.cshtml`
* `Edit.cshtml`
* `Delete.cshtml`
* `Details.cshtml`
* `Index.cshtml`

# Step 15: Run Application

Press:

**Ctrl + F5**

In browser, open:

`https://localhost:44300/Students`

# CRUD Operation 1: Create

Click:

**Create New**

Enter:

| Name | Age | Course |
| ---- | --: | ------ |
| Amit |  20 | BSc IT |

Click:

**Create**

The record is inserted into the database.

# CRUD Operation 2: Read

The student list is displayed.

| ID | Name | Age | Course |
| -: | ---- | --: | ------ |
|  1 | Amit |  20 | BSc IT |

# CRUD Operation 3: Update

Click:

**Edit**

Change:

**Age = 21**

Click:

**Save**

The record is updated.

# CRUD Operation 4: Delete

Click:

**Delete**

Then click:

**Delete**

The student record is removed.

# Understanding Generated Controller Code

## Display Records

```csharp
public ActionResult Index()
{
    return View(db.Students.ToList());
}
```

## Create Record

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public ActionResult Create(Student student)
{
    if (ModelState.IsValid)
    {
        db.Students.Add(student);
        db.SaveChanges();
        return RedirectToAction("Index");
    }

    return View(student);
}
```

## Edit Record

```csharp
db.Entry(student).State = EntityState.Modified;
db.SaveChanges();
```

## Delete Record

```csharp
Student student = db.Students.Find(id);
db.Students.Remove(student);
db.SaveChanges();
```

# Output Screens

The application displays the Student Management System with options to:

* Create a new student
* View student records
* Edit student information
* Delete student records

The CRUD operations can be tested through the **Students** page in the browser.

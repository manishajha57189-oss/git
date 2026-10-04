**Practical 6**

**Working with URL routing and dependency injections.**

**URL routing** is the process of mapping web addresses to specific code functions or files in an application. It takes an incoming web request, reads the path, and sends it to the correct part of the software to load the right page.

**Dependency injection** is a software design pattern where an object receives the other objects it needs—its dependencies—from an external source rather than creating them internally. As a core form of inversion of control, it separates object creation from behavior to make code loosely coupled, reusable, and easy to test.

***Steps for Help:-(not for Print):***

**URL Routing Pract:**

**Create the Project**

Open **Visual Studio 2019**.

Select:

**Create a new project**

Choose:

**ASP.NET Web Application (.NET Framework)**

Click **Next**.

Enter:

Project Name: URLRoutingDemo

Select:

.NET Framework 4.7.2

Click **Create**.

Select:

**MVC**

Select:

**No Authentication**

Click **Create**.

**5. Create StudentController**

Right-click: **Controllers**

Select:

**Add → Controller**

Select:

**MVC 5 Controller - Empty**

Click **Add**.

Enter: StudentController

Click **Add**.

Replace the code with:

```csharp
using System.Web.Mvc;

namespace URLRoutingDemo.Controllers
{
    public class StudentController : Controller
    {
        public ActionResult Index()
        {
            return Content("Welcome to Student Page");
        }

        public ActionResult Details(int id)
        {
            return Content("Student ID is: " + id);
        }
    }
}
```

**6. Test the First URL**

Run the project using:

**Ctrl + F5**

Suppose Visual Studio starts the project at:

**https://localhost:44300/**

Now enter:

**https://localhost:44300/Student/Index**

You should see:

**Welcome to Student Page**

**Why?**

The URL: /Student/Index is mapped as:

Controller → Student

Action → Index

Therefore MVC executes:

StudentController

Index()

**7. Test URL with Parameter**

Now enter:

**https://localhost:44300/Student/Details/101**

You should see:

**Student ID is: 101**

The URL:

/Student/Details/101 is mapped as:

Controller = Student

Action = Details

Id = 101

Therefore MVC executes:

```csharp
public ActionResult Details(int id)
{
    return Content("Student ID is: " + id);
}
```

Here: id = 101

---

**Dependency Injection Pract:**

**Step 1: Create the ASP.NET MVC Project**

Open **Visual Studio 2019**.

Select: **Create a new project**

Select:

**ASP.NET Web Application (.NET Framework)**

Click **Next**.

Enter:

Project Name: DependencyInjectionDemo

Select:

.NET Framework 4.7.2 or another .NET Framework version available in your Visual Studio.

Click **Create**.

Select:

**MVC**

Select: **No Authentication**

Click **Create**.

**Step 2: Install Ninject**

Go to:

**Tools → NuGet Package Manager → Package Manager Console**

Run:

```text
Install-Package Ninject.MVC5
```

Wait until installation finishes.

**Important**

You may **not** get: NinjectWebCommon.cs

Then create your own:

NinjectConfig.cs

**Step 3: Check Ninject References**

In **Solution Explorer**, expand:

**References**

Look for Ninject-related references.

You should have packages/references such as:

Ninject

Ninject.Web.Common

Ninject.Web.Mvc

If necessary, install them from Package Manager Console:

```text
Install-Package Ninject
```

```text
Install-Package Ninject.Web.Common
```

```text
Install-Package Ninject.Web.Mvc
```

**Step 4: Create IStudentService.cs**

In Solution Explorer:

**Right-click Models → Add → Class**

Name:

IStudentService.cs

Put this code inside:

```csharp
using System.Collections.Generic;

namespace DependencyInjectionDemo.Models
{
    public interface IStudentService
    {
        List<string> GetStudents();
    }
}
```

**What is this?**

IStudentService is an **interface**.

It tells that any student service must provide:

GetStudents()

**Step 5: Create StudentService.cs**

Right-click:

**Models → Add → Class**

Name: StudentService.cs

Add:

```csharp
using System.Collections.Generic;

namespace DependencyInjectionDemo.Models
{
    public class StudentService : IStudentService
    {
        public List<string> GetStudents()
        {
            return new List<string>
            {
                "Rahul",
                "Priya",
                "Amit",
                "Sneha"
            };
        }
    }
}
```

Here:

public class StudentService : IStudentService means:

StudentService implements IStudentService.

**Step 6: Create StudentController.cs**

Right-click:

**Controllers → Add → Controller**

Select:

**MVC 5 Controller - Empty**

Click **Add**.

Enter: StudentController

Click **Add**.

Replace the controller code with:

```csharp
using System.Web.Mvc;
using DependencyInjectionDemo.Models;

namespace DependencyInjectionDemo.Controllers
{
    public class StudentController : Controller
    {
        private readonly IStudentService studentService;

        public StudentController(IStudentService studentService)
        {
            this.studentService = studentService;
        }

        public ActionResult Index()
        {
            var students = studentService.GetStudents();
            return View(students);
        }
    }
}
```

**Understand the Constructor**

The most important part is:

```csharp
public StudentController(IStudentService studentService)
{
    this.studentService = studentService;
}
```

**The controller is saying:**

**"I need an object that implements IStudentService." We are not writing: studentService = new StudentService(); Ninject will provide the required object.**

**This is called:**

**Constructor Injection**

**Step 7: Create NinjectConfig.cs**

This is the step that replaces NinjectWebCommon.cs.

In Solution Explorer, find:

App_Start

Right-click:

**App_Start → Add → Class**

Name it:

NinjectConfig.cs

Add this complete code:

```csharp
using Ninject;
using Ninject.Web.Mvc;
using System.Web.Mvc;
using DependencyInjectionDemo.Models;

namespace DependencyInjectionDemo.App_Start
{
    public static class NinjectConfig
    {
        public static void RegisterDependencies()
        {
            IKernel kernel = new StandardKernel();

            kernel.Bind<IStudentService>()
                  .To<StudentService>();

            DependencyResolver.SetResolver(
                new NinjectDependencyResolver(kernel)
            );
        }
    }
}
```

**Step 8: Modify Global.asax.cs**

Now we have to tell our MVC application to start Ninject.

Open:

Global.asax.cs

You will see something similar to:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.Mvc;
using System.Web.Optimization;
using System.Web.Routing;

namespace DependencyInjectionDemo
{
    public class MvcApplication : System.Web.HttpApplication
    {
        protected void Application_Start()
        {
            AreaRegistration.RegisterAllAreas();
            FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
            RouteConfig.RegisterRoutes(RouteTable.Routes);
            BundleConfig.RegisterBundles(BundleTable.Bundles);
        }
    }
}
```

Add:

```csharp
using DependencyInjectionDemo.App_Start;
```

Then modify Application_Start().

Your complete code should look like:

```csharp
using System;
using System.Web;
using System.Web.Mvc;
using System.Web.Optimization;
using System.Web.Routing;

using DependencyInjectionDemo.App_Start;

namespace DependencyInjectionDemo
{
    public class MvcApplication : System.Web.HttpApplication
    {
        protected void Application_Start()
        {
            AreaRegistration.RegisterAllAreas();
            FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
            RouteConfig.RegisterRoutes(RouteTable.Routes);
            BundleConfig.RegisterBundles(BundleTable.Bundles);

            NinjectConfig.RegisterDependencies();
        }
    }
}
```

**Important**

NinjectConfig.RegisterDependencies();

This starts our Ninject configuration when the application starts.

**Step 9: Create the Student View**

Go to:

Controllers

StudentController.cs

Right-click inside:

```csharp
public ActionResult Index()
{
    var students = studentService.GetStudents();
    return View(students);
}
```

Select:

**Add View**

Use:

View name: Index

Template: Empty

Click **Add**.

Visual Studio should create:

Views

Student

Index.cshtml

**Step 10: Add Code to Index.cshtml**

Open:

Views → Student → Index.cshtml

Add:

```cshtml
@model List<string>

<!DOCTYPE html>

<html>
<head>
    <title>Student List</title>
</head>

<body>
    <h2>Student List</h2>

    <ul>
        @foreach (var student in Model)
        {
            <li>@student</li>
        }
    </ul>
</body>
</html>
```

**Step 11: Build the Project**

Go to:

**Build → Build Solution**

**Step 12: Run the Project**

Press:

**Ctrl + F5**

**Suppose Visual Studio opens:**

**https://localhost:44300/**

**Change the URL to:**

**https://localhost:44300/Student/Index**

Your output should be:

Student List

Rahul

Priya

Amit

Sneha

**Practical 9**

**Creating an Application with Controllers and Views**

In ASP.NET Core MVC, Controllers and Views work together to handle user requests and display information in a web application. A Controller is a C# class that receives requests from the user, processes the required information, and decides which response should be returned. It contains action methods, where each method performs a particular operation. A View is a user-interface file, usually written using Razor syntax with the .cshtml extension, that is responsible for displaying HTML content to the user. The Controller can pass data to the View using objects such as ViewBag, ViewData, or strongly typed models. When a user requests a URL such as /Home/Index, ASP.NET Core's routing mechanism identifies the HomeController and calls its Index() action method. The action then returns the appropriate View, such as Views/Home/Index.cshtml, which generates the HTML response shown in the browser. Thus, Controllers handle the application logic and requests, while Views handle the presentation and display of data, providing a clear separation between application processing and user interface.

**Part A – Create the Project Step 1: Open Visual Studio 2019** Open **Visual Studio 2019**.

Select:

**Create a new project**

**Step 2: Select ASP.NET MVC Project** In the search box, type:

ASP.NET Web Application (.NET Framework)

Select:

**ASP.NET Web Application (.NET Framework)** Click **Next**.

**Step 3: Configure the Project** Enter:

**Project name:**

StudentMVC

Select a suitable location.

Click **Create**.

**Step 4: Select MVC Template** You will see:

**Create a new ASP.NET Web Application**

Select:

MVC

You can leave the other options at their default values.

Click **Create**.

Visual Studio will create the project.

**Part B – Understand the Project Structure**

In **Solution Explorer**, you will see something similar to:

StudentMVC

│

├── App_Start

│

├── Content

│

├── Controllers

│   └── HomeController.cs

│

├── Models

│

├── Views

│   ├── Home

│   │   ├── Index.cshtml

│   │   ├── About.cshtml

│   │   └── Contact.cshtml

│   │

│   └── Shared

│

├── Global.asax

├── Web.config

└── ...

**Part C – Understand the Existing Controller** Open:

Controllers

```
→ HomeController.cs You will see code similar to: using System.Web.Mvc; namespace StudentMVC.Controllers {     public class HomeController : Controller 

{ 

    public ActionResult Index() 

    {             return View(); 

    } 

    public ActionResult About() 

    { 

        ViewBag.Message = "Your application description page.";             return View(); 

    } 

    public ActionResult Contact() 

    { 

        ViewBag.Message = "Your contact page.";             return View(); 

    } 

} 
```

}

The important part is:

public ActionResult Index()

{     return View();

}

Here:

•        Index() is an **action method**.

•        return View() tells MVC to display the corresponding View.

•        The corresponding view is normally:

Views/Home/Index.cshtml

**Part D – Modify the Controller** Let's create a simple student page.

Open:

Controllers

→ HomeController.cs Add the following action: public ActionResult Student() {

```
ViewBag.Name = "Rahul"; 

ViewBag.Course = "B.Sc. Information Technology"; 

ViewBag.RollNo = 101; 

return View(); 
```

}

Your controller will now contain:

public class HomeController : Controller

{     public ActionResult Index()

```
{         return View(); 

} 

public ActionResult Student() 

{ 

    ViewBag.Name = "Rahul"; 

    ViewBag.Course = "B.Sc. Information Technology"; 

    ViewBag.RollNo = 101; 

    return View(); 

} 
```

}

**Part E – Create the View**

Now we need to create a View for the Student() action.

**Step 1**

Right-click on: Student() inside HomeController.cs.

Select:

Add View...

**Step 2**

In the Add View window, enter:

View name: Student Select:

Empty (without model) Click:

**Add**

Visual Studio creates: Views

└── Home

```
   └── Student.cshtml 
```

**Part F – Add HTML to the View** Open:

Views

→ Home

→ Student.cshtml Replace its contents with:

@{

```
ViewBag.Title = "Student Information"; 
```

}

<h2>Student Information</h2>

<p><strong>Name:</strong> @ViewBag.Name</p>

<p><strong>Roll Number:</strong> @ViewBag.RollNo</p>

<p><strong>Course:</strong> @ViewBag.Course</p> Here:

@ViewBag.Name

gets the value from the Controller.

The Controller contains:

ViewBag.Name = "Rahul";

Therefore, the View displays:

Name: Rahul

**Part G – Run the Application** Press: Ctrl + F5 or click:

**Start Without Debugging** The application will open in the browser.

The default URL might look like:

http://localhost:xxxxx/ To access our Student page, add:

/Home/Student For example:

http://localhost:54321/Home/Student

The exact port number may be different on your computer.

You should see:

Student Information

Name: Rahul

Roll Number: 101

Course: B.Sc. Information Technology

**Part H – Understand MVC Routing** Why does this URL work?

/Home/Student

```
│      │ 

│      └── Action Method 

│ 

└───────── Controller So: 
```

Home → HomeController

Student → Student() MVC executes:

public ActionResult Student()

{

```
... 

return View(); 
```

} and displays:

Views/Home/Student.cshtml

**Part I – Passing Data from Controller to View**

One of the simplest ways to pass data is using ViewBag. **Controller** public ActionResult Student() {

```
ViewBag.Name = "Rahul"; 

ViewBag.Age = 20; 

ViewBag.Course = "B.Sc. IT"; 

return View(); 
```

}

**View**

<h2>Student Details</h2>

<p>Name: @ViewBag.Name</p>

<p>Age: @ViewBag.Age</p>

<p>Course: @ViewBag.Course</p>

**Output**

Student Details

Name: Rahul

Age: 20

Course: B.Sc. IT

**Part J – Creating a Second Controller** Now let's create a separate controller.

In **Solution Explorer**:

Right-click:

Controllers

Select:

Add

→ Controller Select:

MVC 5 Controller - Empty Click **Add**.

Enter: StudentController Click **Add**.

Visual Studio creates:

Controllers

```
├── HomeController.cs 

└── StudentController.cs 
```

**Part K – Add an Action to StudentController** Open:

StudentController.cs Add: using System.Web.Mvc;

namespace StudentMVC.Controllers

{     public class StudentController : Controller

```
{ 

    public ActionResult Index() 

    { 

        ViewBag.Message = "Welcome to Student Management"; 

        return View(); 

    } 

    public ActionResult Details() 

    { 

        ViewBag.Name = "Priya"; 

        ViewBag.Course = "B.Sc. Information Technology";             ViewBag.RollNo = 102; 

        return View(); 

    } 

} 
```

}

**Part L – Create the Details View** Right-click:

public ActionResult Details() Select: Add View

Enter: Details Select: Empty (without model) Click **Add**.

The following file will be created: Views

└── Student

```
   └── Details.cshtml Add: 
```

@{

```
ViewBag.Title = "Student Details"; 
```

}

<h2>Student Details</h2>

<p>Name: @ViewBag.Name</p>

<p>Roll Number: @ViewBag.RollNo</p>

<p>Course: @ViewBag.Course</p> **Part M – Run the Second Controller** Run the project and enter: /Student/Details For example:

http://localhost:54321/Student/Details The flow is:

Browser

|

| /Student/Details

↓

StudentController

|

| Details()

↓

Views/Student/Details.cshtml

|

↓

HTML Page

**Part N – Create a Link Between Views**

Suppose you want a link on the Home page to open the Student page.

Open:

Views/Home/Index.cshtml Add:

<h2>Welcome to Student MVC Application</h2>

<a href="/Student/Details">View Student Details</a> A better MVC approach is to use:

@Html.ActionLink(

```
"View Student Details", 

"Details", 

"Student" 
```

)

This creates a link to:

StudentController

```
   ↓ 
```

Details()

```
   ↓ 
```

Details.cshtml

**Complete Example**

Your project can now have this structure:

StudentMVC

│

├── Controllers

│   ├── HomeController.cs

│   └── StudentController.cs

│

├── Models

│

├── Views

│   │

│   ├── Home

│   │   ├── Index.cshtml

│   │   ├── About.cshtml

│   │   ├── Contact.cshtml │   │   └── Student.cshtml

│   │

│   ├── Student

│   │   └── Details.cshtml

│   │

│   └── Shared

│

├── Global.asax

└── Web.config


**Practical 5**

**Implementing Security to Application**

**Authentication is verifying who a user is, while authorization is determining what they are allowed to do.** In ASP.NET Core, these two processes work together but are handled by distinct middleware components in the application pipeline.

• **Authentication (Who are you?):** The process of challenge and verification. The user provides credentials (like a username/password or a token), and the system validates them to create an identity.

• **Authorization (What can you do?):** The process of checking permissions. The system looks at the authenticated user's identity, roles, or claims to decide if they can access a specific resource.

# 1: Create a New ASP.NET Core MVC Project

• Open **Visual Studio 2019**.

• Click **Create a new project**.

• Select **ASP.NET Web Application(.Net Framework)**.

• Click **Next**.

• Enter

**Project Name: SecureMVCApp**

• Click **Create**.

• Choose

**ASP.NET Core Web App (Model-View-Controller)**

• Click **Change Authentication**.

Choose

**Individual User Accounts**

• Click **OK**.

• Click **Create**.

Visual Studio automatically creates:

• Identity Database

• Login page

• Registration page

• Logout page

• Identity models

# 2: Run the Application

Press

**Ctrl + F5**

The home page opens.

Top-right corner contains

**Register**

**Login**

These are automatically created.

# 3: Create the Identity Database

Open

**View → Server Explorer**

Check

**Data connections\DefaultConnections(DemoSecurity)\Tables**

First register a user, ASP.NET Core automatically creates a database similar to

**SecureMVCAppIdentity**

Contains tables:

**AspNetUsers**

**AspNetRoles**

**AspNetUserRoles**

**AspNetUserClaims**

**AspNetUserLogins**

**AspNetUserTokens**

# 4: Register a User

Click

**Register**

Fill

**Email**

[student@gmail.com](mailto:student@gmail.com)

**Password**

Student@123

**Confirm Password**

Student@123

Click

**Register**

The user is saved in

**AspNetUsers**

# 5: Verify the User

Open

**Server Explorer**

Expand

**Tables**

**AspNetUsers**

Right-click

**View Data**

You can see

**Email**

**Password Hash**

**UserName**

The password is **encrypted**.

# 6: Understand Authentication

Authentication means

**Who are you?**

Example

Username: [student@gmail.com](mailto:student@gmail.com)

Password: Student@123

If correct User logs in.

Otherwise Access denied.

# 7: Create a Secure Controller

Create

**Controllers**

Right-click

**Add**

**Controller**

Create

**StudentController.cs**

```csharp
namespace SecureMVCApp.Controllers
{
    [Authorize]
    public class StudentController : Controller
    {
        public ActionResult Index()
        {
            return View();
        }
    }
}
```

Notice

**[Authorize]**

This protects the controller.

# 8: Create View

Inside

**Views**

**Student**

Create

**Index.cshtml**

```html
<h2>Student Dashboard</h2>

<h3>Only logged-in users can see this page.</h3>
```

# 9: Test Authorization

Run

**https://localhost:5001/Student**

Without login

You are redirected automatically to

**Login Page**

After login

**Student Dashboard** opens.

# 10: Protect Only One Action

Instead of protecting the whole controller

```csharp
public class StudentController : Controller
{
    public ActionResult About()
    {
        return View();
    }

    [Authorize]
    public ActionResult Result()
    {
        return View();
    }
}
```

Now

**https://localhost:44320/Student/Index**

**https://localhost:44320/Student/about**

**Everyone can access.**

**https://localhost:44320/Student/Result**

**Redirect to Login page**

# 11: Allow Anonymous Users

Sometimes one page should be public.

Example

```csharp
[Authorize]
public class StudentController : Controller
{
    [AllowAnonymous]
    public ActionResult About()
    {
        return View();
    }

    public ActionResult Dashboard()
    {
        return View();
    }
}
```

Result

| Page      | Login Required |
| --------- | -------------- |
| About     | No             |
| Dashboard | Yes            |

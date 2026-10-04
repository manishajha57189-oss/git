**Practical 8**

**Creating a RESTful Service**

A RESTful service in ASP.NET Core is a lightweight, high-performance web service that lets different applications—like mobile apps or websites—communicate with a server over the internet. It follows the REST architectural style using standard HTTP verbs like GET, POST, PUT, and DELETE to perform basic data operations (read, create, update, and delete). For beginners, building one with ASP.NET Core Web API means writing simple C# controllers that automatically translate data into standard formats like JSON so that clients can easily send requests and receive responses.

**1. Create a New Project**

Open **Visual Studio 2019**.

Select:

**File → New → Project**

Search for:

**ASP.NET Web Application (.NET Framework)**

Select it and click **Next**.

Enter:

• **Project Name:** StudentRESTService
• **Location:** Choose your preferred folder
• **Framework:** .NET Framework 4.7.2 or .NET Framework 4.8

Click **Create**.

**2. Select Web API**

In the next window, select:

**Web API**

and click **Create**.

Visual Studio creates the basic RESTful Web API project.

**3. Understand REST**

REST stands for **Representational State Transfer**.

A RESTful service allows a client such as a browser, mobile application, or another application to communicate with a server using HTTP.

The commonly used HTTP methods are:

| HTTP Method | Purpose       | Example          |
| ----------- | ------------- | ---------------- |
| GET         | Retrieve data | Get all students |
| POST        | Add data      | Add a student    |
| PUT         | Update data   | Update student   |
| DELETE      | Delete data   | Delete student   |

For example:

GET /api/students

POST /api/students

PUT /api/students/1

DELETE /api/students/1

**4. Create a Student Model**

Right-click the **Models** folder.

Select:

**Add → Class**

Name the class:

**Student.cs**

Add:

```csharp
namespace StudentRESTService.Models
{
    public class Student
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Course { get; set; }
        public int Age { get; set; }
    }
}
```

This class represents a student.

**5. Create the REST Controller**

Right-click the **Controllers** folder.

Select:

**Add → Controller**

Choose: **Web API 2 Controller - Empty**

Click **Add**.

Enter:

**StudentsController**

The controller should inherit from:

**ApiController**

Add the following code:

```csharp
using System.Collections.Generic;
using System.Linq;
using System.Web.Http;
using StudentRESTService.Models;

namespace StudentRESTService.Controllers
{
    public class StudentsController : ApiController
    {
        static List<Student> students = new List<Student>
        {
            new Student
            {
                Id = 1,
                Name = "Rahul",
                Course = "BSc IT",
                Age = 20
            },
            new Student
            {
                Id = 2,
                Name = "Priya",
                Course = "BSc IT",
                Age = 21
            }
        };

        // GET: api/students
        public IEnumerable<Student> GetStudents()
        {
            return students;
        }

        // GET: api/students/1
        public IHttpActionResult GetStudent(int id)
        {
            var student = students.FirstOrDefault(s => s.Id == id);
            if (student == null)
                return NotFound();

            return Ok(student);
        }

        // POST: api/students
        public IHttpActionResult PostStudent(Student student)
        {
            student.Id = students.Count + 1;
            students.Add(student);
            return Ok(student);
        }

        // PUT: api/students/1
        public IHttpActionResult PutStudent(int id, Student student)
        {
            var existingStudent =
                students.FirstOrDefault(s => s.Id == id);

            if (existingStudent == null)
                return NotFound();

            existingStudent.Name = student.Name;
            existingStudent.Course = student.Course;
            existingStudent.Age = student.Age;

            return Ok(existingStudent);
        }

        // DELETE: api/students/1
        public IHttpActionResult DeleteStudent(int id)
        {
            var student =
                students.FirstOrDefault(s => s.Id == id);

            if (student == null)
                return NotFound();

            students.Remove(student);
            return Ok(student);
        }
    }
}
```

**6. Configure Web API Routing**

Open:

**App_Start → WebApiConfig.cs**

You should see routing similar to:

```csharp
using System.Web.Http;

namespace StudentRESTService
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new
                {
                    id = RouteParameter.Optional
                }
            );
        }
    }
}
```

The important part is:

```text
api/{controller}/{id}
```

Therefore:

StudentsController can be accessed using:

```text
/api/students
```

**7. Run the RESTful Service**

Press: **Ctrl + F5** or click:

**IIS Express**

Visual Studio will open the browser.

Suppose Visual Studio gives you:

```text
http://localhost:54321/
```

Then the student service URL will be:

```text
http://localhost:54321/api/students
```

**8. Test GET**

Open:

```text
/api/students
```

The service should return JSON similar to:

```json
[
    {
        "Id": 1,
        "Name": "Rahul",
        "Course": "BSc IT",
        "Age": 20
    },
    {
        "Id": 2,
        "Name": "Priya",
        "Course": "BSc IT",
        "Age": 21
    }
]
```

To retrieve a particular student:

```text
GET /api/students/1
```

Response:

```json
{
    "Id": 1,
    "Name": "Rahul",
    "Course": "BSc IT",
    "Age": 20
}
```

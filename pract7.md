**Practical 7**

**Creating Application with caching**

Caching in .NET Core is a performance optimization technique that stores copies of frequently accessed, expensive-to-compute, or slow-to-fetch data in a temporary, high-speed storage layer. Instead of querying a slow database or a third-party API every time a request comes in, the application serves the data directly from this temporary storage (RAM or an external fastaccess cache). This drastically reduces application latency, lowers server CPU and database load, and improves scalability.

Open **Visual Studio 2019**.

**Step 1: Create a new project**

Select:

**File → New → Project**

Select:

**ASP.NET Web Application (.NET Framework)**

Click **Next**.

**Step 2: Configure the project**

Enter:

**Project Name:** CachingDemo

Choose your desired location.

Click **Create**.

**3. Select MVC Template**

The following window appears:

**Create a new ASP.NET Web Application**

Select:

**MVC**

Keep the default settings.

Click:

**Create**

**4. Create a Simple Data Model**

Right-click the **Models** folder.

Select:

**Add → Class**

Name it:

**Product.cs**

Add:

```csharp
namespace CachingDemo.Models
{
    public class Product
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public double Price { get; set; }
    }
}
```

This class represents a product.

**5. Add Caching to the Controller**

Open:

**Controllers → HomeController.cs**

Replace the contents with:

```csharp
using System;
using System.Collections.Generic;
using System.Web.Mvc;
using System.Web.Caching;
using CachingDemo.Models;

namespace CachingDemo.Controllers
{
    public class HomeController : Controller
    {
        public ActionResult Index()
        {
            string cacheKey = "ProductList";

            List<Product> products =
                HttpContext.Cache[cacheKey] as List<Product>;

            if (products == null)
            {
                // Simulate retrieving data from database
                System.Threading.Thread.Sleep(3000);

                products = new List<Product>
                {
                    new Product
                    {
                        Id = 1,
                        Name = "Laptop",
                        Price = 55000
                    },

                    new Product
                    {
                        Id = 2,
                        Name = "Mobile",
                        Price = 25000
                    },

                    new Product
                    {
                        Id = 3,
                        Name = "Tablet",
                        Price = 30000
                    }
                };

                HttpContext.Cache.Insert(
                    cacheKey,
                    products,
                    null,
                    DateTime.Now.AddMinutes(5),
                    Cache.NoSlidingExpiration
                );

                ViewBag.Message =
                    "Data loaded from database and stored in cache.";
            }
            else
            {
                ViewBag.Message =
                    "Data loaded from cache.";
            }

            return View(products);
        }
    }
}
```

**6. Create the View**

Open:

**Views → Home → Index.cshtml**

Replace its contents with:

```cshtml
@model IEnumerable<CachingDemo.Models.Product>

@{
    ViewBag.Title = "Caching Demo";
}

<h2>Caching Demo</h2>

<p>
    <strong>@ViewBag.Message</strong>
</p>

<table border="1" cellpadding="10">
    <tr>
        <th>ID</th>
        <th>Product Name</th>
        <th>Price</th>
    </tr>

    @foreach (var product in Model)
    {
        <tr>
            <td>@product.Id</td>
            <td>@product.Name</td>
            <td>₹@product.Price</td>
        </tr>
    }
</table>
```

**7. Run the Application**

Press:

**Ctrl + F5**

The browser opens.

You should see the Caching Demo page with the product details.

**8. Test the Cache**

This is the important part of the practical.

**First request**

When you open:

**/Home/Index**

you should see:

**Data loaded from database and stored in cache.**

There will be approximately a **3-second delay**.

The data is now stored in the cache.

**9. Second request**

Refresh the page:

**F5**

You should see:

**Data loaded from cache.**

**This time the page should load much faster.**

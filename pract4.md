**Practical 4**

**Aim: Creating a Shopping Cart Application**

To build a high-performance **Shopping Cart Application in ASP.NET Core**, you must implement a architecture that cleanly separates data models, business logic, and session state persistence.

This comprehensive concept guide outlines the core architecture, data schemas, storage strategies, and programmatic workflows using standard **ASP.NET Core MVC** and **Entity Framework Core (EF Core)**.

**Step 1: Open Visual Studio 2019**

1. Open **Visual Studio 2019**

2. Click

**Create a new project**

**Step 2: Select Project Template**

Choose

**ASP.NET Web Application (.NET Framework)**

Click **Next**

# Step 3: Configure Project

Project Name

**ShoppingCartApp**

Location

Choose your folder.

Framework

**.NET Framework 4.7.2**

Click

**Create**

# Step 4: Select MVC Template

Choose

**MVC**

Authentication

**No Authentication**

Click

**Create**

Visual Studio creates the project.

**Project Structure**

ShoppingCartApp

Controllers

Models

Views

Content

Scripts

App_Start

Web.config

**Step 5: Create Product Model**

Right-click

**Models**

**Add**

**Class**

Name

**Product.cs**

Replace code with

```csharp
namespace ShoppingCartApp.Models
{
    public class Product
    {
        public int ProductID { get; set; }
        public string ProductName { get; set; }
        public decimal Price { get; set; }
    }
}
```

Save.

**Step 6: Create Cart Model**

Again

Right-click

**Models**

**Add**

**Class**

Name

**CartItem.cs**

Code

```csharp
namespace ShoppingCartApp.Models
{
    public class CartItem
    {
        public Product Product { get; set; }
        public int Quantity { get; set; }
    }
}
```

Save.

**Step 7: Create Home Controller**

Right-click

**Controllers**

**Add**

**Controller**

Choose

**MVC 5 Controller - Empty**

Name

**HomeController**

Click Add.

# Step 8: Add Product List

Replace controller code with

```csharp
using System.Collections.Generic;
using System.Web.Mvc;
using ShoppingCartApp.Models;

namespace ShoppingCartApp.Controllers
{
    public class HomeController : Controller
    {
        public ActionResult Index()
        {
            List<Product> products = new List<Product>()
            {
                new Product
                {
                    ProductID = 1,
                    ProductName = "Laptop",
                    Price = 55000
                },
                new Product
                {
                    ProductID = 2,
                    ProductName = "Mouse",
                    Price = 600
                },
                new Product
                {
                    ProductID = 3,
                    ProductName = "Keyboard",
                    Price = 1500
                },
                new Product
                {
                    ProductID = 4,
                    ProductName = "Monitor",
                    Price = 12000
                }
            };

            return View(products);
        }
    }
}
```

Build the project.

**Ctrl + Shift + B**

No errors should appear.

# Step 9: Create Index View

Right-click

**return View(products);**

Select

**Add View**

View Name

**Index**

Template

**Empty**

Click Add.

**Step 10: Display Products**

Replace code inside Index.cshtml

```cshtml
@model IEnumerable<ShoppingCartApp.Models.Product>

<h2>Products</h2>

<table class="table table-bordered">
    <tr>
        <th>ID</th>
        <th>Name</th>
        <th>Price</th>
        <th></th>
    </tr>

    @foreach (var item in Model)
    {
        <tr>
            <td>@item.ProductID</td>
            <td>@item.ProductName</td>
            <td>@item.Price</td>
            <td>
                @Html.ActionLink("Add To Cart", "AddToCart",
                    new { id = item.ProductID })
            </td>
        </tr>
    }
</table>
```

# Step 11: Create Cart Controller

Right-click

**Controllers**

**Add**

**MVC 5 Controller - Empty**

Name

**CartController**

**Step 12: Add Required Namespaces**

```csharp
using ShoppingCartApp.Models;
using System.Collections.Generic;
using System.Linq;
using System.Web.Mvc;
```

**Step 13: Create AddToCart Method**

Inside CartController

```csharp
public ActionResult AddToCart(int id)
{
    List<Product> products = new List<Product>()
    {
        new Product
        {
            ProductID = 1,
            ProductName = "Laptop",
            Price = 55000
        },
        new Product
        {
            ProductID = 2,
            ProductName = "Mouse",
            Price = 600
        },
        new Product
        {
            ProductID = 3,
            ProductName = "Keyboard",
            Price = 1500
        },
        new Product
        {
            ProductID = 4,
            ProductName = "Monitor",
            Price = 12000
        }
    };

    Product product = products.FirstOrDefault(x => x.ProductID == id);

    List<CartItem> cart;

    if (Session["Cart"] == null)
    {
        cart = new List<CartItem>();
    }
    else
    {
        cart = (List<CartItem>)Session["Cart"];
    }

    CartItem item =
        cart.FirstOrDefault(x => x.Product.ProductID == id);

    if (item == null)
    {
        cart.Add(new CartItem()
        {
            Product = product,
            Quantity = 1
        });
    }
    else
    {
        item.Quantity++;
    }

    Session["Cart"] = cart;

    return RedirectToAction("Index", "Home");
}
```

**Step 14: Create Cart Action**

Below AddToCart

```csharp
public ActionResult Index()
{
    List<CartItem> cart;

    if (Session["Cart"] == null)
    {
        cart = new List<CartItem>();
    }
    else
    {
        cart = (List<CartItem>)Session["Cart"];
    }

    return View(cart);
}
```

**Step 15: Create Cart View**

Right-click Index()

**Add View**

Name

**Index**

Template

**Empty**

**Step 16: Display Cart**

Replace code

```cshtml
@model IEnumerable<ShoppingCartApp.Models.CartItem>

<h2>Shopping Cart</h2>

<table class="table table-bordered">
    <tr>
        <th>ID</th>
        <th>Name</th>
        <th>Price</th>
        <th>Quantity</th>
        <th>Action</th>
    </tr>

    @foreach (var item in Model)
    {
        <tr>
            <td>@item.Product.ProductID</td>
            <td>@item.Product.ProductName</td>
            <td>@item.Product.Price</td>
            <td>@item.Quantity</td>
            <td>
                @Html.ActionLink(
                    "Remove",
                    "Remove",
                    "Cart",
                    new { id = item.Product.ProductID },
                    null)
            </td>
        </tr>
    }
</table>
```

# Step 17: Add View Cart Link

Open

**Views → Shared**

→ **_Layout.cshtml**

Find Navbar.

Add

```html
<li>@Html.ActionLink("Cart", "Index", "Cart")</li>
```

inside the menu.

# Step 18: Run Application

Press **F5**

Home page appears.

**Step 19: Test the Application**

Click

**Add To Cart** for any product.

Click

**Cart**

The selected products appear.

# Step 20: Add Remove Button (Optional)

Modify the cart table by adding another column:

```html
<th>Action</th>
```

Inside the foreach loop:

```cshtml
<td>
    @Html.ActionLink(
        "Remove",
        "Remove",
        new { id = item.Product.ProductID })
</td>
```

Add the following action to CartController:

```csharp
public ActionResult Remove(int id)
{
    List<CartItem> cart =
        Session["Cart"] as List<CartItem>;

    if (cart != null)
    {
        CartItem item =
            cart.FirstOrDefault(
                x => x.Product.ProductID == id);

        if (item != null)
        {
            cart.Remove(item);
        }

        Session["Cart"] = cart;
    }

    return RedirectToAction("Index");
}
```

# Step 21: Build and Run

1. Press **Ctrl + Shift + B** to build the project.

2. Press **F5** to run the application.

3. Test the following:

   o Add multiple products to the cart.

   o Increase the quantity by adding the same product again.

   o View the cart.

   o Remove products from the cart.

   o Verify that the grand total updates correctly.

**Output**

**Home Page**

| Product  | Price | Action      |
| -------- | ----: | ----------- |
| Laptop   | 55000 | Add To Cart |
| Mouse    |   600 | Add To Cart |
| Keyboard |  1500 | Add To Cart |
| Monitor  | 12000 | Add To Cart |

**Shopping Cart**

| Product | Price | Quantity | Total |
| ------- | ----: | -------: | ----: |
| Laptop  | 55000 |        1 | 55000 |
| Mouse   |   600 |        2 |  1200 |

**Grand Total: 56200**

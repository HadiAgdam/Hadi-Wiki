


### Pattern Routing
```c#
app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");
```

**URL:** `/Home/About` $\rightarrow$ calls `HomeController.cs` $\rightarrow$ `About()` method.



### Attribute Routing (Recommended for APIs & specific pages)

You place attributes directly above your controller classes and methods:

```c#
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    [HttpGet("{id}")] // Handles GET /api/products/5
    public IActionResult GetById(int id)
    {
        return Ok(new { Id = id, Name = "Widget" });
    }
}
```


---

## Use .html files

### Enable static routing

```c#
// Enable serving files from wwwroot (css, js, html, etc.)
app.UseStaticFiles();
```


### Method A: Direct Controller Action

```c#

[HttpGet("my-page")]
public IActionResult GetHtmlPage()
{
 // Path relative to wwwroot
var filePath = Path.Combine(Directory.GetCurrentDirectory(), "wwwroot", "index.html");
// Returns the physical HTML file with the correct MIME type
return PhysicalFile(filePath, "text/html");
}

```


#### Method B: Default Document Routing (Automatic `index.html`)

If you want visiting the root URL (`/`) or a subfolder URL to automatically load `index.html` from `wwwroot`:

Add `app.UseDefaultFiles();` **before** `app.UseStaticFiles()` in `Program.cs`:

```c#
app.UseDefaultFiles(); // Looks for index.html, default.html, etc.
app.UseStaticFiles();  // Serves the file
```

- Now, navigating to `https://localhost:xxxx/` will automatically display `wwwroot/index.html`


### Method C: Fallback Route

All of unmapped URL's would load the specified file:
```c#
app.MapFallbackToFile("index.html");
```


### To Controller
In `Program.cs` we can specify a action in a controller to redirect the not found traffic.

`Program.cs`:
```c#
app.MapFallbackToController("NotFound", "Home");

app.Run();
```
Make sure you run the code before `app.Run()`.


`HomeController.cs`:
```c#
public IActionResult NotFound()
{
    return View("/Views/NotFound.cshtml");
}
```
File path: `/Views/NotFound.cshtml`.

### To file:

`Program.cs`:
```c#
app.MapFallbackToFile("/Pages/NotFound.html");
```

File path: `/wwwroot/Pages/NotFound.html`.



# ASP.NET Core Dependency Injection Lifetimes

Dependency Injection (DI) in ASP.NET Core provides three service lifetimes:

## 1. Singleton

* **One instance** is created for the **entire application lifetime**.
* The same instance is shared across all requests and users.
* Suitable for:

  * Configuration services
  * Logging
  * Caching
  * Stateless shared services

```csharp
builder.Services.AddSingleton<IMyService, MyService>();
```

---

## 2. Scoped

* **One instance per HTTP request**.
* The same instance is reused throughout a single request.
* A new instance is created for each new request.
* Suitable for:

  * Database contexts (e.g., `DbContext`)
  * Business services that work within a request

```csharp
builder.Services.AddScoped<IMyService, MyService>();
```

---

## 3. Transient

* A **new instance is created every time** the service is requested.
* Best for lightweight, stateless services.

```csharp
builder.Services.AddTransient<IMyService, MyService>();
```

---

## Lifetime Comparison

| Lifetime      | Instance Created         | Shared Within      |
| ------------- | ------------------------ | ------------------ |
| **Singleton** | Once for the application | Entire application |
| **Scoped**    | Once per HTTP request    | Single request     |
| **Transient** | Every time requested     | Not shared         |

---

## Easy Way to Remember

* **Singleton** → **One app, one instance**
* **Scoped** → **One request, one instance**
* **Transient** → **Every injection, new instance**

### Memory Trick

```
Singleton → Application
Scoped    → Request
Transient → Injection
```

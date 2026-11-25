1. Dependency injection is a design pattern used to increase the maintainability and testability of applications by reducing the coupling between components. In ASP.NET core applications, DI is an essential part of the framework and is used to manage the lifetime and dependencies of services used through out the application.
2. DI is a technique where instead of creating an object directly inside your class, you let someone else (usually a framework like ASP.NET) give that object to you. With DI, instead of creating the object inside the class, you ask the framework (ASP.NET Core) to "inject" the object into the class. So, you don’t have to worry about creating or managing the objects.
3. ASP.NET core provides a built in DI container that can be used to manage the dependencies of an application. The DI container is responsible for creating and managing instances of services which are registered with the container when the application starts.


# Application which does not use DI

1. public class MyController : ControllerBase
{
    private readonly MyService_service;

    public MyController() 
    {what
        _service = new MyService();
    }

    public IActionResult Index ()
    {
        var data = _service.GetData();
        return Ok(data);
    }
}

2. We have a controller class and we have a constructor over here, which is MyController inside the constructor because we want to use the service MyService class later on in the Index action method (at line 19). We are instantiating the class inside the controller (at line 14). Now this class would have to be managed by the controller and later on, if the class name changes or we have to implement a different implementation of MyService(), then we have to change all the controllers or classes that uses this my service.

# Application which use DI

3. services.AddScoped<IMyService, MyService>(); 

public class MyController : ControllerBase
{
    private readonly IMyService_service;
    // Inside this controller, we have a service that follows IMyService rules. We will set it one time when the controller is created. After that, it cannot be changed.
    
    public MyController(IMyService service) 
    {
        _service = service ;
    }

    public IActionResult Index ()
    {
        var data = _service.GetData();
        return Ok(data);
    }
}
4. Instead of creating the instance within the class. We can easily inject the interface MyService inside our application in the Program.cs file and we also provide the implementation detail of the interface of what we are trying to implement. (at line 28). So MyService has been injected inside the application, inside the controller or any controller that we want to use this MyService, we can easily inject the service as a parameter to the controller or a parameter to the method, and then we can use this service throughout the application. For example, in this index action method.

5. private readonly IMyService _service;

private → It means only this class (MyController) can use _service.
(Other classes can't directly access it.)

readonly → It means you can only set _service once (usually inside constructor).
After that, you can't change it anywhere else. (It becomes "read-only".)

IMyService → This is the type (like a blueprint or a rule book).
It says: "this variable must be an object that follows the IMyService rules."

_service → This is just the name of the variable.
(You can name it anything, but _service is a common naming style.)

6. If I want a new service, I will just change MyService() to NewService() manually. No big deal! Imagine You have 30 controllers. All 30 controllers have written this:
_service = new MyService();
Now your boss says, "Hey, we will not use MyService, we will use NewService from today." What happens?
You have to open all 30 files ❌
Find and change every MyService() to NewService() ❌
Very boring, slow, and can easily make mistakes ❌
With Dependency Injection. If you use DI. You only change it one time in Program.cs:
services.AddScoped<IMyService, NewService>();
Done! You don't touch any controller. Everything will automatically start using NewService!

7. Real Advantages of DI:

Without DI (Manual)	                                                                                    With DI (Automatic)
Change code in every file manually ❌	                                                               Change code only once ✅
Harder to maintain ❌	                                                                               Easy to maintain ✅
Hard to test ❌	                                                                                       Easy to test (you can inject fake services) ✅
If service depends on other things (ex: Database), you also have to manage them manually ❌	           DI automatically handles dependencies ✅

8. builder.Services.AddDbContext<NZWalksDbContext>(options => options.UseSqlServer(builder.Configuration.GetConnectionString "NZWalksConnectionString")));
Using this, we have injected our Dbcontext class and we have also provided the connection string to this Dbcontext class and the application will manage all the instances of this Dbcontext class whenever we call it inside controllers or repositories.
var builder = WebApplication.CreateBuilder(args);

// Add services to the container.

builder.Services.AddControllers();
// Learn more about configuring Swagger/OpenAPI at https://aka.ms/aspnetcore/swashbuckle
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();

app.UseAuthorization();

app.MapControllers();

app.Run();

1. This is a builder, this is used to control things like dependency injection, provide services in various things that you actually add to your program like a module
2. To rename multiple same words in visual studio tab ctrl + r + r and this will rename all the word in the code.
3. One Solution have multiple projects. Inside solution we can consume projects
4. Project.cs file is the first thing executed. Its the entry point. When programs runs the Program.cs file is executed first
5. In this file we add dependencies to our application (Program.cs)
6. At the starting of the file we are creating a builder object and then we are injecting services inside our application
7. Then finally we build the builder object
8. Another important function of Program.cs file is to configure the http request pipeline. By using the request pipeline we add middleware (a software) that assemble into a application pipeline to handle request and responses. After configuring the middleware, we then run our application.
9. In the csproj file we have all the Property group. If we add NuGet packages to this project they will also end up as a reference over here. 
10. The launchSettings.json file it have launch related information. Stuff like profiles and the application URLs reside inside the launchSetting.json
11. Inside the controller folder we have different endpoints for our application
12. appsettings.json file is used to store configuration for our application stuff like log levels, which is for example, the default is information and other stuff like connection strings to the database also reside in this appsettings.json file. Environment specific variables also reside here.
# DbContext Class 
1. It is a class that represents a session with a database and provide a set of APIs for performing database operations. This is part of the entity framework core packages. 
2. The DbContext class is responsible for maintaining a connection to the database, tracking changes to data and performing database operations such as squaring, inserting, updating and deleting. 
3. It also provide a way to define the database schema using entity classes or domain classes that we build in Walks project, which maps directly to database tables. 
4. So we can say that the Dbcontext class is a bridge between your domain model classes and the database. ( Controller <------->  DbContext <-------> Database )
5. DbContext class is the primary class that is responsible for interacting with the database and performing CRUD operations on our database tables.
6. Whenever a call comes to a controller, in order to get or retrieve a data, the controller will have to talk to the DbContext and it is the responsibility of the DbContext to get the data from the database and provide it back to the controller.
7. NZWalksDbContext class inherits from the DB Context class, which is part of the entity framework core packages. (public class NZWalksDbContext: DbContext)
8. A DB set is a property of DB context class that represents a collection of entities in the database.
9. public NZWalksDbContext(DbContextOptions dbContextOptions): base(dbContextOptions) // In this constructor we want to pass the DB options because we later on want to send our own connections through the program.cs file. Then we will pass dbContextOptions to the base class 'base(dbContextOptions)'
10. After the constructor we want to create DB sets. A DB set is a property of DB context class that represents a collection of entities in the database.
11. In NZWalks application, we have three entities or domain models. (Difficulty, Region, Walk). So we want to represent the sets of each of these entities as a collection in our database and we will do that inside the DB context. So we will create property for each one of them.
12. When we run entity framework core migrations, these three properties will create tables inside this new database (Database name will be define in the ConnectionStrings in appsettings.json file). 
public DbSet<Difficulty> Difficulties {get; set;}
public DbSet<Walk> Walks { get; set; }
public DbSet <Region> Regions { get; set; }




# Detailed Explanation

You wrote this in `Program.cs`:
builder.Services.AddDbContext<NZWalksDbContext>(options =>  options.UseSqlServer(builder.Configuration.GetConnectionString("NZWalksConnectionString")));

How does the connection string actually reach DbContextOptions? Here's the detailed behind-the-scenes:

**Step 1:**  
When ASP.NET Core starts the application, it reads your `Program.cs` file.

**Step 2:**  
`AddDbContext<NZWalksDbContext>(options => {...})` is called.
At this moment ASP.NET Core internally creates a `DbContextOptionsBuilder` object for your `NZWalksDbContext`.

**Step 3:**  
Inside the lambda `(options => {...})`, you are configuring this builder. Specifically: options.UseSqlServer("YourConnectionString");
- `UseSqlServer` sets the connection string and tells EF Core "use SQL Server database". Now `options` (which is a `DbContextOptionsBuilder`) holds all the settings (like which database, which connection string, etc.).

**Step 4:**  
ASP.NET Core then automatically builds the `DbContextOptions` object using this builder. `DbContextOptions` now has all your configuration inside it (including your connection string).

Then what happens? When you request `NZWalksDbContext` somewhere (for example, in a Controller):

public class SomeController
{
    private readonly NZWalksDbContext dbContext;

    public SomeController(NZWalksDbContext dbContext)
    {
        this.dbContext = dbContext;
    }
}

- The dependency injection system will create an object of `NZWalksDbContext`.
- And pass that built `DbContextOptions` object into the constructor automatically.

Remember your constructor in `NZWalksDbContext`:

public NZWalksDbContext(DbContextOptions dbContextOptions) : base(dbContextOptions)
{ }

So here,
- `dbContextOptions` already contains the connection string inside!
- `base(dbContextOptions)` passes it to the `DbContext` class, and EF Core internally uses it to connect to the database.

Very small diagram:

Program.cs:
    AddDbContext -> options.UseSqlServer(connectionString)
                       |
                       v
                DbContextOptions created
                       |
                       v
       Dependency Injection -> NZWalksDbContext(DbContextOptions)
                       |
                       v
               EF Core uses connectionString


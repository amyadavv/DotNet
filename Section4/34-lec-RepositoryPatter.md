# Repository Pattern

1. Design pattern that helps to separate the data access layer from the rest of the application.
2. Provides a standard interface, which is an interface class for accessing and managing data in a data store such as database without exposing the underline implementation detail to the rest of the application.
3. Repository pattern typically involves creating abstraction layer between the application and the data store, which is implemented by a concrete repository class.
4. The repository class is responsible for performing CRUD operations on the data store and it exposes a set of methods that the application can use to interact with the data.
5. If you remember your application now, we have the Dbcontext class injected inside the controller and it is the controller who is directly talking to the database using that dbcontext. That is also a wrong practice and using the repository design pattern, we can eliminate that by adding an abstraction layer in between.
6. Repositories can be added in between the controller and the database so that all the operations on the database is then handled by the repository.
7. The dbcontext class is injected inside the repository rather than inside the controller, and it is the repository that then injects in the controller. So controller will use the repository instead of using the dbcontext. 


                                       DbContext
                                           |
                                           |
                                           |
                    Controller -----  Repository ----- Database

8. By doing that, the controller now has no awareness of what's being called through the Dbcontext, whether it's a SQL server database or a MongoDB database, it has no idea about it. Controller is just using the interface method exposed by the interface repository and the implementation is hidden behind the implementation repository.
9. Using that, you can switch the logic and the data stores behind the implementation repository. For example, you can use entity framework core to store your changes in a database, or you can just use an in-memory database by creating another implementation repository. All of those changes are happening behind the repository and the controller has no knowledge about the data stores at all.

# Benefits

1. Decoupling the data access layer from the rest of the application, which makes it easier to maintain and test the application.
2. Providing a standard interface for accessing data which improves the Consistency and readability of the code. Now every connection to the database goes through the repository. 
3. We can also improve the Performance of the application by using caching, batching, or other optimization techniques.
4. supporting multiple data sources, which allows the application to switch between different data sources without affecting the application logic. Multiple data sources (switching)
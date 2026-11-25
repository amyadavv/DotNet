# DTOs - Data Transfer Objects
1. DTOs are object used to transfer data between different layers of components of an application.
2. Typically contain a subset of the properties in the domain model and are designed to be used for a specific purpose, such as transferring data over the network or between layers of an application.
3. For example transferring data over a network.
4. Client  ------  DTO  ------  API  ------  Domain Model  ------ Database

We have api over here, we have database which talks to the api and we also have the client which is calling this API to get some data or to add some data. Because of entity framework core, domain models have a mapping between the tables in the database and the domain models that are used internally in the application. When we use entity framework to talk to a table, it gives us the domain model because the Dbcontext class only knows about the domains models, but because we have the domain models within the API, it is a good practice to add a layer of DTOs that is the data transfer objects and that is what we send back to the client. So we never send the domain model back to the client, but we send the DTO instead. So once the Dbcontext class is able to provide us with the list of domain model or the information about the domain model, we then do a mapping between the domain model and the DTO so that we can send the DTO back to the client.
If the client wants to add a new resource, then we receive a DTO, which then we convert from DTO to domain model and then we send the domain model using entity framework core to the database.


What are the advantages of using DTOs if they are exactly the same as domain models?

1. Domain models can have slightly more properties and slightly different named properties, which we don't want to expose to our client.
2. Separation of concerns. - The domain objects are usually tightly coupled with a database schema, as we just know, because Entity framework core uses them to talk to a database, whereas DTOs can be designed to match the business requirements. This separation ensures that the domain objects are not tightly coupled with the API or the view layer.
3. Performance - As DTO allow us to retrieve only the data that is needed.
4. Security - As DTOs can help us to improve security by limiting the amount of data that is exposed to the client. By only exposing the necessary data, you can reduce the risk of exposing sensitive data to unauthorized users.
5. Versioning - As it can be versioned independently, allowing you to update the API without breaking the existing clients. If you were to make a change to a domain model which was exposed to the client, it wasn't possible because then the client wouldn't be able to send the information again because you now have breaking changes. But now we can change the domain model but can keep the DTO the same so that we have not breaking the contract between the client and the api. But we can still play around with the domain models within our application.
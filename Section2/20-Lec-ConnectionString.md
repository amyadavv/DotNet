1. Create a new object called 'ConnectionStrings' and inside this object we will have key value pairs and we will first specify the key, which is the name of the connection string and this is something that you can give yourself through for your application. So 'NZWalksConnectionString' is the name, and then we have to provide a value for this connection string. The connection string needs the name of the server that it can connect to, and this will be the server name that you installed (localHost). We will provide a database name for the application and when we run entity framework core migrations, we will create a new database for this application.

    "NZWalksConnectionString": "Server=localhost;Database=NZWalksDb;Trusted_Connection=True;TrustServerCertificate=True"

    The last two value as per dotnet 6 and 7. We need to use these two information as well so that we can connect to a local instance of sql server.

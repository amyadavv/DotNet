# Implementation Repository Pattern

1. To implement repositories, we first have to create an interface for the repository. If we want to create a repository for the region domain model which we will use to just interact with the regions table, we would need a IRegionRepository and after that, after we created the interface which exposes the CRUD operations, we would then need an implementation, a concrete implementation that would actually implement all these methods for us (CRUD). 
2. Now the implementation can be named as a SQLRegionRepository. If we are using SQL server with entity framework and if it was a in-memory region repository, that means we must be using an in-memory database. Then swapping becomes really easy once we have used the repository pattern.
3. Now create a repository folder. Inside the repositories folder we would need an interface for the region repository. (IRegionRepository)
4. using NZWalks.API.Models.Domain;

namespace NZWalks.API.Repositories
{
    public interface IRegionRepository
    {
        // In this interface, I want definitions of the methods that I want to expose.
        // For example, at the moment our controller method in the region controller has five methods the GetAll, GetById, Create, Update and delete. So all these operations that are being operated on a database by using the Dbcontext needs to be through a repository now. 
        // So we need five definition of methods that we will later on implement in a concrete class.

        Task<List<Region>> GetAllAsync();

        // This method would return the list of regions back from the database and we will get the list of domain models back from the database using the dbcontext.
        // If I call this method, this method will be then used by the controller because we expose the interface to the application, not the implementation 
        // Now that we have a definition, lets create an implementation that will implement this interface. Inside the repositories folder.
    }
}
5. using Microsoft.EntityFrameworkCore;
using NZWalks.API.Data;
using NZWalks.API.Models.Domain;

namespace NZWalks.API.Repositories
{
    // This will implement the interface IRegionRepository because it has to define the definition of that interface.
    // To implement the interface use ctrl+. and it gives you the structure of the method.

    public class SQLRegionRepository : IRegionRepository
    {
        // This is the SQLRegionRepository, we want to make use of the Dbcontext class, which we were previously using inside the controller. But now we have to use this through the repository.
        // So inject the Dbcontext class in the repository.

        private readonly NZWalksDbContext dbContext;
        public SQLRegionRepository (NZWalksDbContext dbContext)
        {
            this.dbContext = dbContext;
        }
        public async Task<List<Region>> GetAllAsync()
        {
            return await dbContext.Regions.ToListAsync();
            // We wont be accessing the Dbcontext class directly. It is the repository's job to access the dbcontext or the data source through that dbcontext.
        }

        // We can now inject this inside our application in the program.cs file.
    }
}
 6. builder.Services.AddScoped<IRegionRepository, SQLRegionRepository>();
// Whenever I want to use IRegionRepository, I want to pass the concrete  implementation of SQLRegionRepository. Above line basically injects the IRegionRepository with the implementation SQLRegionRepository.

7. So if my controller now wants to use the repository, I will now have to inject IRegionRepository in the constructor. Because IRegionRepository has method GetAllAsync(), I can use this method inside the controller.

8. Previously I was using the Dbcontext class to get the regions back, but now call the interface instead.

9. using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Storage.Json;
using NZWalks.API.Data;
using NZWalks.API.Models.Domain;
using NZWalks.API.Models.DTO;
using NZWalks.API.Repositories;

namespace NZWalks.API.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class RegionsController : ControllerBase
    {
        private readonly NZWalksDbContext dbContext;
        private readonly IRegionRepository regionRepository;

        public RegionsController(NZWalksDbContext dbContext, IRegionRepository regionRepository)
        {
            this.dbContext = dbContext;
            this.regionRepository = regionRepository;
        }


        // GET ALL REGION
        // GET: https//localhost:portnumber/api/regions
        [HttpGet]
        public async Task<IActionResult>  GetAll()
        {
            var regionsDomain = await regionRepository.GetAllAsync();
            var regionDTO = new List<RegionDTO>();
            foreach(var regionDomain in regionsDomain)
            {
                regionDTO.Add(new RegionDTO()
                {
                    Id = regionDomain.Id,
                    Code = regionDomain.Code,
                    Name = regionDomain.Name,
                    RegionImageUrl = regionDomain.RegionImageUrl
                });
            }
            return Ok(regionDTO);
        }
    }
}

10.  var regionsDomain = await regionRepository.GetAllAsync(); - The GetAllAsync() is the method which we declare in the interface (IRegionRepository) and we use that one. Then it is define in the SQLRegionRepository. Now it is the responsibility of the concrete class SQLRegionRepository to give me the information from the database.
11. IRegionRepository is basically a layer between our data and the application.
12. Now our controller calls the repository and the repository is the one who is responsible to talk to the database.


# Switch to Different Database

1. The application is using SQL server implementation using dbcontext let say the business decides not to use SQL server, but to use an in-memory database.

2. What the developer will now do is to create another concrete implementation of the IRegionRepository. Creates a new class in the repository folder, that class also implements the IRegionRepository because it has to get the list of regions back using this method. To implement the repository press control + . to get the method extract.
3. I am passing the or returning a hard coded list back.
4. using NZWalks.API.Models.Domain;

namespace NZWalks.API.Repositories
{
    public class InMemoryRegionRepository : IRegionRepository
    {
        public async Task<List<Region>> GetAllAsync()
        {
            return new List<Region>
             {
                new Region()
                {
                    Id = Guid.NewGuid(),
                    Code = "Random",
                    Name = "fgf",
                    RegionImageUrl = "ggfd"

                }
            };
        }
    }
}

5. So what the developer would do is to go to the Program.cs file and instead of this implementation ( 6. builder.Services.AddScoped<IRegionRepository, SQLRegionRepository>();) I will change this to the InMemoryRegionRepository() implementation. That means whenever somebody or the controller calls the IRegionRepository(), instead they will get the implementation InMemoryRegionRepository(). (builder.Services.AddScoped<IRegionRepository, InMemoryRegionRepository>();)
6. When you run the getAll api you will get region coming from InMemoryRegionRepository not the SQLRegionRepository implementation.
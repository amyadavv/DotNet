# Async Programming

1. In traditional Synchronous programming, the program execution is blocked while waiting for a long running operation to complete.
2. This can result in Poor performance and unresponsive user interfaces. (Synchronous programming)
3. Asynchronous programming allows the program to continue executing other tasks while waiting for the long running operation to complete, resulting in a much better performance and responsiveness.
4. In ASP.NET Core, asynchronous programming is achieved using the Async and await keywords which enable developers to write code that can be executed asynchronously.
5. This means that long running operations such as database queries, file operations, etc will be executed without blocking the main thread, allowing the application to continue responding to user requests. 
4. By using asynchronous programming techniques, ASP.NET Core applications can handle way more requests simultaneously, thereby improving scalability and responsiveness and providing a way better user experience.
5. Following is a operation that is going to the database, and if there's a network failure or there's a unresponsiveness over the line, this call can lead to longer wait times, therefore decreasing the performance of our application because of this main thread will be blocked, we won't be able to take further request.


// GET ALL REGION
// GET: https//localhost:portnumber/api/regions
[HttpGet]
public IActionResult GetAll()
{
    var regionsDomain = dbContext.Regions.ToList(); 
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

6. The return type for asynchronous methods in ASP.NET Core is a task. Every return type is wrapped inside a task. ( public async Task<IActionResult>  GetAll() ). 
7. This database call is going to the database through the DB context(var regionsDomain = dbContext.Regions.ToList(); ), and we have to make sure that this is an asynchronous call. So we will use the await keyword just before we start this call.
8. Entity framework core provides us a extension method that is to list async. So whenever the data is back to us, it will create a list from that data. The method is "ToListAsync()"
9. Now this call is Asynchronous ---- var regionsDomain = await dbContext.Regions.ToListAsync();
10. public async Task<IActionResult>  GetAll()
{
    var regionsDomain = await dbContext.Regions.ToListAsync();
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
11. using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Storage.Json;
using NZWalks.API.Data;
using NZWalks.API.Models.Domain;
using NZWalks.API.Models.DTO;

namespace NZWalks.API.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class RegionsController : ControllerBase
    {
        private readonly NZWalksDbContext dbContext;

        public RegionsController(NZWalksDbContext dbContext)
        {
            this.dbContext = dbContext;
        }


        // GET ALL REGION
        // GET: https//localhost:portnumber/api/regions
        [HttpGet]
        public async Task<IActionResult>  GetAll()
        {
            var regionsDomain = await dbContext.Regions.ToListAsync();
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


        // GET Single Region (Get Region By Id)
        // GET: https//localhost:portnumber/api/regions/{id}
        [HttpGet]
        [Route("{id:guid}")]
        public async Task<IActionResult> GetById([FromRoute] Guid id)
        {
            //var region = dbContext.Regions.Find(id);
            // Get Region Domain Model From Database
            var regionDomain = await dbContext.Regions.FirstOrDefaultAsync(x => x.Id == id);
                
            if(regionDomain == null)
            {
                return NotFound(); 
            }

            // Map/Convert Region Domain Model to Region DTO
            var regionDTO = new RegionDTO
            {
                Id = regionDomain.Id,
                Code = regionDomain.Code,
                Name = regionDomain.Name,
                RegionImageUrl = regionDomain.RegionImageUrl
            };

            // Return DTO Back To The Client
            return Ok(regionDTO);
        }


        // POST To Create new region
        // POST: https//localhost:portnumber/api/regions
        [HttpPost]
        public async Task<IActionResult> Create([FromBody] AddRegionRequestDto addRegionRequestDto)
        {
            // Map or convert DTO to domain model

            var regionDomainModel = new Region
            {
                Code = addRegionRequestDto.Code,
                Name = addRegionRequestDto.Name,
                RegionImageUrl = addRegionRequestDto.RegionImageUrl
            };

            // Use domain model to create region
            await dbContext.Regions.AddAsync(regionDomainModel);
            await dbContext.SaveChangesAsync();

            // Map domain model back to dto as we want to send back to client the changes

            var regionDto = new RegionDTO
            {
                Id = regionDomainModel.Id,
                Name = regionDomainModel.Name,
                Code = regionDomainModel.Code,
                RegionImageUrl = regionDomainModel.RegionImageUrl
            };

            return CreatedAtAction(nameof(GetById), new { id = regionDto.Id }, regionDto);
            

        }


        // Update region
        // PUT: https//localhost:portnumber/api/regions/{id}
        [HttpPut]
        [Route("{id:guid}")]
        public async Task<IActionResult> Update([FromRoute] Guid id, [FromBody] UpdateRegionRequestDto updateRegionRequestDto)
        {
            var regionDomainModel = await dbContext.Regions.FirstOrDefaultAsync(x => x.Id == id);

            // Check if region exists
            if(regionDomainModel == null)
            {
                return NotFound();
            }

            // Map dto to Domain model
            regionDomainModel.Code = updateRegionRequestDto.Code;
            regionDomainModel.Name = updateRegionRequestDto.Name;
            regionDomainModel.RegionImageUrl = updateRegionRequestDto.RegionImageUrl;

            await dbContext.SaveChangesAsync();
            // This directly called because this entity was already tracked by the dbcontext over above (var regionDomainModel = dbContext.Regions.FirstOrDefault(x => x.Id == id);), so I dont have to call the update method. I can just directly call SaveChanges(); 

            // Convert Domain model to DTO

            var regionDto = new RegionDTO
            {
                Id = regionDomainModel.Id,
                Name = regionDomainModel.Name,
                Code = regionDomainModel.Code,
                RegionImageUrl = regionDomainModel.RegionImageUrl
            };

            return Ok(regionDto);

        }


        // Delete region
        // DELETE: https//localhost:portnumber/api/regions/{id}
        [HttpDelete]
        [Route("{id:Guid}")]
        public async Task<IActionResult> Delete([FromRoute] Guid id) 
        {
            var regionDomainModel  = await dbContext.Regions.FirstOrDefaultAsync(x => x.Id == id);

            if(regionDomainModel == null)
            {
                return NotFound();
            }

            //Delete region
            dbContext.Regions.Remove(regionDomainModel);
            // Remove method doesn't have a remove async method. Remove is still synchronous, so we can't have a remove async method.
            await dbContext.SaveChangesAsync();

            //return deleted Region back
            // map domain to DTO

            var regionDto = new RegionDTO
            {
                Id = regionDomainModel.Id,
                Name = regionDomainModel.Name,
                Code = regionDomainModel.Code,
                RegionImageUrl = regionDomainModel.RegionImageUrl
            };

            return Ok(regionDto);
        }
    }
} 
12.  dbContext.Regions.Remove(regionDomainModel);
 // Remove method doesn't have a remove async method. Remove is still synchronous, so we can't have a remove async method.

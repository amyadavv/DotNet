1. We have injected dbcontext using dependency injection, we can now use that dbcontext inside the controller through constructor injection.

2. using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using NZWalks.API.Data;
using NZWalks.API.Models.Domain;

namespace NZWalks.API.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class RegionsController : ControllerBase
    {
        private readonly NZWalksDbContext dbContext;

        public RegionsController (NZWalksDbContext dbContext)
        {
            this.dbContext = dbContext;
        }

        // GET ALL REGION
        // GET: https//localhost:portnumber/api/regions
        [HttpGet]
        public IActionResult GetAll ()
        {
            var region = dbContext.Regions.ToList();
            return Ok(region);
        }
    }
}

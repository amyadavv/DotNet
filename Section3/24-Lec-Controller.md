1. Controller by default has the root attribute and the API controller attribute as well. So the API controller [ApiController] attribute will tell this application that this controller is for API use. So it automatically know for example validates the model state and gives a 400 response back to the caller.

using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;

namespace NZWalks.API.Controllers
{
    [Route("api/[controller]")]    //[Route("api/regions")]    Both are the same thing

    [ApiController]
    public class RegionsController : ControllerBase
    {
    }
}

2. The route attribute is basically defining the root whenever a user enters this root (api/[controller]) along with the application URL, it will be pointed to the regions controller. For example our application is https://localhost:1234/api/regions and when somebody enters this URL, it will be pointed to the regionsController.

3. IActionResult is a response type.

4. using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using NZWalks.API.Models.Domain;

namespace NZWalks.API.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class RegionsController : ControllerBase
    {
        [HttpGet]
        public IActionResult GetAll ()
        {
            var region = new List<Region>
            {
                new Region
                {
                    Id = Guid.NewGuid(),
                    Name = "Auckland Region",
                    Code = "AKL",
                    RegionImageUrl = "https://images.pexels.com/photos/814285/pexels-photo-814285.jpeg"
                },
                new Region
                {
                    Id = Guid.NewGuid(),
                    Name = "Wellington Region",
                    Code = "WLG",
                    RegionImageUrl = "https://images.pexels.com/photos/814285/pexels-photo-814285.jpeg"
                }
            };
            return Ok(region);
        }
    }
}

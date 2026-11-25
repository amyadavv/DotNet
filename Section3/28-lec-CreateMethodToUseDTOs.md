1. public IActionResult GetById([FromRoute] Guid id)
{
    //var region = dbContext.Regions.Find(id);
    var region = dbContext.Regions.FirstOrDefault(x => x.Id == id); 

    if(region == null)
    {
        return NotFound(); 
    }

    return Ok(region);
}
2. This is coupling our domain model to the API view layer, which is not a good practice. 
        //var region = dbContext.Regions.Find(id);
3. We have  to convert this to DTOs and expose DTOs to the outside layer.
 - Get data from database - Domain models
 - Map domain Models to DTOs
 - Return DTOs. We never return domain models back to the client.
 4. DTOs should be add in the Models folder
 5. public IActionResult GetAll()
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

[HttpGet]
[Route("{id:guid}")]

public IActionResult GetById([FromRoute] Guid id)
{
    //var region = dbContext.Regions.Find(id);
    // Get Region Domain Model From Database
    var regionDomain = dbContext.Regions.FirstOrDefault(x => x.Id == id); 

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
6. We have modified our existing methods and we are now using DTOs to expose to our clients and domain models to map our application to the database.
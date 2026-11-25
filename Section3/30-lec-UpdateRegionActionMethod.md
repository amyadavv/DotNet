// Update region
// PUT: https//localhost:portnumber/api/regions/{id}
[HttpPut]
[Route("{id:guid}")]
public IActionResult Update([FromRoute] Guid id, [FromBody] UpdateRegionRequestDto updateRegionRequestDto)
{
    var regionDomainModel = dbContext.Regions.FirstOrDefault(x => x.Id == id);

    // Check if region exists
    if(regionDomainModel == null)
    {
        return NotFound();
    }

    // Map dto to Domain model
    regionDomainModel.Code = updateRegionRequestDto.Code;
    regionDomainModel.Name = updateRegionRequestDto.Name;
    regionDomainModel.RegionImageUrl = updateRegionRequestDto.RegionImageUrl;

    dbContext.SaveChanges();
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
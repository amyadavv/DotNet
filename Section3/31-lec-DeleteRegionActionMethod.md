// Delete region
// DELETE: https//localhost:portnumber/api/regions/{id}
[HttpDelete]
[Route("{id:Guid}")]
public IActionResult Delete([FromRoute] Guid id) 
{
    var regionDomainModel  = dbContext.Regions.FirstOrDefault(x => x.Id == id);

    if(regionDomainModel == null)
    {
        return NotFound();
    }

    //Delete region
    dbContext.Regions.Remove(regionDomainModel);
    dbContext.SaveChanges();

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
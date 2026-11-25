1. [HttpGet]
[Route("{id:guid}")]

public IActionResult GetById([FromRoute] Guid id)
{
    //var region = dbContext.Regions.Find(id);
    var region = dbContext.Regions.FirstOrDefault(x => x.Id == id); 

    if(region == null)
    {
        return NotFound();
    }

    return Ok(region);
}
2. The id should not be int it will be the guid as we store id as guid. (public IActionResult GetById([FromRoute] Guid id))
3. To receive the id we have to use route attribute. Make sure the id in the route should match with the id in the IActionResult function parameter.
4. When we pass the id after the URL of the controller (URL- https://localhost:5000/api/regions/{id}), it will get mapped from the route to input parameter. We know id is coming from the route, we can use the property FromRoute in front of input parameter. (public IActionResult GetById([FromRoute] Guid id)).
5. Another way to get a single region back is using FirstOrDefault, which is a linkq method.  You can do this for id but you can also do this for other properties such as if you want to search for name or the code. Only if you are passing code/name in the route.
6. The find method only takes the primary key, so you can't use the find method for other properties such as code, name or image. 
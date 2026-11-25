Swagger is a popular tool for documenting APIs and providing a user friendly interface for testing and exploring the APIs.
Whenever you create a controller, you have to suffix with the keyword controller so that ASP.NET core can recognize that this is a controller.

When you point to this URL, with the get keyword, it will come to this action method and will execute this code
// GET: https://localhost:portnumber/api/students
[HttpGet]
public IActionResult GetAllStudents()
{
    string[] studentNames = new string[] { "amit", "aditi", "sharma", "yadav" };
    return Ok(studentNames);

}

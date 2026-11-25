1. Auto mapper is just another way to do this stuff. It is not a complete replacement, so if you want, you can stick with this mapping of the DTOs to domain models or the domain models to DTOs. Thats is totally fine. But if you want, you can also reduce these lines of code by a single line of code by using auto mapper.
2. To use auto mapper we have to install the packages for auto mapper.
3. We now want to create the mapping profiles for our models. For that I will create a folder inside the project. Inside the mappings folder I will create a profiles class, or you can also call it a mapping class. 
4. Now the AutoMapperProfiles class inherits from a class from the Auto mapper package, which is the profile class (Profile).
5. After that we have to create a constructor and inside the constructor we will now create the mappings. Mappings can be created using the method create map. CreateMap<> needs two types, it needs a type of source and it needs a type of destination. So we are taking a source and a destination and auto mapper is doing the mapping for us. If the name of the property is exactly match with each other, auto mapper will just map the value of that property from the source to the destination. And if they are not matching, we have to explicitly define the mapping between those properties in these two models.
6. For an example, if we had two classes, for example a user DTO and a user domain model, and they had some properties, for example, a property called full name in the user DTO, and a property called full name in the user domain model, then in the auto mapper we could define the property mapping like, let's say we need the source first. So let's say there is a conversion happening from DTO to domain model. So UserDTO as a source and the destination is UserDomain model. - CreateMap<UserDTO, UserDomain>();
7. Now auto mapper does give us a smart way to do a reverse mapping as well. In case you also want to map domain model to DTO. Either you can just type it one more time and change the values around, or you can use a reverse map. Using a reverse map, auto mapper automatically defines a reverse map between the source and destination as well. -  CreateMap<UserDTO, UserDomain>().ReverseMap();

using AutoMapper;

namespace NZWalks.API.Mappings
{
    public class AutoMapperProfiles : Profile
    {
        public AutoMapperProfiles()
        {
            CreateMap<UserDTO, UserDomain>().ReverseMap();
        }
    }

    public class UserDTO
    {
       public string FullName { get; set; }
    }
    public class UserDomain
    {
        public string FullName { get; set; }
    }
}



8. What will happen if the property names were slightly different in each of the domain and DTO, which could totally be possible. So let's say our user domain model had the property of name, and that's the full name as well. And the DTO has the property of full name. Now auto mapper won't be able to map the property because the property names don't match and hence it won't be able to map the full name to the name property. In the domain model we have to do something different over here, before the reverse map I want to also use this method for member, and we will say for member x such that x dot name which 
1. public string? RegionImageUrl { get; set; }, By adding '?' we can add null values in the RegionImageUrl
2. Navigation properties tells entity framework core that a walk will have a difficulty and region, and above have DifficultyId and RegionId for it. So when we run entity framework code migrations, entity framework core is smart enough to give all those relationships that we want and create the database and tables as per the relationships that we defining now. 
   public class Walk
    {
        public Guid Id { get; set; }
        public string Name { get; set; }
        public string Description { get; set; }
        public double LengthInKm { get; set; }
        public string? WalkImageUrl { get; set; }
        public Guid DifficultyId { get; set; }
        public Guid RegionId { get; set; }
   
        // Navigation properties
        public Difficulty Difficulty { get; set; }
        public Region Region { get; set; }
        
    }
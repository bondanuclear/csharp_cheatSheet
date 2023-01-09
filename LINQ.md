# Use of LINQ
## LINQ - Language-Integrated Query - являє собою мову запитів до джерела даних. Існує декілька видів LINQ, наприклад: 
- LINQ to Objects
- LINQ to Entities
- LINQ to XML
- LINQ to DataSet
- Parallel LINQ (PLINQ)
# https://metanit.com/sharp/tutorial/15.1.php - посилання на більше функцій
```csharp
public class StarRating
{
  public string Name {get; set;}
  public int Stars {get; set;}
}
public class Game
{
  public string Name {get; set;}
  public Date ReleaseDate {get;  set;}
  public int Score {get; set;}
  public Game(string name, Date date, int score)
  {
    this.Name = name;
    this.ReleaseDate = date;
    this.Score = score;
  }
}

public class TestLinq
{
  static void main(string[] args)
  {
    // Тут може бути будь-яка з колекцій, що реалізує IEnumerable
    List<int> numbers = new List<int> {1,2,3,4,5,6,7,8,9,10};
    List<Game> games = new List<Games> {
      new Game{Name = "Game1", ReleaseDate = new Date(2017,3,24), Score = 9 },
      new Game{Name = "Game2", ReleaseDate = new Date(2019,8,12), Score = 7 },
      new Game{Name = "Game3", ReleaseDate = new Date(2020,9,5), Score = 10 }
      new Game{Name = "Game4", ReleaseDate = new Date(2022,3,24), Score = 6 },
      new Game{Name = "Game5", ReleaseDate = new Date(2021,8,12), Score = 9 },
      new Game{Name = "Game6", ReleaseDate = new Date(2018,9,5), Score = 9 }
    }
    //////
    var isOver5 = numbers.Where(number => number > 5); // фільтр вибірки, для отримання підмножини з вибірки. всередині повинна бути умова
    //////
    var doAllNumbersMeetCondition = numbers.All(n => n > 8) // Чи всі числа більше 8?
    //////
    IEnumerable<string> nameList = games.Select(s => s.Name); // створюємо окремо колекцію з вибраних даних. Select також може створити
    // абсолютно нову колекцію
    List<StarRating> stars = games.Select(s => new StarRating {
      Name = s.Name;
      Stars = s.Score * 0.5f;
    }).ToList();
    //////
    var firstWith10Rating = games.First(g => g.Score == 10); // BE CAREFUL! Throws exception if no item was found
    ///// better to use
    var firstWith9Rating = games.FirstOrDefault(g => g.Score == 9); // if not found - variable will be null
    ///// 3 random games with rating EG 9, released after 2018
    static Random _random = new Random();
    var g = games.Where(g => g.Score >= 9 && g.ReleaseDate.Year >= 2018)
                  .OrderBy(g => _random.Next())
                  .Take(3);
  }
  ////////
  var atLeastOneNineRate = games.Any(g => g.Score == 9); // хоча б одна гра з рейтингом 9
  /////
  
}
// how to create your own lambda function
public static class Helper
{
  public static IEnumerable<Game> AddRationgToName(this IEnumerable<Game> games) 
  {
      foreach(var game in games)
      {
        game.Name = $"{game.Name}  - {game.Score}";
      }
      return games;
  }
}
```

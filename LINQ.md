# Use of LINQ
## LINQ - Language-Integrated Query - являє собою мову запитів до джерела даних. Існує декілька видів LINQ, наприклад: 
- LINQ to Objects
- LINQ to Entities
- LINQ to XML
- LINQ to DataSet
- Parallel LINQ (PLINQ)

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
      new Game{Name = "Game1", ReleaseDate = new Date(2020,3,24), Score = 9 },
      new Game{Name = "Game2", ReleaseDate = new Date(2020,8,12), Score = 7 },
      new Game{Name = "Game3", ReleaseDate = new Date(2020,9,5), Score = 10 }
    }
    var isOver5 = numbers.Where(number => number > 5); // фільтр вибірки, для отримання підмножини з вибірки. всередині повинна бути умова
    var doAllNumbersMeetCondition = numbers.All(n => n > 8) // Чи всі числа більше 8?
    IEnumerable<string> nameList = games.Select(s => s.Name); // створюємо окремо колекцію з вибраних даних. Select також може створити
    // абсолютно нову колекцію
    List<StarRating> stars = games.Select(s => new StarRating {
      Name = s.Name;
      Stars = s.Score * 0.5f;
    }).ToList();
    
    // 
  }
}
```

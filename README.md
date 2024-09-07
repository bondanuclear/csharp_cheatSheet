# csharp_cheatSheet
``` csharp

public static void main(string[] args)
{
  Console.WriteLine("Hello, World!);
}
```
# In Ref Out modifiers
## In/ Ref/ Out - використовуються для передачі параметрів по ссилці. 
Наприклад 
``` csharp
int intValue = 5; - value type.
//щоб змінити його треба передати його по ссилці в метод 
ChangeValue(ref intValue) або ChangeValue(out intValue);

void ChangeValue(ref/out value)
{
  value++;
}
```
### Difference between out and ref
Різниця між out i ref в тому, що при модифікаторі out ми зобов'зані присвоїти змінній певне значення в методі. Це дозволяє передавати неініціалізовану змінну в метод.
E.G треба змінити розмір масиву: int[] array = new int[4] => new int[10]
``` csharp
void ChangeArraySize(ref int[] array)
{
  array = new int[10];
}
```
Але якщо ми хочемо змінити лише один з елементів масиву, передавати по ссилці не треба, оскільки масив - за замовчанням клас Array, який є reference типом.
### In modifier
Модифікатор In призначений для роботи зі значимими (value) типами, оскільки забороняє будь-які дії над змінними. Передаючи структуру по ссилці ми зменшуємо час, який витрачається на копіювання інформації з одного значимого типа в інший, оскільки структура - value type.
``` csharp
structure Alphabet
{
  public decimal a;
  public decimal b;
 ........
  public decimal x;
}
void Foo(in Alphabet alphabet) {}
void Boo(Alphabet alphabet) {} => в залежності від розмірів, швидкість роботи Foo буде більшою від Boo в n кількість разів.
``` 
 # 
# params Keyword
## Використовується для зручного передавання безліччі параметрів у функцію.
### For example
``` csharp
public int Sum(params int[] array)
{
  int sum = 0;
  for(int i = 0; i < array.Length; i++)
  {
    sum += array[i];
  }
  return sum;
}

public static void main(string[] args)
{
    int sum = Sum(1,2,3,4);
}
```
# Іменовані параметри
## Дозволяють ігнорувати порядок подання параметрів у функцію
### Example of using:
``` csharp
void int Sum(int a, int b, bool isLogging = false) // isLogging - так званий необов'язковий параметр, по дефолту завжди false.
{
  if(isLogging)
  {
    Console.WriteLine("a =" + a);
    Console.WriteLine("b =" + b);
  }
  return a + b;
}
public static void main(string[] args)
{
  Sum(b: 5, a :14);
}
```
# Перевірка на арифметичне переповнення:
## Наприклад, інтервал типу byte = [0,255]. а отже 
## byte a = 0; a - 1 = 255;
# Ключові слова checked/unchecked.
``` csharp
  public void TestChecked()
  {
    byte a = 0;
    a = checked((byte) (a - 1));
    або
    checked
    {
      /// .....
      /// викине exception, якщо трапляється арифметичне переповнення.
    }
  }
```
### Переповнення для типів double i float => це або ж Infinity(maxValue + maxValue) або NaN (0.0/0.0)
### Що стосується Decimal, то цей тип завжди викидає exception при переповненні, навіть при unchecked.

# Оператор об'єднання з null
### null - reference type.
``` csharp
 string str = null;
 Console.WriteLine(str ?? "empty"); => if(str == null) ConsoleWriteLine("Empty");
```
# Оператор умовного null

``` csharp
 Array myArray = null;
 myArray?.Sum(); => якщо myArray == null, то код не буде виконуватись.
```

# Оператор присвоєння null

``` csharp
 str ??= "empty"
 => if(str == null) str = "empty";
```
# Nullable 
### Використовується частіше за все для значимих типів, для змоги тримати в них null. Це робиться для зручного і правильного отримання і опрацювання даних, які, наприклад, надходять з SQL датабази. Оскільки прийти звідти може int, всередині якого міститься null.
``` csharp
 int? a = null;
```
# Properties
## Те саме, що і гетери сетери.
### get i set - accessors. => propfull - shortcut, prop - automatic properties => {get; set;}
``` csharp
  private int val;
  public int Val {
    get {  return val; }
    set {val = value; }
    // value - special variable
  }
```
# Static constructors
## Створюються один раз за повне існування програми. Перед всіма звичайними конструкторами та статичними методами

### Статичний конструктор - ідеальне місце ініціалізації статичних даних нашого класу, якщо на момент написання програми цих даних ще немає. І потрібно отримати ці дані в момент виконання. Наприклад, для підключення до бази даних нам треба певний стрінг. В більш комплексних проектах використовується Dependency Injection

``` csharp
class DBRepository
{
  private static string connectionString;
  static DBRepository() {
      ConfigurationManager confMan = new ConfigurationManager();
      connectionString = confMan.GetConnectionString();
  }
}
class ConfigurationManager
{
  public string GetConnectionString(){return "db local";} // отримання даних з певного конфіг файлу.
}
```
# Extension methods
## Необхідні для розширення функціоналу класа або структури, без безпосереднього влізання в них. Наприклад, структура DateTime:
``` csharp
  DateTime currentDateTime = DateTime.Now;
  // Для використання кастомного методу, який друкує дату, треба розширити цю структуру наступним чином
  static class DateTimeExtension
  {
    public static PrintFullData(this DateTime currentDateTime)
    {
      Console.WriteLine(currentDateTime);
    }
  }
  // Тепер ми можемо використати наше розширення
  currentDateTime.Print(); // => this DateTime не потрібно передавати в параметри, оскільки це синтаксис для розширення конкретного класу чи структури
  
```
### Також варто зазначити, що методи розширення варто виноситии в окремі namespace, щоб не заважати іншим розробникам.
# Partial classes and methods
## Partial classes використовуються для розбиття одного класа по декількох файлах. Наприклад, щоб винести певну логіку з поля зору. Це може бути певна автоматизована логіка, яку краще не чіпати, а отже, варто винести її з класу, в якому ми працюємо.
``` csharp  
partial class MyClass {}
```
### Логіка з методами така сама, ми хочем винести певний метод в інший файл того самого класа, але при цьому, щоб було видно, що такий метод існує
# CONST TA READONLY
## const є неявно статичним.
### модифікатор const робить значення незмінним, та потрібно для зручного використання певних незмінних даних в коді, таких як число PI.
## readonly поля - поля, які можна не ініціалізувати після об'явлення. тобто public readonly int a;
### можна об'явити статичним. Зазвичай використовуються як аналог до константних полів, лише з тією різницею, що якщо ми підтягуємо дані з бази даних/файлу, то в константне поле ці дані неможливо буде вписати. Для цього в конструкторі/статичному конструкторі ми readonly поля ініціалізуємо з потрібними даними. 
# Зручний спосіб ініціалізації об'єктів класу
``` csharp  
public class Address {
  public string city {get; set;}
  public string country {get; set;}
  
}
public class Person
{
  public int age {get; set;}
  public string name {get; set;}
  public Address address {get; set;}
}
public class Main(string[] args)
{
  Person person1 = new Person();
  person1.age = 20; person.name = "Harry"; 
  Address address1 = new Address(); 
  //... 
  // можна так, але якщо наявний власний тип даних можна це зробити зручніше
  Person person2 = new Person {
    age = 20,
    name = "Robert",
    Address = new Address {city = "New York", country = "USA", }  
  };
  
}
```
# base keyword
## Посилання на батьківський клас

``` csharp
public class Point2D 
{
  public int X {get; set;}
  public int Y {get; set;}
  public Point2D(int x, int y)
  {
    X = x;
    Y = y;
  }
  public void Print2D()
  {
    Console.WriteLine("x = " + X);
    Console.WriteLine("y = " + Y);
  }
}
public class Point3D : Point2D
{
  public int Z {get; set;}
  // якщо конструктор без параметрів, то конструктор батьківського класу використовується автоматично. раніше, ніж конструктор
  // успадкованого
  // якщо конструктор з параметрами, то використовуєм base 
  public Point3D(int x, int y, int z) : base(x,y)
  {
    Z = z;
    
  }
  // або 
  public void Print3D()
  {
    base.Print2D();
    Console.WriteLine("z = " + Z);
  }
}
```
 # AS IS operators
 ## as - використовується для явного приведення типу. is - для перевірки типу
 ``` csharp
 public void AsUse(object obj)
 {
    Point2D point = obj as Point2D;
    // if obj is not Point2D then it'll be null
    // else point will initialize as Point2D
    if(point != null)
    {
      point.Print2D(); 
    } 
 } 
 
 public void IsUse(object obj)
 {
    if(obj is Point2D)
    {
        point = (Point2D) obj;
        point.Print2D();
        // it is not comfortable     
    }
    // another way
    // if obj is point2d then initialize point right away
    if(obj is Point2D point)
    {
       point.Print2D();
    }
 } 
 ``` 
# Наслідування інтерфейсів
## Якщо у нас наприклад є гравець, який володіє вогнепальною зброєю і зберігає її в інвентарі, а нам треба додати холодну зброю, яку ще наприклад можна кидати, то вирішенням може бути наслідування інтерфейсів. І пістоль, і ніж наносять певну шкоду, ними можна атакувати, але ніж ще можна і кинути у ворога
``` csharp
interface IWeapon 
{
  int Damage {get;}
  void Fire();
}
interface IThrowingWeapon : IWeapon
{
  void Throw();
}
// клас вогнепальної зброї
class Pistol : IWeapon
{
  public int Damage => 5;
  public void Fire()
  {
    Console.WriteLine("Shooting pistol");
  }
}
// клас холодної зброї
class Knife : IThrowingWeapon
{
  public int Damage => 10;
  public void Fire() { Console.WriteLine("Swinging the knife"); }
  public void Throw() {Console.WriteLine("Throwing the knife");}
}
class Player
{
  public void ShowWeapon(IWeapon weapon)
  {
      Console.WriteLine($"Weapon : {weapon.GetType().Name}  Deals Damage {weapon.Damage}");
  }
}
public static void main(string[] args)
{
  Player player = new Player();
  IWeapon weaponsInventory = {new Knife(), new Pistol()};
  foreach(var item in weaponsInventory)
  {
    player.ShowWeapon(item);
    // зараз ми можемо працювати і з холодною зброєю, і з вогнепальною без зміни коду
  }
}
```
# Явна реалізація інтерфейсів
## Якщо 2 чи більше інтерфейсів описують методи з однаковими сигнатурами, то щоб кожен з них можна було реалізувати окремо ми явно реалізуємо їх
``` csharp 
  interface IFirst {void Action();}
  interface ISecond { void Action();}
  void F1(IFirst iFirst) { Console.WriteLine("first interface");}
  void F2(ISecond iSecond) { Console.WriteLine("Second interface");}
  class TestClass : IFirst, ISecond 
  {
    // public void Action() {Console.WriteLine("Action is happening");} // такий код означає, що реалізація однакова для обох інтерфейсів
    public void IFirst.Action() {}
    public void ISecond.Action() {}
  }
public static void main(string[] args)
{
  TestClass test = new TestClass();
  F1(test);
  F2(test);
  IFirst firstAction = test as IFirst; // 
  if(test is ISecond secondAction) 
  {
      secondAction.Action();
  }
}
```
# IEnumerable and IEnumerator interfaces
## IEnumerable says that our class can be enumerated, IEnumerator says how to do it.
### If we have 1 million item, it may be better to take 1 number at the time using IEnumerable rather than allocating items into memory with ToList()
### Either way, the best way to choose between IEnumerables and ToList is to use a benchmark.
### if we have some function that returns a collection, it's better to return IEnumerable and use yield return, if possible.
```csharp
// for example. we have 1 million items. If we want to use only 5 of them,
//  it would be wise to pull one number at the time to make performance
// better
IEnumerator<int> GetNumbers()
{
  for(int i = 0; i < 1000000; i++) {
    Console.WriteLine($"Processed {i}");  
    yield return i;
  }
}

foreach(var number in GetNumbers())
{
  Console.WriteLine(number);
  if(number > 5) break;
}

```
## If we want to be able to iterate over our class, we have to implement IEnumerable iterface with GetEnumerator() method
```csharp
public class Example : IEnumerable
{
  List<string> names = new List<string>();
  public IEnumerator GetEnumerator()
  {
    names.GetEnumerator();
    // return new Test2(names);
  }
}
```
## If we want to create our custom IEnumerator, we have to implement IEnumerator interface, with methods MoveNext(), Reset(), property Current; Example and Test2 works together
```csharp
// for example
public class Test2 : IEnumerator
{
  List<string> myList = new List<string>();
  int currIndex = -1; // -1 because foreach loop automaticaly makes currIndex++
  public Test2( List<string> myList)
  {
    this.myList = myList;
  }
  public void Reset()
  {
    currIndex = -1;
  }
  public bool MoveNext()
  {
    currIndex += 1;
    return currIndex < myList.Count;
  }
  public object Current => myList[currIndex];
}
```
## If we want to use LINQ together with our IEnumerable, we have to make both IEnumerable and IEnumerator generic. By the way, there will be some additional methods to implement, if we use generics.
### For example IEnumerable.GetEnumerator() and IEnumerable<> GetEnumerator(). As for IEnumerator, we have to implement Dispose() method (though it's not necessary, which might violate interface segregation principle ?? )
  
```csharp
public class Example : IEnumerable<string>
public class Test2 : IEnumerator<string>
```
## Problems with statics
### Static methods or classes, especially Singleton pattern, lead to the implicit dependencies, which make our life harder (at code reading process)
#### The best way to organize a class is to put all the dependencies at constructor, so that reader can see all explicit dependencies. </br> All the static methods (registration methods) should be controlled by dependency injection container and put in the same place to not create a lot of confusion. By using DI containers we can explicitly see the dependencies each class has. 
### Problems with testing
#### Since static methods lead to implicit dependencies, it becomes really hard to limit the frames of the testing.


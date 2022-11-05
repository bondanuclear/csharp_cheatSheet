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

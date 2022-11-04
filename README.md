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

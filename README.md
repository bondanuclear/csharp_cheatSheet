# csharp_cheatSheet
# In Ref Out modifiers
## In/ Ref/ Out - використовуються для передачі параметрів по ссилці. 
Наприклад 
int intValue = 5; - value type.
щоб змінити його треба передати його по ссилці в метод 
ChangeValue(ref/out intValue);

void ChangeValue(ref/out value)
{
  value++;
}
### Difference between out and ref
Різниця між out i ref в тому, що при модифікаторі out ми зобов'зані присвоїти змінній певне значення в методі. Це дозволяє передавати неініціалізовану змінну в метод.
E.G треба змінити розмір масиву: int[] array = new int[4] => new int[10]
void ChangeArraySize(ref int[] array)
{
  array = new int[10];
}
Але якщо ми хочемо змінити лише один з елементів масиву, передавати по ссилці не треба, оскільки масив - за замовчанням клас Array, який є reference типом.
### In modifier
Модифікатор In призначений для роботи зі значимими (value) типами, оскільки забороняє будь-які дії над змінними. Передаючи структуру по ссилці ми зменшуємо час, який витрачається на копіювання інформації з одного значимого типа в інший, оскільки структура - value type.
structure Alphabet
{
  public decimal a;
  public decimal b;
 ........
  public decimal x;
}
void Foo(in Alphabet alphabet) {}
void Boo(Alphabet alphabet) {} => в залежності від розмірів, швидкість роботи Foo буде більшою від Boo в n кількість разів.
# 
'''
public static void main()
{
  Console.WriteLine("Hello, World!);
}
'''

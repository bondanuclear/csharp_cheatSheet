# csharp_cheatSheet
# In Ref Out modifiers
## In/ Ref/ Out - використовуються для передачі параметрів по ссилці. 
наприклад 
int intValue = 5; - value type.
щоб змінити його треба передати його по ссилці в метод 
ChangeValue(ref/out intValue);

void ChangeValue(ref/out value)
{
  value++;
}
Різниця між out i ref в тому, що при модифікаторі out ми зобов'зані присвоїти змінній певне значення в методі. Це дозволяє передавати неініціалізовану змінну в метод.


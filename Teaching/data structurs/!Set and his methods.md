---
дата создания: 2024-05-04
Программирование:
---
<hr>
<h5>Внутренние ссылки по Теме :</h5>
1. [[]]
2. [[]]

Множества (sets) — это коллекции, которые содержат уникальные элементы и не поддерживают порядок. В Python множества могут быть полезны для удаления дубликатов из последовательности и для выполнения математических операций, таких как объединение, пересечение, разность и симметричная разность.
Множества в Python особенно полезны, когда вам нужно обеспечить уникальность элементов и выполнить операции, связанные с теорией множеств.

Вот некоторые основные операции и методы, связанные с множествами в Python:
- Создание множества: `my_set = {1, 2, 3}` или `my_set = set([1, 2, 3])`
- Добавление элемента: `my_set.add(4)`
- Удаление элемента: `my_set.remove(4)` (вызывает KeyError, если элемента нет) или `my_set.discard(4)` (не вызывает ошибку)
- Очистка множества: `my_set.clear()`
- Проверка наличия элемента: `if 1 in my_set:`
- Операции над множествами:
    - Объединение: `my_set | another_set` или `my_set.union(another_set)`
    - Пересечение: `my_set & another_set` или `my_set.intersection(another_set)`
    - Разность: `my_set - another_set` или `my_set.difference(another_set)`
    - Симметричная разность: `my_set ^ another_set` или `my_set.symmetric_difference(another_set)`


###пример с кодом:
```python
# Создание множеств
fruits = {'apple', 'banana', 'cherry'}
more_fruits = {'pineapple', 'mango', 'banana'}

# Добавление элемента
fruits.add('orange')

# Удаление элемента
fruits.discard('banana')

# Операции над множествами
union_set = fruits | more_fruits  # Объединение множеств
intersection_set = fruits & more_fruits  # Пересечение множеств
difference_set = fruits - more_fruits  # Разность множеств
symmetric_difference_set = fruits ^ more_fruits  # Симметричная разность множеств

# Вывод результатов
print('Объединение:', union_set)
print('Пересечение:', intersection_set)
print('Разность:', difference_set)
print('Симметричная разность:', symmetric_difference_set)

```






Метод **`filter()`** в Django QuerySet используется для **отбора объектов из базы данных, соответствующих заданным условиям**.  
Он **возвращает новый QuerySet**, содержащий только те записи, которые удовлетворяют фильтру.

### Основной синтаксис

Фильтрация выполняется с помощью **именованных аргументов (keyword arguments)**, где имя аргумента строится по схеме:

```python
Model.objects.filter(<поле>__<условие>=<значение>)
```
>[!attention] Важно! поле соединятся при помощи двух  нижних подчеркиваний

>[!quote] Примеры
>```Book.objects.filter(title__icontains='война')```
>Название содержит “война” (без учёта регистра). 
>```Book.objects.filter(author__in=['Толстой', 'Достоевский'])```
>Книги, написанные Толстым или Достоевским.
>```Book.objects.filter(pages__range=(100, 300))```
>Книги, где количество страниц от 100 до 300

# Таблица частых выражений (lookup expressions)

| Выражение              | Значение / Описание                  | Пример                                                     |
| ---------------------- | ------------------------------------ | ---------------------------------------------------------- |
| `exact`                | Точное совпадение                    | `filter(price__exact=100)`                                 |
| `iexact`               | Точное совпадение без учёта регистра | `filter(author__iexact='толстой')`                         |
| `contains`             | Содержит подстроку                   | `filter(title__contains='война')`                          |
| `icontains`            | Содержит подстроку (без регистра)    | `filter(title__icontains='война')`                         |
| `gt`                   | Больше чем                           | `filter(price__gt=500)`                                    |
| `gte`                  | Больше или равно                     | `filter(price__gte=500)`                                   |
| `lt`                   | Меньше чем                           | `filter(price__lt=500)`                                    |
| `lte`                  | Меньше или равно                     | `filter(price__lte=500)`                                   |
| `in`                   | Находится в списке                   | `filter(author__in=['Толстой', 'Достоевский'])`            |
| `range`                | Входит в диапазон                    | `filter(year__range=(1900, 1950))`                         |
| `startswith`           | Начинается с                         | `filter(title__startswith='Война')`                        |
| `istartswith`          | Начинается с (без регистра)          | `filter(title__istartswith='война')`                       |
| `endswith`             | Заканчивается на                     | `filter(title__endswith='мир')`                            |
| `iendswith`            | Заканчивается на (без регистра)      | `filter(title__iendswith='мир')`                           |
| `isnull`               | Проверка на `NULL`                   | `filter(description__isnull=True)`                         |
| `year`, `month`, `day` | Фильтрация по дате                   | `filter(published_date__year=2024)`                        |
| `date`                 | Точная дата                          | `filter(published_date__date=datetime.date(2024, 10, 28))` |

# Объект Q

Если нужно использовать **OR** или **NOT**, применяются `Q`-объекты:

```python
from django.db.models import Q

# Книги Толстого ИЛИ книги дороже 1000
Book.objects.filter(Q(author='Толстой') | Q(price__gt=1000))

# Книги, не написанные Толстым
Book.objects.filter(~Q(author='Толстой'))
```
> то есть `Q`-объекты нужны, когда требуется **логика, отличная от простого AND**

### Цепочки

при это можно писать и так, что будет эквивалентно **AND**
```python
Book.objects.filter(author='Толстой', price__lte=1000) 
# или 
Book.objects.filter(author='Толстой').filter(price__lte=1000)
```

>[!TIP] Фишка
>Порой бывают ситуации, где **цепочки действительно полезны**.
>1. Пошаговое построение запроса - когда фильтры зависят от условий
>2. Повторное использование QuerySet - можно определять базовый QuerySet и накладывать разные фильтры
>3. Отложенное выполнение - где `filter()` не выполняет SQL сразу — Django просто **дополняет объект запроса**, пока ты не обратишься к результату. Это безопасно и не "бьёт" по производительности.

1. Пошаговое построение запроса 
```python
qs = Book.objects.all()

if author:
    qs = qs.filter(author=author)
if min_price:
    qs = qs.filter(price__gte=min_price)
if max_price:
    qs = qs.filter(price__lte=max_price)
```

2. Повторное использование QuerySet
```python
base_qs = Book.objects.filter(is_published=True)
cheap_books = base_qs.filter(price__lt=500)
expensive_books = base_qs.filter(price__gte=500)
```

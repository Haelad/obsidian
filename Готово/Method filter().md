# Method filter()

>[!info] Метод filter() 
>используется для **отбора объектов из базы данных, соответствующих заданным условиям**. 
>Он **не изменяет исходный QuerySet**, а **возвращает новый QuerySet**, содержащий только те записи, которые удовлетворяют фильтру.


- ###### Синтаксис
	- `Model.objects.filter(<поле>__<lookup>=<значение>)`
- ###### Значения:
	- `<поле>` — имя поля модели.
	- `<lookup>` — способ сравнения или поиска (например, `exact`, `contains`, `gte` и т.д.).
	- `<значение>` — то, с чем производится сравнение.

>[!note] Заметка
>`filter()` — это декларативное описание запроса, а не его немедленное выполнение.  
>Запрос к базе будет выполнен **только при обращении к данным** (например, при итерации, `list()`, `len()` и т.п.).

>[!attention]  Обрати внимание
>Части выражения соединяются **двойным подчёркиванием**:  `поле__lookup`
>Если `lookup` не указан, Django использует `exact` по умолчанию (точное совпадение).

# Таблица частых выражений (lookup expressions)
| Выражение              | Значение / Описание                  | Пример                                                     |
| ---------------------- | ------------------------------------ | ---------------------------------------------------------- |
| `exact`                | Точное совпадение                    | `filter(price__exact=100)`                                 |
| `iexact`               | Точное совпадение без учёта регистра | `filter(author__iexact='толстой')`                         |
| `contains`             | Содержит подстроку                   | `filter(title__contains='война')`                          |
| `icontains`            | Содержит подстроку (без регистра)    | `filter(title__icontains='война')`                         |
| `regex`                | Поиск по регулярному выражению       | `filter(title__regex=r'^Война.*')`                         |
| `iregex`               | Поиск по regex без учёта регистра    | `filter(title__iregex=r'^война.*')`                        |
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

> [!tip] `regex` и `iregex`
> Используется, когда невозможно описать фильтрацию стандартными lookup-полями.


>[!example] Пример
>Эти запросы возвращают QuerySet — не список, а "описание запроса".  
>Django выполнит SQL **только при реальном обращении** к данным:
>```python
>for book in Book.objects.filter(price__lte=500):
>  print(book.title)
>```


>[!important] # Особенности работы 
>1. **`filter()` не делает SQL-запрос сразу.**  
>> Каждый вызов создаёт новый QuerySet, хранящий параметры фильтрации.
>2. **Все фильтры объединяются логически через AND**, если не указано иное.
>```python
>Book.objects.filter(author='Толстой', price__lte=1000)
>```
> 3. **Цепочки фильтров** работают на уровне ORM, не создавая множество SQL-запросов:
> ```python
> Book.objects.filter(author='Толстой').filter(price__lte=1000) # эквивалентно одному запросу с AND.
> ```
>4. **Выполнение запроса** происходит только в момент обращения:
>```python
>list(qs)   # выполнение SQL
>len(qs)    # выполнение COUNT(*)
>for obj in qs:  # итерация — выполнение запроса
>```


# Сложные логические построения с методом filter

> [!important] #  **Q-объекты**.
> Q-объекты нужны для создания более гибких условий фильтрации:
> - Условия динамически формируются (например, зависят от пользовательского ввода).
> - Требуется объединить фильтры **по OR**, что нельзя сделать обычной цепочкой `filter()`.
> - Нужно построить **отрицательное** условие (`NOT`).

- ###### Синтаксис
	- `Model.objects.filter(Q(<условие_1>) Логический оператор Q(<условие_2>))`
	- ###### Логические операторы
		- `&` —  **И**
		- `|` —  **ИЛИ**
		- `~` —  **НЕ**

>[!example] Примеры:
>>Книги Толстого ИЛИ книги дороже 1000
>```python
>from django.db.models import Q
>Book.objects.filter(Q(author='Толстой') | Q(price__gt=1000))
>```
>>Книги, не написанные Толстым
>```python
>from django.db.models import Q
>Book.objects.filter(~Q(author='Толстой'))
>```
>>Книги Толстого, но не опубликованные
>```python
>from django.db.models import Q
>Book.objects.filter(Q(author='Толстой') & Q(is_published=False))
>```

### Комбинации с другими методами ORM

> [!important] # Взаимодействие `filter()` с другими методами QuerySet  
> `filter()` — часть общей ORM-цепочки.  
> Он может использоваться совместно с другими методами, и все они работают **лениво** (SQL формируется только при обращении к данным):
> ```python
> Book.objects.filter(is_published=True)
>            .exclude(price__lt=300)
>            .annotate(num_pages_group=F('pages'))
>            .order_by('-published_date')
> ```
> - **`exclude()`** — противоположен `filter()`, исключает записи.  
>- **`annotate()` / `aggregate()`** — позволяют добавлять вычисляемые поля.  
>- **`order_by()` / `distinct()`** — управляют сортировкой и уникальностью.
>- **`select_related()` / `prefetch_related()`** — оптимизируют загрузку связанных данных.
>Все они **не выполняют SQL немедленно**, а **дополняют QuerySet**, что делает ORM гибкой и выразительной.


# SQL, который стоит за фильтрацией

> [!tip] Как Django строит SQL-запросы при фильтрации  
> ORM автоматически конструирует SQL в зависимости от условий в `filter()` или `Q()`.

>[!example] Пример 
>простой filter() вместе с логическим оператором and
>```python
> Book.objects.filter(author='Толстой', price__lte=500)
>```
>SQL:
>```SQL
>SELECT *
>FROM "app_book"
>WHERE "app_book"."author" = 'Толстой'
>AND "app_book"."price" <= 500;
>```
>использование Q-объекта с OR
>```python
>Book.objects.filter(Q(author='Толстой') | Q(price__gt=1000))
>```
>SQL:
>```SQL
>SELECT *
>FROM "app_book"
>WHERE ("app_book"."author" = 'Толстой'
>OR "app_book"."price" > 1000);
>```
>отрицание условия (NOT)
>```python
>Book.objects.filter(~Q(author='Толстой'))
>```
>SQL:
>```SQL
>SELECT *
>FROM "app_book"
>WHERE NOT ("app_book"."author" = 'Толстой');
>```

>[!example] пример ORM-запроса
>```python
>from django.db.models import Q, F, Count
>qs = (
>    Book.objects
>    .filter(Q(author='Толстой') | Q(price__gte=500))  # фильтрация OR
>   .exclude(is_published=False)                      # исключаем неопубликованные
>    .annotate(num_reviews=Count('reviews'))           # добавляем вычисляемое поле>
>    .filter(num_reviews__gte=10)                      # повторная фильтрация по аннотации
>    .order_by('-published_date')                      # сортировка по дате
>)
>```
>Сгенерированный SQL (упрощённый вид)
>```SQL
>SELECT "books"."id",
>      "books"."title",
>     "books"."author",
>       "books"."price",
>       "books"."published_date",
>       COUNT("reviews"."id") AS "num_reviews"
>FROM "books"
>LEFT OUTER JOIN "reviews" ON ("books"."id" = "reviews"."book_id")
>WHERE (
>  "books"."author" = 'Толстой'
>    OR "books"."price" >= 500
>)
>AND NOT ("books"."is_published" = false)
>GROUP BY "books"."id"
>HAVING COUNT("reviews"."id") >= 10
>ORDER BY "books"."published_date" DESC;
>```

>[!tip] Таким образом  
>Комбинация `filter()` + `exclude()` + `annotate()` + `order_by()`  
>превращается в **единый SQL-запрос**, а не в серию отдельных операций.  
>Это делает Django ORM не просто синтаксическим сахаром, а **оптимизированным генератором SQL**.


>[!tldr]  Вывод
>Метод filter() является основным мощным инструментом построения SQL запроса в форме декларативного ORM описания запроса, позволяющего комбинированить другие методы для построения сложных и гибких выборок данных.
>Понимание принципов работы `filter()` и того, **как он превращается в SQL**, помогает писать более эффективный и читаемый код, избегая избыточных обращений к базе данных.


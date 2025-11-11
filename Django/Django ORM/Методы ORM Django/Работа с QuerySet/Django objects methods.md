### методы firts и last

Для того что бы выбрать первую или последнюю запись в таблице можно воспользоваться методом first и last.
Они отбирают первую и последнюю запись по выборке.

 Пример:
```python
 MyModel.objects.first()
```
>здесь мы выбрали нашу первую запись

```python
 MyModel.objects.last()
```
>а здесь выбрали последнюю...

### методы earlist и latest
Выбирает записи по времени: самые ранние - earlist, самые поздние - latest.

```python
MyModel.objects.earlist('time_update')
```
> тут мы выбираем запись по самым ранним датам

```python
MyModel.objects.latest('time_update')
```
> а тут по самым поздним...

>[!NOTE] Замечание
>>в поле метода мы должны выбрать к какой записи будет применятся метод earlist()
>к примеру если у нас 2 записи со определенным времен: time_update и time_create


## методы get_previous_by и get_next_by

### get_previous_by

Для того чтобы получить предыдущею запись относительно выбранной используется метод `get_previous_by`.

#### Синтаксис :
get_previous_by_`запись` из таблицы относительно которой мы хотим получить предыдущею.
```python
model = MyModel.objects.all()
model.get_previous_by_time_update()
```

### get_next_by

Для того чтобы получить следующую запись относительно выбранной используется метод
`get_next_by`.

#### Синтаксис :
get_next_by_`запись` из таблицы относительно которой мы хотим получить следующую.
```python
model = MyModel.objects.all()
model.get_previous_by_time_update()
```

>[!NOTE] примечание
>В круглых скобках  можно прописывать условия выборки записей, но такой
>что бы в него попадала следующая или предыдущая запись
>

## методы exists и count

### метод exists 
используется для проверки существования хотя бы одной записи

>[!WARNING] Важно знать
для того чтобы узнать существуют ли хотя бы одна запись в модели:
`название модели_set.all().exists`
или если установлено имя  менеджера через параметр (`related_name`)
`указанное имя.exists`

```python
m = MyModel.objects
m.my_model_set.all().exists()
```
>если хотя бы одна запись присутствует - выведет True, а если нет, то Flase.
>posts является менеджером

>[!NOTE] примечание
>именно по QuerySet определяется значение True или False
>Если <QuerySet []> =  False. Либо <QuerySet [данные]> = True

### метод count 
используется для подсчета существующих записей

работает так же как и по аналогии с exists
можно использовать с filter

```python
m = MyModel.objects
m.posts.count()
MyModel.objects.filter(id=1).count()
```



## Class F 
позволяет сравнивать поля между друг другом
Если требуются математические операции с записями


Класс `F` в Django (`django.db.models.F`) используется для референции полей модели в запросах к базе данных. Это позволяет выполнять операции и обновления с использованием значений других полей в тех же строках, не загружая значения этих полей в память.  
  
### Основные возможности класса `F`:  
1. **Арифметические операции:**  
С помощью `F` можно выполнять арифметические операции над полями напрямую в базе данных.  

```python
from django.db.models import F 
from myapp.models import MyModel   
# Увеличение значения field1 на значение field2   
MyModel.objects.update(field1=F('field1') + F('field2'))   

```

  
  
2. **Обновление на основе текущего значения поля:**  
Можно обновить поле модели на основе его же текущего значения.  

```python   
# Увеличение значения field1 на 1   
MyModel.objects.update(field1=F('field1') + 1)   
```

  
  
3. **Фильтрация с использованием значений других полей:**  
Можно фильтровать записи, сравнивая значения разных полей.  

```python   
# Получение всех объектов, где значение field1 больше значения field2   
results = MyModel.objects.filter(field1__gt=F('field2'))   
```

  
  
4. **Использование F для аннотаций:**  
Можно использовать `F` в аннотациях для создания дополнительных вычисляемых полей.  

```python 
from django.db.models import F, ExpressionWrapper, DecimalField   
# Добавление аннотации с вычисляемым полем 
results = MyModel.objects.annotate(
total=ExpressionWrapper(F('price') * F('quantity'), output_field=DecimalField()))   
```

  
  
### Преимущества `F`:  
- **Эффективность:** Позволяет базе данных выполнять операции на уровне SQL, что может быть быстрее, чем выполнение операций на уровне Python.  
- **Атомарность:** Гарантирует, что операции будут атомарными (изменения будут происходить одновременно и последовательно), что важно для корректности данных в условиях конкурентного доступа.  
- **Гибкость:** Позволяет создавать сложные запросы, комбинируя значения полей, без необходимости загружать части данных в память.



## метод annotate()

позволяет создавать поля принимающие вычислительную операцию 

Метод `annotate()` в Django используется для добавления аннотаций к QuerySet'у. Аннотации представляют дополнительные вычисляемые поля, которые включаются в каждый объект QuerySet'а. Эти поля могут быть основаны на агрегированных данных, арифметических операциях, условных выражениях и т.д.  
  
### Основные возможности `annotate()`:  
  
1. **Агрегация данных:**  
С помощью аннотаций можно добавить к объектам агрегированные данные. Например, количество связанных объектов или сумма значений.  
  

```python 
from django.db.models import Count   from myapp.models import Author, Book   # Аннотация количества книг для каждого автора   
authors = Author.objects.annotate(num_books=Count('book'))   
```

  
  
2. **Арифметические операции:**  
Можно добавить вычисляемые поля на основе арифметических операций.  
  

```python  
from django.db.models import F, ExpressionWrapper, DecimalField   
# Аннотация общей стоимости товаров (цена * количество)   
products = Product.objects.annotate(total_value=F('price') * F('quantity'))   
# Если требуется указать тип данных для результата   
products = Product.objects.annotate(
total_value=ExpressionWrapper(F('price') * F('quantity'), output_field=DecimalField()))   
```

  
  
3. **Вычисляемые поля на основе условных выражений:**  
Можно использовать условные выражения для вычисляемых полей.  
  

```python   
from django.db.models import Case, When, IntegerField   
# Аннотация поля, которое зависит от значения другого поля   
products = Product.objects.annotate(discount=Case(When(category='Electronics',then=10), default=0,output_field=IntegerField(),))   
```

  
  
4. **Использование других агрегатных функций:**  
Помимо Count, можно использовать и другие агрегатные функции, такие как Sum, Avg, Max, Min.  
  

```python
from django.db.models import Sum  
# Аннотация общего веса товаров   
products = Product.objects.annotate(total_weight=Sum('weight'))   
```

  
  
### Пример использования `annotate()`:  
  
Предположим, у нас есть две модели: `Author` и `Book`. Каждая книга имеет автора:  
  

```python

models.pyfrom django.db import modelsclass Author(models.Model):   
name = models.CharField(max_length=100)class Book(models.Model):   
title = models.CharField(max_length=200)    
author = models.ForeignKey(Author, on_delete=models.CASCADE)    
price = models.DecimalField(max_digits=10, decimal_places=2)    
sales = models.IntegerField()
```

  
  
### Пример аннотаций:  
  
1. **Количество книг для каждого автора:**  
  

```python  
from django.db.models import Count  
authors = Author.objects.annotate(num_books=Count('book'))   
for author in authors:       
	print(f'{author.name} has written {author.num_books} books.')   
```

  
  
2. **Общая стоимость продаж для каждого автора:**  
  

```python  
from django.db.models import Sum, F, ExpressionWrapper, DecimalField  
authors = Author.objects.annotate(
total_sales=Sum(ExpressionWrapper(
F('book__price') * F('book__sales'),output_field=DecimalField())))   
for author in authors:      
	print(f'{author.name} has total sales worth {author.total_sales}.')   
```

  
  
### Преимущества использования `annotate()`:  
- Позволяет выполнять сложные вычисления и получения агрегированной информации на уровне базы данных, что может быть значительно быстрее и эффективнее, чем выполнение этих операций на уровне Python.  
- Обеспечивает высокую гибкость и выразительность при построении запросов.  
- Результаты аннотаций доступны в результате QuerySet'a как обычные поля модели, что упрощает доступ и обработку данных.  
  
Таким образом, `annotate()` является мощным инструментом Django для расширения возможностей запросов и получения дополнительной информации из базы данных.


## Метод values

values() используется для отбора конкретных записей
Пример
```python
MyModel.objects.values('Название записи', )
```
>выведет все записи с этим именем 


Для того что бы выбрать конкретную запись можно воспользоваться методом get
```python
MyModel.objects.values('Название записи', ).get(pk=1)
```
>выведет только одну запись


> [!NOTE] Примечание
> Выбирать записи можно и с другой таблицы, но только, если эта Таблица связана с ней

метод values менее ресурсо емкий

### Группировка записей

для того что бы сгруппировать записи нужно к методу values применить агрегирующею функцию

Пример
```python
MyModel.objects.values('Название записи', ).annotate(total=Count('id'))
```

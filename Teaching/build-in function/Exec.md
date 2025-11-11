---
дата создания: 2024-05-24
Язык: Python
tags:
  - Программирование
  - Python
  - Function
Функции: Встроенные
---
<hr>
<h5>Внутренние ссылки по Теме :</h5>
1. [[#exec()]]
2. [[#Параметры]] exec()

# exec:

*следует знать что:*
	1. **это функция, которая выражает код из типа данных - String** 
	2. Если это строка, то строка анализируется как набор операторов Python, который затем выполняется (если не возникает синтаксическая ошибка).
	3. Если это объект кода, он просто выполняется.
#### Синтаксис:

`exec(object, globals=None, locals=None, closure=None)`

#### Параметры:

- `object` - [строка кода](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-str-tekstovye-stroki/ "Текстовые строки str в Python."), либо объект кода,
- `globals` - [словарь](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-dict-slovar/ "Словарь dict в Python.") глобального пространства, относительно которого следует исполнить код,
- `locals` - объект-отображение (например [`dict`](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-dict-slovar/ "Словарь dict в Python.")), локальное пространство, в котором следует исполнить код.
- `closure=None` - определяет замыкание - кортеж переменных (добавлен в Python 3.11).


### пример с кодом:
```python
x = 'name = "John"\nprint(name)'
exec(x)
# John

y = 'print("5 + 10 =", (5+10))'
exec(y)
# 5 + 10 = 15

 prog = 'for x in range(9):\n    res = x*x\n    print(res)'
 exec(prog)
```

```python
user_input = input('Напишите ваш код: ')
exec(user_input)
# выполнит код написанный пользователем
```



---
aliases:
  - "[[cbv]]"
дата создания: 2024-06-19
tags:
---


# Распределение тем - название

1. что это такое (краткое описание)
2. для чего нужна (где и как применятся)
3. как работает (краткая техническая часть)
4. Концепция 
5. 
6. 
7. 
8. 
9. 
10. 

При обращении к какому-нибудь URL Django-проекта запрос передаётся в _urls.py_, и там специальный обработчик `path()` вызывает **view-объект**, передавая ему в качестве аргумента объект типа `request`, а на выходе ожидает объект типа `response`.

- **View-функция**, «функции представления» (от англ. _view_, «отображение, представление») или просто «представление». Это самый простой тип view-объектов. Функция получает на вход стандартный объект `request` и возвращает объект типа `response`.Объекты `response` могут быть созданы встроенными функциями-помощниками, например, [функцией `render()`](https://docs.djangoproject.com/en/2.2/topics/http/shortcuts/#render). Вызов view-функции: `path('any_url/', my_view)`
- **View-классы** или **class-based view** («представление, основанное на классе», «представление-класс»), как и view-функции, обрабатывают запрос и возвращают объект типа `response`. Такие классы должны наследоваться от специальных встроенных классов **Generic Views***.*  
    В `path()` view-классы вызываются через метод `as_view()`:`path('any_url/', ClassName.as_view())`

## Usage in your URLconf[¶](https://docs.djangoproject.com/en/5.0/topics/class-based-views/#usage-in-your-urlconf "Permalink to this headline")

The most direct way to use generic views is to create them directly in your URLconf. If you’re only changing a few attributes on a class-based view, you can pass them into the [`as_view()`](https://docs.djangoproject.com/en/5.0/ref/class-based-views/base/#django.views.generic.base.View.as_view "django.views.generic.base.View.as_view") method call itself:

```python
from django.urls import path
from django.views.generic import TemplateView

urlpatterns = [
    path("about/", TemplateView.as_view(template_name="about.html")),
]
```


## `TemplateView`[¶](https://docs.djangoproject.com/en/5.0/ref/class-based-views/base/#templateview "Permalink to this headline")

_class_ `django.views.generic.base.``TemplateView`[¶](https://docs.djangoproject.com/en/5.0/ref/class-based-views/base/#django.views.generic.base.TemplateView "Permalink to this definition")

Renders a given template, with the context containing parameters captured in the URL.

**Context**

- Populated (through [`ContextMixin`](https://docs.djangoproject.com/en/5.0/ref/class-based-views/mixins-simple/#django.views.generic.base.ContextMixin "django.views.generic.base.ContextMixin")) with the keyword arguments captured from the URL pattern that served the view.
- You can also add context using the [`extra_context`](https://docs.djangoproject.com/en/5.0/ref/class-based-views/mixins-simple/#django.views.generic.base.ContextMixin.extra_context "django.views.generic.base.ContextMixin.extra_context") keyword argument for [`as_view()`](https://docs.djangoproject.com/en/5.0/ref/class-based-views/base/#django.views.generic.base.View.as_view "django.views.generic.base.View.as_view").
## Title 
основная тема


### Title 
под тема


## Title 
основная тема


### Title 
под тема













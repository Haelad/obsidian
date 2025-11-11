---
дата создания: 2024-12-17
tags:
  - Django
  - Python
  - Программирование
---


### Redis[¶](https://docs.djangoproject.com/en/5.1/topics/cache/#redis "Link to this heading")

[[Redis]] -  это база данных NoSQL которая хранит данные in-memory хранилище данных

После установки Redis сервера, для того что бы работать с Redis в Python вам потребуется установить библиотеку  [redis-py](https://pypi.org/project/redis/) В Django оно по умолчанию установлено. Рекомендуется установить  [hiredis-py](https://pypi.org/project/hiredis/) .

Для того что бы воспользоваться редисом в Django:
- Настройте [`BACKEND`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHES-BACKEND) для `django.core.cache.backends.redis.RedisCache`.
- Настройте [`LOCATION`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHES-LOCATION) к URL  где находится ваш Redis сервер.


К  примеру, если редис запущен на локальном хосте: 
```python
CACHES = {
    "default": {
        "BACKEND": "django.core.cache.backends.redis.RedisCache",
        "LOCATION": "redis://127.0.0.1:6379",
    }
}
```

Часто Redis сервера защищены с аутентификацией. 
Для того что бы связать их с Django нужно указать имя и пароль в `LOCATION` url 
```python
CACHES = {
    "default": {
        "BACKEND": "django.core.cache.backends.redis.RedisCache",
        "LOCATION": "redis://username:password@127.0.0.1:6379",
    }
}
```

если у вас множество Redis серверов запущенных в репликационном моде, вы указать серверы как строку, разделенную точкой с запятой или запятой, или как список. 
```python
CACHES = {
    "default": {
        "BACKEND": "django.core.cache.backends.redis.RedisCache",
        "LOCATION": [
            "redis://127.0.0.1:6379",  # leader
            "redis://127.0.0.1:6378",  # read-replica 1
            "redis://127.0.0.1:6377",  # read-replica 2
        ],
    }
}
```
>При использовании нескольких серверов операции записи выполняются на первом сервере (лидере). Операции чтения выполняются на других серверах (репликах), выбранных случайным образом::
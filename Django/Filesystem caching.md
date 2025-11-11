---
дата создания: 2024-12-18
tags:
---
****

>[!ERROR]
Лучше обходить этот кэш стороной. 
 >1. Он не безопасный, ведь если хакер сможет получить доступ к папке где хранится кэш, то он сможет получить из нее данные.
 >2. Если в папке кэша будет слишком много это ухудшит производительность 



Сериализует и хранит каждое значение кэша как отдельный файл 

Что воспользоваться им:

```python
CACHES = {
    "default": {
        "BACKEND": "django.core.cache.backends.filebased.FileBasedCache",
        "LOCATION": "/var/tmp/django_cache",
    }
}
```
>Мы прописываем backend к url где находится файл-базе
>А в LOCATION мы указываем путь, где будут хранится данные 

>[!note] Замечанье
>Если вы пользуетесь Windows, постройте путь так :
>
>
	CACHES = {
            "default": {
	        "BACKEND": "django.core.cache.backends.filebased.FileBasedCache",
	        "LOCATION": "c:/foo/bar",
	    }
	 }
 




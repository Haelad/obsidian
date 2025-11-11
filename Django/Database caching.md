Для того хранить кэш в базе данных требуется

использовать вашу таблицу в базе данных как место для кэша:

- Настройте [`BACKEND`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHES-BACKEND) к `django.core.cache.backends.db.DatabaseCache`
    
- Настройте [`LOCATION`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHES-LOCATION) к `tablename`, таблице вашей базы данных.

>[!note] Примечание
>Имя таблицы может быть каким угодно, пока это имя не используется в базе данных.


к примеру:
```python
CACHES = {
    "default": {
        "BACKEND": "django.core.cache.backends.db.DatabaseCache",
        "LOCATION": "my_cache_table",
    }
}
```

>[!warning] Важно знать
>В отличие от других бэкэндов кэша, кэш базы данных не поддерживает автоматическую выбраковку просроченных записей на уровне базы данных. Вместо этого просроченные записи кэша выбраковываются каждый раз при вызове `add()`, `set()` или `touch()`.


нужно перевести и структурировать статью, вырезав и переформулировав все не нужное, сократив объем материал.

! побить текст на темы поменьше



неполная - https://habr.com/ru/companies/otus/articles/775318/


### `CACHES`[¶](https://docs.djangoproject.com/en/5.1/ref/settings/#caches "Permalink to this heading")

что такое кэш?
как настроить и подключить кэш?

###### **Кэш** - это **Специальная область на диске или в операционной памяти компьютера, предназначенная для временного хранения информации и для часто используемых данных и команд.**

Обыкновенные настройки кэша:

```python
CACHES =  {
    "default": {
        "BACKEND": "django.core.cache.backends.locmem.LocMemCache",
    }
}
```

Это словарь, содержащий настройки для всех кэшей, которые будут использоваться с Django. Это вложенный словарь, содержимое которого сопоставляет псевдонимы кэша со словарем, содержащим параметры для отдельного кэша.

Параметр [`CACHES`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHES) должен настроить кэш по умолчанию; 
Также может быть указано любое количество дополнительных кэшей. 
Если вы используете бэкэнд кэша, отличный от локального кэша памяти, или вам нужно определить несколько кэшей, потребуются другие параметры. 
Доступны следующие параметры кэша :

#### `BACKEND`[¶](https://docs.djangoproject.com/en/5.1/ref/settings/#backend "Permalink to this heading")

Default: `''"` (Empty string)

Кэш-бэкэнд для использования. Встроенные кэш-бэкэнды:

- `'django.core.cache.backends.db.DatabaseCache'`
    
- `'django.core.cache.backends.dummy.DummyCache'`
    
- `'django.core.cache.backends.filebased.FileBasedCache'`
    
- `'django.core.cache.backends.locmem.LocMemCache'`
    
- `'django.core.cache.backends.memcached.PyMemcacheCache'`
    
- `'django.core.cache.backends.memcached.PyLibMCCache'`
    
- `'django.core.cache.backends.redis.RedisCache'`


Вы можете использовать бэкэнд кэша, который не поставляется с Django, установив [`BACKEND`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHES-BACKEND) на полный путь к классу бэкэнда кэша (например, `mypackage.backends.whatever.WhateverCache`).

#### `LOCATION`[¶](https://docs.djangoproject.com/en/5.1/ref/settings/#location "Permalink to this heading")

Default: `''` (Empty string)

Расположение кэша для использования. 

Это может быть каталог для кэша файловой системы, хост и порт для сервера memcache или идентификационное имя для локального кэша памяти. Например:

```python
CACHES = {
    "default": {
        "BACKEND": "django.core.cache.backends.filebased.FileBasedCache",
        "LOCATION": "/var/tmp/django_cache",
    }
}
```


### Cache arguments[¶](https://docs.djangoproject.com/en/5.1/topics/cache/#cache-arguments "Permalink to this heading")

Каждому бэкэнду кэша можно задать дополнительные аргументы для управления поведением кэширования. Эти аргументы предоставляются как дополнительные ключи в настройке [`CACHES`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHES). 
Допустимые аргументы следующие:

- [`TIMEOUT`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHES-TIMEOUT): Тайм-аут по умолчанию в секундах для использования в кэше. Этот аргумент по умолчанию равен `300` секундам (5 минут). Вы можете установить `TIMEOUT` на `None`, чтобы по умолчанию ключи кэша никогда не истекали. Значение `0` приводит к немедленному истечению срока действия ключей (фактически «не кэшируются»).
    
- [`OPTIONS`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHES-OPTIONS): Любые параметры, которые должны быть переданы в бэкэнд кэша. Список допустимых параметров будет отличаться для каждого бэкэнда, а бэкэнды кэша, поддерживаемые сторонней библиотекой, будут передавать свои параметры непосредственно в базовую библиотеку кэша.
    
    Бэкэнды кэширования, реализующие собственную стратегию отбраковки (т. е. бэкэнды `locmem`, `filesystem` и `database`), будут учитывать следующие параметры:
    
    - `MAX_ENTRIES`: 
	    Максимальное количество записей, разрешенных в кэше, перед удалением старых значений. Этот аргумент по умолчанию равен `300`.
    
    - `CULL_FREQUENCY`: 
	    Доля записей, которые отбраковываются при достижении `MAX_ENTRIES`. (Фактическое соотношение составляет `1 / CULL_FREQUENCY`, поэтому установите `CULL_FREQUENCY` на `2`, чтобы отбраковать половину записей при достижении `MAX_ENTRIES`. Этот аргумент должен быть целым числом и по умолчанию равен `3`.)
        
        Значение `0` для `CULL_FREQUENCY` означает, что весь кэш будет сброшен при достижении `MAX_ENTRIES`. На некоторых бэкэндах (в частности, `database`) это делает отбраковку _гораздо_ быстрее за счет большего количества промахов кэша.
        
    
	Бэкэнды **Memcached** и **Redis** передают содержимое [`OPTIONS`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHES-OPTIONS) в качестве keyword arguments клиентским конструкторам, что позволяет осуществлять более расширенный контроль поведения клиента. Пример использования см. ниже.
    
- [`KEY_PREFIX`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHES-KEY_PREFIX): Строка, которая будет автоматически включена (по умолчанию добавлена ​​в начало) ко всем ключам кэша, используемым сервером Django.
    
    See the [cache documentation](https://docs.djangoproject.com/en/5.1/topics/cache/#cache-key-prefixing) for more information.
    
- [`VERSION`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHES-VERSION): Номер версии по умолчанию для ключей кэша, сгенерированных сервером Django.
    
    See the [cache documentation](https://docs.djangoproject.com/en/5.1/topics/cache/#cache-versioning) for more information.
    
- [`KEY_FUNCTION`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHES-KEY_FUNCTION) Строка, содержащая точечный путь к функции, которая определяет, как составить префикс, версию и ключ в окончательный ключ кэша.


для примера как могут выглядеть настройки одного бэкэнда :

```python
CACHES = {
    "default": {
        "BACKEND": "django.core.cache.backends.memcached.PyMemcacheCache",
        "LOCATION": "127.0.0.1:11211",
        "OPTIONS": {
            "no_delay": True,
            "ignore_exc": True,
            "max_pool_size": 4,
            "use_pooling": True,
        },
    }
}
```
мы указываем сам сервер кэша, который хотим использовать, затем место где его будем хранить и пользоваться данными, и специфичные настройки бэкэнда, которые могут быть разными (в зависимости каким бэкэндом мы пользуемся )


## The per-site cache[¶](https://docs.djangoproject.com/en/5.1/topics/cache/#the-per-site-cache "Permalink to this heading")
Кэш для каждого сайта

После настройки кэша самый простой способ использовать кэширование — кэшировать весь сайт. Вам нужно будет добавить: `'django.middleware.cache.UpdateCacheMiddleware'` `'django.middleware.cache.FetchFromCacheMiddleware'`
в настройку [`MIDDLEWARE`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-MIDDLEWARE)


пример:

```python
MIDDLEWARE = [
    "django.middleware.cache.UpdateCacheMiddleware",
    "django.middleware.common.CommonMiddleware",
    "django.middleware.cache.FetchFromCacheMiddleware",
]
```

Затем добавьте следующие обязательные настройки в файл настроек Django:

- [`CACHE_MIDDLEWARE_ALIAS`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHE_MIDDLEWARE_ALIAS) – псевдоним кэша, который будет использоваться для хранения.

- [`CACHE_MIDDLEWARE_SECONDS`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHE_MIDDLEWARE_SECONDS) – целое число секунд, в течение которых каждая страница должна кэшироваться.

- [`CACHE_MIDDLEWARE_KEY_PREFIX`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHE_MIDDLEWARE_KEY_PREFIX) – Если кэш используется несколькими сайтами, использующими одну и ту же установку Django, задайте здесь имя сайта или какую-либо другую строку, уникальную для этого экземпляра Django, чтобы предотвратить конфликты ключей. Используйте пустую строку, если вам все равно.(Строка, которая будет стоять перед ключами кэша, созданными промежуточным программным обеспечением кэша. Этот префикс сочетается с настройкой KEY_PREFIX; оно не заменяет его.)

`FetchFromCacheMiddleware` кэширует ответы GET и HEAD со статусом 200, если заголовки запроса и ответа это допускают. Ответы на запросы для одного и того же URL с разными параметрами запроса считаются уникальными страницами и кэшируются отдельно. Это промежуточное ПО ожидает, что на запрос HEAD будет дан ответ с теми же заголовками ответа, что и на соответствующий запрос GET; в этом случае оно может вернуть кэшированный ответ GET для запроса HEAD.

Кроме того, `UpdateCacheMiddleware` автоматически устанавливает несколько заголовков в каждом [`HttpResponse`](https://docs.djangoproject.com/en/5.1/ref/request-response/#django.http.HttpResponse "django.http.HttpResponse"), которые влияют на [нижние кэши](https://docs.djangoproject.com/en/5.1/topics/cache/#downstream-caches) :

- Устанавливает заголовок `Expires` на текущую дату/время плюс заданные [`CACHE_MIDDLEWARE_SECONDS`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHE_MIDDLEWARE_SECONDS).

- Устанавливает заголовок `Cache-Control` для указания максимального возраста страницы – опять же, из настройки [`CACHE_MIDDLEWARE_SECONDS`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHE_MIDDLEWARE_SECONDS).

Подробнее о промежуточном программном обеспечении см. в [Middleware](https://docs.djangoproject.com/en/5.1/topics/http/middleware/).

IЕсли представление устанавливает собственное время истечения срока действия кэша (т. е. в его заголовке `Cache-Control` есть раздел `max-age`), то страница будет кэшироваться до истечения срока действия, а не [`CACHE_MIDDLEWARE_SECONDS`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHE_MIDDLEWARE_SECONDS). Используя декораторы в `django.views.decorators.cache`, вы можете легко задать время истечения срока действия представления (используя декоратор [`cache_control()`](https://docs.djangoproject.com/en/5.1/topics/http/decorators/#django.views.decorators.cache.cache_control "django.views.decorators.cache.cache_control")) или отключить кэширование для представления (используя декоратор [`never_cache()`](https://docs.djangoproject.com/en/5.1/topics/http/decorators/#django.views.decorators.cache.never_cache "django.views.decorators.cache.never_cache"). Подробнее об этих декораторах см. в разделе [использование других заголовков](https://docs.djangoproject.com/en/5.1/topics/cache/#controlling-cache-using-other-headers).

Если [`USE_I18N`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-USE_I18N) установлено в `True`, то сгенерированный ключ кэша будет включать имя активного [языка](https://docs.djangoproject.com/en/5.1/topics/i18n/#term-language-code) – см. также [Как Django обнаруживает языковые предпочтения](https://docs.djangoproject.com/en/5.1/topics/i18n/translation/#how-django-discovers-language-preference)). Это позволяет вам легко кэшировать многоязычные сайты без необходимости самостоятельно создавать ключ кэша.

Ключи кэша также включают [текущий часовой пояс](https://docs.djangoproject.com/en/5.1/topics/i18n/timezones/#default-current-time-zone), когда [`USE_TZ`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-USE_TZ) установлено в `True`.

## The per-view cache[¶](https://docs.djangoproject.com/en/5.1/topics/cache/#the-per-view-cache "Permalink to this heading")
вьюшки

django.views.decorators.cache.cache_page(_timeout_, _*_, _cache=None_, _key_prefix=None_)[¶](https://docs.djangoproject.com/en/5.1/topics/cache/#django.views.decorators.cache.cache_page "Permalink to this definition")

Более детальный способ использования кэширующего фреймворка — кэширование вывода отдельных представлений. `django.views.decorators.cache` определяет декоратор `cache_page`, который автоматически кэширует ответ представления для вас:



```python
from django.views.decorators.cache import cache_page
@cache_page(60 * 15)
def my_view(request):
	...
```
`cache_page` принимает один аргумент: тайм-аут кэша в секундах. В приведенном выше примере результат представления `my_view()` будет кэшироваться в течение 15 минут. (Обратите внимание, что мы записали его как `60 * 15` для удобства чтения. `60 * 15` будет оценено как `900` – то есть 15 минут, умноженных на 60 секунд в минуту.)

Тайм-аут кэша, установленный `cache_page`, имеет приоритет над директивой `max-age` из заголовка `Cache-Control`.

Кэш для каждого представления, как и кэш для каждого сайта, привязан к URL. Если несколько URL указывают на одно и то же представление, каждый URL будет кэшироваться отдельно. Продолжая пример `my_view`, если ваша конфигурация URL выглядит следующим образом:

```python
urlpatterns = [
    path("foo/<int:code>/", my_view),
]
```

then requests to `/foo/1/` and `/foo/23/` will be cached separately, as you may expect. But once a particular URL (e.g., `/foo/23/`) has been requested, subsequent requests to that URL will use the cache.

`cache_page` can also take an optional keyword argument, `cache`, which directs the decorator to use a specific cache (from your [`CACHES`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHES) setting) when caching view results. By default, the `default` cache will be used, but you can specify any cache you want:

```python
@cache_page(60 * 15, cache="special_cache")
def my_view(request): ...
```

You can also override the cache prefix on a per-view basis. `cache_page` takes an optional keyword argument, `key_prefix`, which works in the same way as the [`CACHE_MIDDLEWARE_KEY_PREFIX`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHE_MIDDLEWARE_KEY_PREFIX) setting for the middleware. It can be used like this:

```python
@cache_page(60 * 15, key_prefix="site1")
def my_view(request): ...
```

The `key_prefix` and `cache` arguments may be specified together. The `key_prefix` argument and the [`KEY_PREFIX`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHES-KEY_PREFIX) specified under [`CACHES`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHES) will be concatenated.

Additionally, `cache_page` automatically sets `Cache-Control` and `Expires` headers in the response which affect [downstream caches](https://docs.djangoproject.com/en/5.1/topics/cache/#downstream-caches).


### Specifying per-view cache in the URLconf[¶](https://docs.djangoproject.com/en/5.1/topics/cache/#specifying-per-view-cache-in-the-urlconf "Permalink to this heading")
url конфиги

Примеры в предыдущем разделе жестко запрограммировали тот факт, что представление кэшируется, потому что `cache_page` изменяет функцию `my_view` на месте. Этот подход связывает ваше представление с системой кэширования, что не является идеальным по нескольким причинам. Например, вы можете захотеть повторно использовать функции представления на другом сайте без кэширования или вы можете захотеть распространить представления среди людей, которые могут захотеть использовать их без кэширования. Решение этих проблем заключается в указании кэша для каждого представления в URLconf, а не рядом с самими функциями представления.

Вы можете сделать это, обернув функцию представления в `cache_page`, когда вы ссылаетесь на нее в URLconf. Вот старый URLconf из более раннего варианта:

```python 
urlpatterns = [
    path("foo/<int:code>/", my_view),
]
```

Here’s the same thing, with `my_view` wrapped in `cache_page`:

```python
from django.views.decorators.cache import cache_page

urlpatterns = [
    path("foo/<int:code>/", cache_page(60 * 15)(my_view)),
```

## Template fragment caching[¶](https://docs.djangoproject.com/en/5.1/topics/cache/#template-fragment-caching "Permalink to this heading")
шаблоны

Если вам нужен еще больший контроль, вы также можете кэшировать фрагменты шаблона с помощью тега шаблона `cache`. Чтобы предоставить вашему шаблону доступ к этому тегу, поместите `{% load cache %}` в верхней части шаблона.

Тег шаблона `{% cache %}` кэширует содержимое блока на заданное время. Он принимает как минимум два аргумента: тайм-аут кэша в секундах и имя, которое нужно дать фрагменту кэша. Фрагмент кэшируется навсегда, если тайм-аут равен `None`. Имя будет принято как есть, не используйте переменную. Например:

```django
{% load cache %}
{% cache 500 sidebar %}
    .. sidebar ..
{% endcache %}
```
Иногда вам может понадобиться кэшировать несколько копий фрагмента в зависимости от некоторых динамических данных, которые появляются внутри фрагмента. 
Например, вам может понадобиться отдельная кэшированная копия боковой панели, использованной в предыдущем примере, для каждого пользователя вашего сайта. 
Сделайте это, передав один или несколько дополнительных аргументов, которые могут быть переменными с фильтрами или без них, в тег шаблона `{% cache %}` для уникальной идентификации фрагмента кэша:

```django
{% load cache %}
{% cache 500 sidebar request.user.username %}
    .. sidebar for logged in user ..
{% endcache %}
```

Если [`USE_I18N`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-USE_I18N) установлено в `True`, кэш промежуточного программного обеспечения для каждого сайта будет [соблюдать активный язык](https://docs.djangoproject.com/en/5.1/topics/cache/#i18n-cache-key). Для тега шаблона `cache` вы можете использовать одну из [переводно-специфичных переменных](https://docs.djangoproject.com/en/5.1/topics/i18n/translation/#template-translation-vars), доступных в шаблонах, чтобы достичь того же результата:

```django
{% load i18n %}
{% load cache %}

{% get_current_language as LANGUAGE_CODE %}

{% cache 600 welcome LANGUAGE_CODE %}
    {% translate "Welcome to example.com" %}
{% endcache %}
```

установление параметра таймауту:

Время ожидания кэша может быть переменной шаблона, пока переменная шаблона разрешается в целочисленное значение. Например, если переменная шаблона `my_timeout` установлена ​​в значение `600`, то следующие два примера эквивалентны:
```django
{% cache 600 sidebar %} ... {% endcache %}
{% cache my_timeout sidebar %} ... {% endcache %}
```

Эта функция полезна для избежания повторений в шаблонах. Вы можете установить тайм-аут в переменной в одном месте и повторно использовать это значение.

установление параметра template_fragments:

По умолчанию тег кэша попытается использовать кэш под названием «template_fragments». Если такого кэша не существует, он вернется к использованию кэша по умолчанию. Вы можете выбрать альтернативный бэкэнд кэша для использования с аргументом ключевого слова `using`, который должен быть последним аргументом тега.

```django
{% cache 300 local-thing ...  using="localcache" %}
```

Указание имени кэша, которое не настроено, считается ошибкой.

django.core.cache.utils.make_template_fragment_key(_fragment_name_, _vary_on=None_)[¶](https://docs.djangoproject.com/en/5.1/topics/cache/#django.core.cache.utils.make_template_fragment_key "Постоянная ссылка на это определение")

Если вы хотите получить ключ кэша, используемый для кэшированного фрагмента, вы можете использовать `make_template_fragment_key`. `fragment_name` совпадает со вторым аргументом тега шаблона `cache`; `vary_on` — это список всех дополнительных аргументов, переданных тегу. Эта функция может быть полезна для аннулирования или перезаписи кэшированного элемента, например:

```python
>>> from django.core.cache import cache
>>> from django.core.cache.utils import make_template_fragment_key
# cache key for {% cache 500 sidebar username %}
>>> key = make_template_fragment_key("sidebar", [username])
>>> cache.delete(key)  # invalidates cached template fragment
```

## Controlling cache: Using other headers[¶](https://docs.djangoproject.com/en/5.1/topics/cache/#controlling-cache-using-other-headers "Permalink to this heading")
 конфиденциальность данных

Другие проблемы с кэшированием — конфиденциальность данных и вопрос о том, где данные должны храниться в каскаде кэшей.

Обычно пользователь сталкивается с двумя типами кэшей: кэш собственного браузера (частный кэш) и кэш провайдера (публичный кэш). 

Публичный кэш используется несколькими пользователями и контролируется кем-то другим. Это создает проблемы с конфиденциальными данными — вы не хотите, чтобы, скажем, номер вашего банковского счета хранился в публичном кэше. Поэтому веб-приложениям нужен способ сообщать кэшам, какие данные являются частными, а какие — публичными.

Решение — указать, что кэш страницы должен быть «частным». Чтобы сделать это в Django, используйте декоратор представления [`cache_control()`](https://docs.djangoproject.com/en/5.1/topics/http/decorators/#django.views.decorators.cache.cache_control "django.views.decorators.cache.cache_control"). Пример:

```python
from django.views.decorators.cache import cache_control

@cache_control(private=True)
def my_view(request):
	...
```

Этот декоратор заботится об отправке соответствующего заголовка HTTP за кулисами.

Обратите внимание, что настройки управления кэшем «private» и «public» являются взаимоисключающими. Декоратор гарантирует, что директива «public» будет удалена, если должна быть установлена ​​«private» (и наоборот). Примером использования двух директив может быть сайт блога, который предлагает как частные, так и публичные записи. Публичные записи могут кэшироваться в любом общем кэше. Следующий код использует [`patch_cache_control()`](https://docs.djangoproject.com/en/5.1/ref/utils/#django.utils.cache.patch_cache_control "django.utils.cache.patch_cache_control"), ручной способ изменения заголовка управления кэшем (он вызывается внутренне декоратором [`cache_control()`](https://docs.djangoproject.com/en/5.1/topics/http/decorators/#django.views.decorators.cache.cache_control "django.views.decorators.cache.cache_control")):



```python
from django.views.decorators.cache import patch_cache_control
from django.views.decorators.vary import vary_on_cookie


@vary_on_cookie
def list_blog_entries_view(request):
    if request.user.is_anonymous:
        response = render_only_public_entries()
        patch_cache_control(response, public=True)
    else:
        response = render_private_and_public_entries(request.user)
        patch_cache_control(response, private=True)

    return response
```

Вы также можете управлять кэшами нисходящего потока другими способами (подробнее о кэшировании HTTP см. в [**RFC 9111**](https://datatracker.ietf.org/doc/html/rfc9111.html)). Например, даже если вы не используете серверную структуру кэширования Django, вы все равно можете указать клиентам кэшировать представление на определенное время с помощью директивы [**max-age**](https://datatracker.ietf.org/doc/html/rfc9111.html#section-5.2.2.1):

```python
from django.views.decorators.cache import cache_control

@cache_control(max_age=3600)
def my_view(request): ...
```

(Если вы _используете_ промежуточное ПО кэширования, оно уже устанавливает `max-age` со значением параметра [`CACHE_MIDDLEWARE_SECONDS`](https://docs.djangoproject.com/en/5.1/ref/settings/#std-setting-CACHE_MIDDLEWARE_SECONDS). В этом случае пользовательский `max_age` из декоратор [`cache_control()`](https://docs.djangoproject.com/en/5.1/topics/http/decorators/#django.views.decorators.cache.cache_control "django.views.decorators.cache.cache_control") будет иметь приоритет, и значения заголовков будут объединены правильно.)
Любая допустимая директива ответа `Cache-Control` допустима в `cache_control()`. Вот еще несколько примеров:

- `no_transform=True`
    
- `must_revalidate=True`
    
- `stale_while_revalidate=num_seconds`
    
- `no_cache=True`
    

Полный список известных директив можно найти в [реестре IANA](https://www.iana.org/assignments/http-cache-directives/http-cache-directives.xhtml) (обратите внимание, что не все из них применяются к ответам).

Если вы хотите использовать заголовки для полного отключения кэширования, [`never_cache()`](https://docs.djangoproject.com/en/5.1/topics/http/decorators/#django.views.decorators.cache.never_cache "django.views.decorators.cache.never_cache") — это декоратор представления, который добавляет заголовки, чтобы гарантировать, что ответ не будет кэшироваться браузерами или другими кэшами. Пример:

``` python
from django.views.decorators.cache import never_cache

@never_cache
def myview(request): ...
```
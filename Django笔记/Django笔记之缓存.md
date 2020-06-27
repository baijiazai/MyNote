# Django笔记之缓存

### 一、数据库缓存

- 创建缓存表，在终端输入命令

```bash
python manage.py createcachetable my_cache_table
```

- settings.py

```python
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.db.DatabaseCache',
        'LOCATION': 'my_cache_table',
    }
}
```

- views.py

```python
@cache_page(timeout=60)
def cache_list(request):
    arr = ['The number is %d' % i for i in range(10)]
    time.sleep(5)  # 伪装耗时
    return render(request, 'demo/cache_list.html', {'arr': arr})
```

### 二、内存缓存

- 安装 redis

```bash
sudo apt-get install redis-server
```

- 安装插件

```bash
pip install django-redis
pip install django-redis-cache
```

- settings.py

```python
CACHES = {
    'default': {
        "BACKEND": "django_redis.cache.RedisCache",
        "LOCATION": "redis://127.0.0.1:6379/1",
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
        }
    }
}
```

### 多缓存

- settings.py

```python
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.db.DatabaseCache',
        'LOCATION': 'my_cache_table',
    },
    'redis_backend': {
    	"BACKEND": "django_redis.cache.RedisCache",
        "LOCATION": "redis://127.0.0.1:6379/1",
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
        }
    }
}
```

- views.py

```python
@cache_page(timeout=60, cache='default')
def cache_list(request):
    arr = ['The number is %d' % i for i in range(10)]
    time.sleep(5)  # 伪装耗时
    return render(request, 'demo/cache_list.html', {'arr': arr})


@cache_page(timeout=60, cache='redis_backend')
def redis_cache_list(request):
    arr = ['The number is %d' % i for i in range(10)]
    time.sleep(5)  # 伪装耗时
    return render(request, 'demo/cache_list.html', {'arr': arr})

```




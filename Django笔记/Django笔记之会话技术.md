# Django笔记之会话技术

### 一、cookie 

> 客户端会话技术，数据存储在客户端

- views.py

```python
def cookie_demo(request):
    response = HttpResponse('Hello World!')
    # 设置 cookie (key, value, max_age=最大过期时间，单位秒/s)
    response.set_cookie('user', 'lala', max_age=60)
    # 获取 cookie
    request.COOKIES.get('user')
    # 设置加盐 cookie (key, value, salt=加盐字符串)
    response.set_signed_cookie('name', 'lala', salt='haha')
    # 获取加盐 cookie (key, salt=加盐字符串)
    request.get_signed_cookie('name', salt='haha')
    # 删除单个 cookie
    response.delete_cookie('user')
    return response
```

### 二、session

> 服务端会话技术，数据存储在服务器，session 依赖于 cookie

- views.py

```python
def session_demo(request):
	# 设置 session
    request['user'] = 'lala'
    # 获取 session
    request.session.get('user')
    # 删除当前 session 并删除 cookie
    request.session.flush()
    # 清空 session
    request.session.clear()
```

- template.py

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
user:{{ request.session.user }}
</body>
</html>
```

### 三、token

> 服务端会话技术，自定义 session




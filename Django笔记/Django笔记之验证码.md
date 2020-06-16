# Django笔记之验证码

- settings.py

  ```python
  STATIC_ROOT = os.path.join(BASE_DIR, 'static')
  ```

- urls.py

  ```python
  from django.urls import path
  
  from demo import views
  
  app_name = 'demo'
  urlpatterns = [
      path('auth_code', views.auth_code, name='auth_code'),
      path('get_code', views.get_code, name='get_code'),
  ]
  ```

- views.py

  ```python
  import os
  import random
  from random import randint
  import string
  
  from PIL import Image, ImageFont, ImageDraw
  from django.http import HttpResponse
  from django.shortcuts import render
  from django.utils.six import BytesIO
  
  from Mall.settings import STATIC_ROOT
  
  
  # 验证码
  def auth_code(request):
      if request.method == 'POST':
          input_code = request.POST.get('code')
          msg = '对' if input_code == request.session.get('check_code') else '错'
          return render(request, 'demo/auth_code.html', {'msg': msg})
      return render(request, 'demo/auth_code.html')
  
  
  # 获取验证码
  def get_code(request):
      img = Image.new('RGB', (120, 60), (255, 255, 255))
      font = ImageFont.truetype(os.path.join(STATIC_ROOT, 'demo/fonts/DroidSans.ttf'), randint(25, 30))
      draw = ImageDraw.Draw(img)
      check_code = ''.join(random.choices(string.ascii_letters + string.digits, k=6))
      # 元素点
      for _ in range(1000):
          draw.point((randint(0, 120), randint(0, 60)), (randint(0, 255), randint(0, 255), randint(0, 255)))
      # 验证码
      for i in range(6):
          draw.text((5 + i * 20, randint(5, 35)), check_code[i], (0, 0, 0), font)
      # 横线
      for x in range(0, 121):
          for y in range(15, 46, 15):
              draw.point((x, y), (0, 0, 0))
      # 缓存
      fp = BytesIO()
      img.save(fp, 'png')
      request.session['check_code'] = check_code
      return HttpResponse(fp.getvalue(), content_type='image/png')
  ```

- auth_code.html

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>auth code</title>
  </head>
  <body>
  <form action="#" method="post">
      {% csrf_token %}
      <label>验证码：
          <input type="text" name="code" maxlength="6" minlength="6" required>
      </label>
      <img src="{% url 'demo:get_code' %}" alt="" id="auth_code">
      <input type="submit" value="check">
      {{ msg }}
  </form>
  
  <script>
      window.onload = function () {
          var oAuthCode = document.getElementById('auth_code');
          oAuthCode.onclick = function(){
              this.src = '{% url 'demo:get_code' %}' + '?t=' + Math.random() * 10;
          }
      }
  </script>
  </body>
  </html>
  ```

  
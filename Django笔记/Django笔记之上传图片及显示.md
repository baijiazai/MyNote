# Django笔记之上传文件及显示

- urls.py

  ```python
  from django.conf.urls.static import static
  from django.contrib import admin
  from django.urls import path, include
  
  from Mall import settings
  
  urlpatterns = [
      path('admin/', admin.site.urls),
      path('demo/', include('demo.urls', namespace='demo')),
  ] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
  ```

- settings.py

  ```python
  # 存放用户上传文件的目录的绝对文件系统路径。
  MEDIA_ROOT = os.path.join(BASE_DIR, 'media/uploads')
  # 用于管理从其提供的媒体
  MEDIA_URL = '/media/'
  ```

- demo.models.py

  ```python
  from django.db import models
  
  
  class Upload(models.Model):
      img = models.ImageField(upload_to='%Y/%m/%d')
  ```

- demo.urls.py

  ```python
  from django.urls import path
  
  from demo import views
  
  app_name = 'demo'
  urlpatterns = [
      path('upload', views.upload, name='upload'),
  ]
  ```

- demo.views.py

  ```python
  from django.http import HttpResponseRedirect
  from django.shortcuts import render
  from django.urls import reverse
  
  from demo.models import Upload
  
  
  def upload(request):
      if request.method == 'POST':
          img = request.FILES.get('img')
          Upload.objects.create(img=img)
          return HttpResponseRedirect(reverse('demo:upload'))
      images = Upload.objects.all()
      return render(request, 'demo/upload.html', {'images': images})
  ```

- demo/upload.html

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>upload</title>
  </head>
  <body>
  <form action="#" method="post" enctype="multipart/form-data">
      {% csrf_token %}
      <input type="file" name="img" required>
      <input type="submit" value="upload">
  </form>
  {% for image in images %}
      <img src="{{ image.img.url }}" alt="" width="25%">
  {% endfor %}
  </body>
  </html>
  ```

  
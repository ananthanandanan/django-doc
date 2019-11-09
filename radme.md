### Django Quick Look.

## A simple view 
```python

from django.http import HttpResponse


def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")

```

## app/url

this were the app looks to the view part
```python
rom django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
]

```

## URLconf setup :
+ root_app/url
This is to make the root url.py to look to the view
```python
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]

```

## Simple model 

 models – essentially, your database layout, with additional metadata.

 ```python
from django.db import models


class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')


class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
 ```
 [For more reference on models](https://docs.djangoproject.com/en/2.2/intro/tutorial02/)
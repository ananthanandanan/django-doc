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

 + Another example of model

 
```python

from django.db import models

class Blog(models.Model):
    name = models.CharField(max_length=100)
    tagline = models.TextField()

    def __str__(self):
        return self.name

class Author(models.Model):
    name = models.CharField(max_length=200)
    email = models.EmailField()

    def __str__(self):
        return self.name

class Entry(models.Model):
    blog = models.ForeignKey(Blog, on_delete=models.CASCADE)
    headline = models.CharField(max_length=255)
    body_text = models.TextField()
    pub_date = models.DateField()
    mod_date = models.DateField()
    authors = models.ManyToManyField(Author)
    number_of_comments = models.IntegerField()
    number_of_pingbacks = models.IntegerField()
    rating = models.IntegerField()

    def __str__(self):
        return self.headline
 
 ```

 #### creating Objects

 eg : blog class:
  ```python

from blog.models import Blog
 b = Blog(name='Beatles Blog', tagline='All the latest Beatles news.')
b.save()

  ```
#### Filtering

```python
ntry.objects.filter(pub_date__year=2006)
```
+ Chaining Filters

```python

Entry.objects.filter(
...     headline__startswith='What'
... ).exclude(
...     pub_date__gte=datetime.date.today()
... ).filter(
...     pub_date__gte=datetime.date(2005, 1, 30)
... )
```

+ Limitiing QuerySet
```python
Entry.objects.all()[:5]
```

####  [field-lookup](https://docs.djangoproject.com/en/2.2/ref/models/querysets/#field-lookups)

This is basically how the SQL query would relate to.

+ **RegeX**
```python 

Entry.objects.get(title__regex=r'^(An?|The) +')

```

#### Conditional expressions

eg :
```python 
from django.db import models

class Client(models.Model):
    REGULAR = 'R'
    GOLD = 'G'
    PLATINUM = 'P'
    ACCOUNT_TYPE_CHOICES = [
        (REGULAR, 'Regular'),
        (GOLD, 'Gold'),
        (PLATINUM, 'Platinum'),
    ]
    name = models.CharField(max_length=50)
    registered_on = models.DateField()
    account_type = models.CharField(
        max_length=1,
        choices=ACCOUNT_TYPE_CHOICES,
        default=REGULAR,
    )
```
+ When 

A When() object is used to encapsulate a condition and its result for use in the conditional expression. Using a When() object is similar to using the filter() method
```python 
from django.db.models import F, Q, When
>>> # String arguments refer to fields; the following two examples are equivalent:
>>> When(account_type=Client.GOLD, then='name')
>>> When(account_type=Client.GOLD, then=F('name'))
>>> # You can use field lookups in the condition
```
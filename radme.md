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

A When() object is used to encapsulate a condition and its result for use in the conditional expression. Using a When() object is similar to using the filter() method. The condition can be specified using field lookups or Q objects. The result is provided using the then keyword.

```python 
from django.db.models import F, Q, When
>>> # String arguments refer to fields; the following two examples are equivalent:
>>> When(account_type=Client.GOLD, then='name') # then is the break through or result part.
>>> When(account_type=Client.GOLD, then=F('name'))
>>> # You can use field lookups in the condition
```


#### httpRequest object 

+ httpRequest.scheme


A string representing the scheme of the request (http or https usually).

 + HttpRequest.META¶

    A dictionary containing all available HTTP headers. Available headers depend on the client and server, but here are some examples

            
- CONTENT_LENGTH – The length of the request body (as a string).
- CONTENT_TYPE – The MIME type of the request body.
- HTTP_ACCEPT – Acceptable content types for the response.
- HTTP_ACCEPT_ENCODING – Acceptable encodings for the response.
- HTTP_ACCEPT_LANGUAGE – Acceptable languages for the response.
- HTTP_HOST – The HTTP Host header sent by the client.
- HTTP_REFERER – The referring page, if any.
- HTTP_USER_AGENT – The client’s user-agent string.
- QUERY_STRING – The query string, as a single (unparsed) string.
- REMOTE_ADDR – The IP address of the client.
- REMOTE_HOST – The hostname of the client.
- REMOTE_USER – The user authenticated by the Web server, if any.
- REQUEST_METHOD – A string such as "GET" or "POST".
- SERVER_NAME – The hostname of the server.
- SERVER_PORT – The port of the server (as a string).


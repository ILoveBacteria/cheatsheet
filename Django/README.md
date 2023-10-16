# Django Cheatsheet

{% raw %}
## Table Of Contents

- [Django Cheatsheet](#django-cheatsheet)
  - [Table Of Contents](#table-of-contents)
  - [Django-Admin Commands](#django-admin-commands)
    - [Built-in Commands](#built-in-commands)
    - [Custom Commands](#custom-commands)
  - [CSRF Protection](#csrf-protection)
    - [Setting the token on the AJAX request](#setting-the-token-on-the-ajax-request)
    - [Dynamic Forms](#dynamic-forms)
    - [Decorators](#decorators)
  - [Handwrite Notes](#handwrite-notes)
  - [Forms](#forms)
    - [How to write a minimal form in Django](#how-to-write-a-minimal-form-in-django)
    - [A simple form in Django](#a-simple-form-in-django)
    - [Bound and unbound form instances](#bound-and-unbound-form-instances)
    - [Access form values](#access-form-values)
    - [Custom Validators](#custom-validators)
  - [Authentication](#authentication)
    - [Authentication Settings](#authentication-settings)
    - [Authentication Views](#authentication-views)
    - [is\_authenticated](#is_authenticated)
    - [How to log a user in](#how-to-log-a-user-in)
    - [How to log a user out](#how-to-log-a-user-out)
    - [Decorators](#decorators-1)
  - [Django Admin](#django-admin)
    - [Register Models](#register-models)
    - [Customizing Models](#customizing-models)
    - [Adding Search Box](#adding-search-box)
    - [Inlines](#inlines)
    - [Actions](#actions)
  - [`HttpRequest`](#httprequest)
  - [Http Method Decorators](#http-method-decorators)
  - [Generic Views](#generic-views)
  - [Cursor Pagination](#cursor-pagination)
  - [URL Dispatcher](#url-dispatcher)
  - [Flash Messages](#flash-messages)
    - [Add and use message](#add-and-use-message)
    - [Message\_TAGS](#message_tags)
  - [Template Language](#template-language)
    - [Filter](#filter)
      - [Tip](#tip)
    - [Custom Template Tags and Filters](#custom-template-tags-and-filters)
      - [First Step](#first-step)
      - [Load Modules](#load-modules)
      - [Register](#register)
    - [url](#url)
    - [Context Processors](#context-processors)
  - [Models](#models)
    - [Choices](#choices)
    - [Field Type Parameters](#field-type-parameters)
    - [Deep Copy](#deep-copy)
    - [Manager](#manager)
      - [Manager Names](#manager-names)
      - [Extra Manager](#extra-manager)
      - [Modifying a manager’s initial `QuerySet`](#modifying-a-managers-initial-queryset)
      - [Default managers](#default-managers)
    - [Field lookups](#field-lookups)
    - [Aggregate](#aggregate)
    - [Relations](#relations)
      - [OneToMany](#onetomany)
      - [ManyToMany](#manytomany)
      - [OneToOne](#onetoone)
  - [Django Rest Framework](#django-rest-framework)
    - [Serializer](#serializer)
      - [`Serializer`](#serializer-1)
      - [`ModelSerializer`](#modelserializer)
      - [Custom Method Field](#custom-method-field)
    - [Function Based Views](#function-based-views)
    - [Class Based Views](#class-based-views)
      - [`APIView`](#apiview)
      - [`RetrieveAPIView`](#retrieveapiview)
      - [`CreateAPIView`](#createapiview)
      - [`ListAPIView`](#listapiview)
      - [`ListCreateAPIView`](#listcreateapiview)
    - [Serializer Relations](#serializer-relations)
      - [`SlugRelatedField`](#slugrelatedfield)
      - [`HyperlinkedRelatedField`](#hyperlinkedrelatedfield)
    - [Requests](#requests)

## Django-Admin Commands

### Built-in Commands

- `django-admin startproject mysite`: Create a project named `mysite`
- `python manage.py runserver`: Run the server
- `python manage.py startapp polls`: Create an app named `polls`
- `python manage.py makemigrations polls`: Create migrations for `polls` app
- `python manage.py migrate`: Apply migrations
- `python manage.py createsuperuser`: Create a superuser
- `python manage.py shell`: Open a shell

### Custom Commands

Add `management/commands` directory to the application. `_` modules will be **ignored**.
```
polls/
    __init__.py
    models.py
    management/
        __init__.py
        commands/
            __init__.py
            _private.py
            closepoll.py
    tests.py
    views.py
```
Add this sample class:
```python
from django.core.management.base import BaseCommand, CommandError

class Command(BaseCommand):
    help = "Closes the specified poll for voting"

    def add_arguments(self, parser):
        # Positional arguments
        parser.add_argument("poll_ids", nargs="+", type=int)

        # Named (optional) arguments
        parser.add_argument(
            "--delete",
            action="store_true",
            help="Delete poll instead of closing it",
        )

    def handle(self, *args, **options):
        for poll_id in options["poll_ids"]:
            try:
                poll = Poll.objects.get(pk=poll_id)
            except Poll.DoesNotExist:
                raise CommandError('Poll "%s" does not exist' % poll_id)

            poll.opened = False
            poll.save()

            self.stdout.write(
                self.style.SUCCESS('Successfully closed poll "%s"' % poll_id)
            )
```

## CSRF Protection

1. The CSRF middleware is activated by default in the MIDDLEWARE setting. If you override that setting, remember that `django.middleware.csrf.CsrfViewMiddleware` should come before any view middleware that assume that CSRF attacks have been dealt with.

    If you disabled it, which is not recommended, you can use `csrf_protect()` on particular views you want to protect (see below).

2. In any template that uses a POST form, use the csrf_token tag inside the `<form>` element if the form is for an internal URL, e.g.: 
    ```html
    <form method="post">{% csrf_token %}
    ```

    This should not be done for POST forms that target external URLs, since that would cause the CSRF token to be leaked, leading to a vulnerability.

3. In the corresponding view functions, ensure that `RequestContext` is used to render the response so that `{% csrf_token %}` will work properly. If you’re using the `render()` function, generic views, or contrib apps, you are covered already since these all use `RequestContext`.

### Setting the token on the AJAX request

```javascript
const request = new Request(
    /* URL */,
    {
        method: 'POST',
        headers: {'X-CSRFToken': csrftoken},
        mode: 'same-origin' // Do not send CSRF token to another domain.
    }
);
fetch(request).then(function(response) {
    // ...
});
```

### Dynamic Forms

If your view is not rendering a template containing the `csrf_token` template tag, Django might not set the CSRF token cookie. This is common in cases where forms are dynamically added to the page. To address this case, Django provides a view decorator which forces setting of the cookie: `ensure_csrf_cookie()`.

### Decorators

- `csrf_exempt(view)`: This decorator marks a view as being exempt from the protection ensured by the middleware.
- `csrf_protect(view)`: Decorator that provides the protection of CsrfViewMiddleware to a view.
- `requires_csrf_token(view)`: Normally the csrf_token template tag will not work if CsrfViewMiddleware.process_view or an equivalent like csrf_protect has not run. The view decorator requires_csrf_token can be used to ensure the template tag does work. This decorator works similarly to csrf_protect, but never rejects an incoming request.
- `ensure_csrf_cookie(view)`: This decorator forces a view to send the CSRF cookie.

## Handwrite Notes

1. `CORS`: When site *A* wants to access content from another site *B*, it is called a Cross-Origin request. As it is disabled for security reasons, *B* sends an `Access-Control-Allow-Origin` header in the response. By default, a domain is not allowed to access an API hosted on another domain. The `Origin` header should be in request.

    [django-cors-headers][1] is a Django application for handling the server headers required for Cross-Origin Resource Sharing (CORS).

## Forms

### How to write a minimal form in Django

[Read the doc][2]

```html
<!-- polls/templates/polls/detail.html -->
<form action="{% url 'polls:vote' question.id %}" method="post">
{% csrf_token %}
<fieldset>
    <legend><h1>{{ question.question_text }}</h1></legend>
    {% if error_message %}<p><strong>{{ error_message }}</strong></p>{% endif %}
    {% for choice in question.choice_set.all %}
        <input type="radio" name="choice" id="choice{{ forloop.counter }}" value="{{ choice.id }}">
        <label for="choice{{ forloop.counter }}">{{ choice.choice_text }}</label><br>
    {% endfor %}
</fieldset>
<input type="submit" value="Vote">
</form>
```

```python
# polls/views.py
from django.http import HttpResponse, HttpResponseRedirect
from django.shortcuts import get_object_or_404, render
from django.urls import reverse

from .models import Choice, Question

def vote(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    try:
        selected_choice = question.choice_set.get(pk=request.POST['choice'])
    except (KeyError, Choice.DoesNotExist):
        # Redisplay the question voting form.
        return render(request, 'polls/detail.html', {
            'question': question,
            'error_message': "You didn't select a choice.",
        })
    else:
        selected_choice.votes += 1
        selected_choice.save()
        # Always return an HttpResponseRedirect after successfully dealing
        # with POST data. This prevents data from being posted twice if a
        # user hits the Back button.
        return HttpResponseRedirect(reverse('polls:results', args=(question.id,)))
```

Above codes are available in [gists][5]

### A simple form in Django

[Read the doc][3]

```python
# forms.py
from django import forms

class NameForm(forms.Form):
    your_name = forms.CharField(label='Your name', max_length=100)
```

```html
<!-- index.html -->
<form action="/your-name/" method="post">
    {% csrf_token %}
    {{ form }}
    <input type="submit" value="Submit">
</form>
```

```python
# views.py
from django.http import HttpResponseRedirect
from django.shortcuts import render

from .forms import NameForm

def get_name(request):
    # if this is a POST request we need to process the form data
    if request.method == 'POST':
        # create a form instance and populate it with data from the request:
        form = NameForm(request.POST)
        # check whether it's valid:
        if form.is_valid():
            # process the data in form.cleaned_data as required
            # ...
            # redirect to a new URL:
            return HttpResponseRedirect('/thanks/')

    # if a GET (or any other method) we'll create a blank form
    else:
        form = NameForm()

    return render(request, 'name.html', {'form': form})
```

Above codes are available in [gists][4]

### Bound and unbound form instances

- An unbound form has no data associated with it. When rendered to the user, it will be empty or will contain default values.
- A bound form has submitted data, and hence can be used to tell if that data is valid. If an invalid bound form is rendered, it can include inline error messages telling the user what data to correct.

### Access form values

```python
v = form.cleaned_data['my_form_field_name']
```

### Custom Validators

A validator is a callable object or function that takes a value and returns nothing if the value is valid or raises a `ValidationError` if not

```python
slug = forms.CharField(validators=[validators.validate_slug])
```

The `clean_<fieldname>()` method is called on a form subclass – where `<fieldname>` is replaced with the name of the form field attribute. This method does any cleaning that is specific to that particular attribute, unrelated to the type of field that it is. This method is not passed any parameters. You will need to look up the value of the field in `self.cleaned_data` and remember that it will be a Python object at this point, not the original string submitted in the form (it will be in cleaned_data because the general field `clean()` method, above, has already cleaned the data once).

```python
#forms.py
from django.forms import ModelForm, ValidationError  # Import ValidationError
from .models import Palindrome

class PalindromeForm(ModelForm):
    def clean_word(self):
        # Get the user submitted word from the cleaned_data dictionary
        data = self.cleaned_data["word"]

        # Check if the word is the same forward and backward
        if data != "".join(reversed(data)):
            # If not, raise an error
            raise ValidationError("The word is not a palindrome")

        # Return data even though it was not modified
        return data

    class Meta:
        model = Palindrome
        fields = ["word"]
```

## Authentication

### Authentication Settings

- AUTH_USER_MODEL Default: `auth.User`

- LOGIN_REDIRECT_URL Default: `/accounts/profile/`

- LOGIN_URL Default: `/accounts/login/`

- LOGOUT_REDIRECT_URL Default: `None`

### Authentication Views

```python
urlpatterns = [
    path("accounts/", include("django.contrib.auth.urls")),
]
```
This will include the following URL patterns:

```
accounts/login/ [name='login']
accounts/logout/ [name='logout']
accounts/password_change/ [name='password_change']
accounts/password_change/done/ [name='password_change_done']
accounts/password_reset/ [name='password_reset']
accounts/password_reset/done/ [name='password_reset_done']
accounts/reset/<uidb64>/<token>/ [name='password_reset_confirm']
accounts/reset/done/ [name='password_reset_complete']
```

If you want more control over your URLs, you can reference a specific view in your URLconf:

```python
urlpatterns = [
    path(
        "change-password/",
        auth_views.PasswordChangeView.as_view(template_name="change-password.html"),
    ),
]
```

### is_authenticated

```python
if request.user.is_authenticated:
    # Do something for authenticated users.
else:
    # Do something for anonymous users.
```

### How to log a user in

To log a user in, from a view, use `login()`. It takes an `HttpRequest` object and a `User` object. `login()` saves the user’s ID in the session, using Django’s session framework.

```python
from django.contrib.auth import authenticate, login

def my_view(request):
    username = request.POST["username"]
    password = request.POST["password"]
    user = authenticate(request, username=username, password=password)
    if user is not None:
        login(request, user)
        # Redirect to a success page.
        ...
    else:
        # Return an 'invalid login' error me
```

### How to log a user out

```python
def logout_view(request):
    logout(request)
    # Redirect to a success page.
```

### Decorators

- `login_required()`: If the user isn’t logged in, redirect to `settings.LOGIN_URL`, passing the current absolute path in the query string. Example: `@login_required(login_url='/accounts/login/')`
- `user_passes_test()`: Decorator for views that checks that the user passes the given test, redirecting to the log-in page if necessary. The test should be a callable that takes the user object and returns True if the user passes.
    ```python
    from django.contrib.auth.decorators import user_passes_test

    @user_passes_test(lambda user: user.is_staff)
    def staff_place(request):
        return HttpResponse("Employees must wash hands", content_type="text/plain")
    ```

## Django Admin

### Register Models

```python
from django.contrib import admin
from core.models import Blog

@admin.register(Blog)
class BlogAdmin(admin.ModelAdmin):
    pass
```

### Customizing Models

- `sortable_by`: Add sort option by clicking on the columns.
- `list_filter`
- `list_editable`
- `search_fields`
- `list_display`
- `readonly_fields`
- `inlines`
- `actions`

Adding the `ordering` attribute will default all queries on Person to be ordered by last_name then first_name.

```python
class Person(models.Model):
    # ...
    class Meta:
        ordering = ("last_name", "first_name")
    # ...
```

It can also reference a method in the admin.ModelAdmin itself:

```python
@admin.register(Person)
class PersonAdmin(admin.ModelAdmin):
    list_display = ("last_name", "first_name", "show_average")

    def show_average(self, obj):
        from django.db.models import Avg
        result = Grade.objects.filter(person=obj).aggregate(Avg("grade"))
        return result["grade__avg"]
```

`show_average()` is called once for each object displayed in the list.

### Adding Search Box

Anything the user types in the search box is used in an `OR` clause of the fields filtering the `QuerySet`. By default, each search parameter is surrounded by `%` signs. You can be more precise by specifying a `__` modifier on the search field.

```python
@admin.register(Person)
class PersonAdmin(admin.ModelAdmin):
    search_fields = ("last_name__startswith", )
```

### Inlines

```python
# OR use -> admin.StackedInline
class BookInline(admin.TabularInline):
    model = Book

class AuthorAdmin(admin.ModelAdmin):
    inlines = [BookInline]
```

### Actions

```python
class ArticleAdmin(admin.ModelAdmin):
    actions = ["make_published"]

    @admin.action(description="Mark selected stories as published")
    def make_published(self, request, queryset):
        queryset.update(status="p")
```

## `HttpRequest`

| Attribute | Description                | Examples                           |
| --------- | -------------------------- | ---------------------------------- |
| scheme    | URL scheme                 | "http" or "https"                  |
| path      | Path portion of the URL    | "/music/bands/"                    |
| method    | HTTP method used           | "GET" or "POST"                    |
| GET       | Query string parameters    | `<QueryDict: {'band_id':['123']}>` |
| POST      | Fields from an HTTP POST   | `<QueryDict: {'name':['Bob']}>`    |
| user      | Object describing the user |                                    |

## Http Method Decorators

The decorators in `django.views.decorators.http` can be used to restrict access to views based on the request method. These decorators will return a `django.http.HttpResponseNotAllowed` if the conditions are not met.

1. `require_http_methods(request_method_list)`
    ```python
    from django.views.decorators.http import require_http_methods

    @require_http_methods(["GET", "POST"])
    def my_view(request):
        # I can assume now that only GET or POST requests make it this far
        pass
    ```
2. `require_GET()`
3. `require_POST()`
4. `require_safe()`
   
    Decorator to require that a view only accepts the `GET` and `HEAD` methods. These methods are commonly considered “safe” because they should not have the significance of taking an action other than retrieving the requested resource.

## Generic Views

- CreateView: Needs absolute url of detail view
- ListView
- DetailView
- UpdateView
- DeleteView
- FormView

## Cursor Pagination

## URL Dispatcher

```python
from news import views

path("archive/", views.archive, name="news-archive")
```

you can use any of the following to reverse the URL:
```python
# using the named URL
reverse("news-archive")
```

## Flash Messages

### Add and use message

To add a message, call:

```python
from django.contrib import messages

messages.add_message(request, messages.INFO, "Hello world.")
```

```html
{% if messages %}
<ul class="messages">
    {% for message in messages %}
    <li{% if message.tags %} class="{{ message.tags }}"{% endif %}>{{ message }}</li>
    {% endfor %}
</ul>
{% endif %}
```

### Message_TAGS

Default:
```python
{
    messages.DEBUG: "debug",
    messages.INFO: "info",
    messages.SUCCESS: "success",
    messages.WARNING: "warning",
    messages.ERROR: "error",
}
```

Override:
```python
from django.contrib.messages import constants as message_constants

MESSAGE_TAGS = {message_constants.INFO: ""}
```

## Template Language

[Reference1](https://docs.djangoproject.com/en/4.2/ref/templates/language)
[Reference2](https://docs.djangoproject.com/en/4.2/ref/templates/builtins)

### Filter

- `{{ value|default:"hello" }}`
- lower
- truncateword
- filesizeformat

#### Tip

```html
<h1>Number of posts: {{ posts.count }}</h1>
<h1>Number of posts: {{ posts|length }}</h1>
```

Actually, in this very specific case, using the length template filter - which just calls len() - would be more efficient. That's because calling .count() on a queryset that hasn't been evaluated causes it to go back to the database to do a SELECT COUNT, whereas len() forces the queryset to be evaluated.

### Custom Template Tags and Filters

#### First Step

```
polls/
    __init__.py
    models.py
    templatetags/
        __init__.py
        poll_extras.py
    views.py
```

#### Load Modules

In every templates that we want to use our custom tags:

```html
{% load poll_extras %}
```

#### Register

To be a valid tag library, the module must contain a module-level variable named `register` that is a `template.Library` instance, in which all the tags and filters are registered. So, near the top of your module, put the following:

```python
from django import template

register = template.Library()
```

Custom filters are Python functions that take one or two arguments:

- The value of the variable (input) – not necessarily a string.
- The value of the argument – this can have a default value, or be left out altogether.
For example, in the filter `{{ var|foo:"bar" }}`, the filter foo would be passed the variable `var` and the argument `bar`.

Once you’ve written your filter definition, you need to register it with your Library instance, to make it available to Django’s template language:
```python
register.filter("cut", cut)
register.filter("lower", lower)
```

The `Library.filter()` method takes two arguments:

- The name of the filter – a string.
- The compilation function – a Python function (not the name of the function as a string).

You can use `register.filter()` as a decorator instead:

```python
@register.filter(name="cut")
def cut(value, arg):
    return value.replace(arg, "")


@register.filter
def lower(value):
    return value.lower()
```
If you leave off the name argument, as in the second example above, Django will use the function’s name as the filter name.

Finally, `register.filter()` also accepts three keyword arguments, `is_safe`, `needs_autoescape`, and `expects_localtime`. 

### url

```html
{% url 'some-url-name' arg1=v1 arg2=v2 %}
```

### Context Processors

```python
def site_settings(request):
    return {'site_name': 'My awesome site', 'site_creation_date': '12/12/12'}

# Add this to the context_processors list
'yourapp.context_processors.site_settings'  # <-- our custom context processor
```

## Models

### Choices

The first element in each tuple is the value that will be stored in the database. The second element is displayed by the field’s form widget.

```python
YEAR_IN_SCHOOL_CHOICES = [
    ("FR", "Freshman"),
    ("SO", "Sophomore"),
    ("JR", "Junior"),
    ("SR", "Senior"),
    ("GR", "Graduate"),
]
```

### Field Type Parameters

- `editable`: Default `True`

### Deep Copy

Just change the primary key of your object and run save().

```python
obj = Foo.objects.get(pk=<some_existing_pk>)
obj.pk = None
obj.save()
```

### Manager

#### Manager Names

Using this example model, `Person.objects` will generate an `AttributeError` exception, but `Person.people.all()` will provide a list of all `Person` objects.

```python
from django.db import models


class Person(models.Model):
    # ...
    people = models.Manager()
```

#### Extra Manager

Adding extra Manager methods is the preferred way to add **table-level** functionality to your models. (For **row-level** functionality – i.e., functions that act on a single instance of a model object – use Model methods, not custom Manager methods.)

For example, this custom Manager adds a method with_counts():
```python
from django.db import models
from django.db.models.functions import Coalesce


class PollManager(models.Manager):
    def with_counts(self):
        return self.annotate(num_responses=Coalesce(models.Count("response"), 0))


class OpinionPoll(models.Model):
    question = models.CharField(max_length=200)
    objects = PollManager()
```

#### Modifying a manager’s initial `QuerySet`

You can override a Manager’s base `QuerySet` by overriding the `Manager.get_queryset()` method.
```python
class DahlBookManager(models.Manager):
    def get_queryset(self):
        return super().get_queryset().filter(author="Roald Dahl")
```

#### Default managers

Django interprets the first Manager defined in a class as the **default** Manager. You can specify a custom default manager using `Meta.default_manager_name`.

### Field lookups

1. `gt`
2. `gte`
3. `lt`
4. `lte`
5. `startswith`: abc%
6. `istartswith`: Case-insensitive version of `startswith`
7. `endswith`: %abc
8. `contains`: %abc%
9. `in`: WHERE id IN (1, 3, 4)

### Aggregate

```python
Person.objects.aggregate(
    average=Avg("age"),
    max_age=Max("age"),
    min_age=Min("age"),
    sum_age=Sum("age"),
    count=Count("age"),
    )
```

### Relations

#### OneToMany

By default, this Manager is named `FOO_set`, where `FOO` is the source model name, **lowercased**. This Manager returns `QuerySets`.

You can **override** the `FOO_set` name by setting the `related_name` parameter in the `ForeignKey` definition.
```python
>>> b = Blog.objects.get(id=1)
>>> b.entries.all()  # Returns all Entry objects related to Blog.

# b.entries is a Manager that returns QuerySets.
>>> b.entries.filter(headline__contains="Lennon")
>>> b.entries.count()
```

#### ManyToMany

```python
from django.db import models


class Publication(models.Model):
    title = models.CharField(max_length=30)

    class Meta:
        ordering = ["title"]

    def __str__(self):
        return self.title


class Article(models.Model):
    headline = models.CharField(max_length=100)
    publications = models.ManyToManyField(Publication)

    class Meta:
        ordering = ["headline"]

    def __str__(self):
        return self.headline
```

Add or Create:
```python
>>> a2.publications.add(p1, p2)
>>> a2.publications.create(title="Science News")
```

#### OneToOne

```python
class EntryDetail(models.Model):
    entry = models.OneToOneField(Entry, on_delete=models.CASCADE)
    details = models.TextField()

ed = EntryDetail.objects.get(id=2)
ed.entry  # Returns the related Entry object.
e = Entry.objects.get(id=2)
e.entrydetail  # returns the related EntryDetail object (Reverse)
```

If **no** object has been assigned to this relationship, Django will **raise** a `DoesNotExist` exception.

## Django Rest Framework

### Serializer

#### `Serializer`

Custom `create`, `update` and validation functions.

```python
class SnippetSerializer(serializers.Serializer):
    id = serializers.IntegerField(read_only=True)
    title = serializers.CharField(required=False, allow_blank=True, max_length=100)
    code = serializers.CharField(style={'base_template': 'textarea.html'})
    linenos = serializers.BooleanField(required=False)

    def create(self, validated_data):
        """
        Create and return a new `Snippet` instance, given the validated data.
        """
        return Snippet.objects.create(**validated_data)

    def update(self, instance, validated_data):
        """
        Update and return an existing `Snippet` instance, given the validated data.
        """
        instance.title = validated_data.get('title', instance.title)
        instance.linenos = validated_data.get('linenos', instance.linenos)
        instance.language = validated_data.get('language', instance.language)
        instance.save()
        return instance
```

#### `ModelSerializer`

Very looks like model forms:

```python
rom rest_framework import serializers
from .models import Product

class ProductSerializer(serializers.ModelSerializer):
    class Meta:
        model = Product
        fields = '__all__'
        # Other attributes
        exclude = ['users']
        depth = 1  # Specifying nested serialization
```

#### Custom Method Field

```python
class PostSerializer(serializers.ModelSerializer):
    owner = serializers.SerializerMethodField()
    class Meta:
        model = Post
        fields = ('title', 'body', 'created', 'owner')

    def get_owner(self, obj):
        return obj.owner.username
```

### Function Based Views

```python
from .models import Product
from .serializers import ProductSerializer
from rest_framework.response import Response
from rest_framework.decorators import api_view


@api_view(['GET'])
def my_api(request):
    instance = Product.objects.all()
    data = ProductSerializer(instance, many=True).data
    return Response(data)
```

### Class Based Views

One of the key benefits of class-based views is the way they allow you to compose bits of reusable behavior.

#### `APIView`

`APIView` classes are different from regular `View` classes in the following ways:

- Requests passed to the handler methods will be REST framework's `Request` instances, not Django's `HttpRequest` instances.
- Handler methods may return REST framework's `Response`, instead of Django's `HttpResponse`. The view will manage content negotiation and setting the correct renderer on the response.
- Any APIException exceptions will be caught and mediated into appropriate responses.
- Incoming requests will be authenticated and appropriate permission and/or throttle checks will be run before dispatching the request to the handler method.

#### `RetrieveAPIView`

Used for **read-only** endpoints to represent a **single** model instance. Provides a `get` method handler.
```python
from rest_framework import generics
from .models import Product
from .serializers import ProductSerializer

class MyAPIView(generics.RetrieveAPIView):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
```
```python
path('<int:pk>', views.MyAPIView.as_view())
```

#### `CreateAPIView`
Used for **create-only** endpoints. Provides a `post` method handler.

You can override the save method and customize what data is saved. For example:
```python
def perform_create(self, serializer):
    serializer.save(owner=self.request.user)
```
The owner content will override.

#### `ListAPIView`
Used for **read-only** endpoints to represent a collection of model instances. Provides a `get` method handler.

#### `ListCreateAPIView`
Used for **read-write** endpoints to represent a collection of model instances. Provides `get` and `post` method handlers.

### Serializer Relations

#### `SlugRelatedField`

- `many` - If applied to a to-many relationship, you should set this argument to `True`.
- `queryset` - The queryset used for model instance lookups when validating the field input. Relationships must either set a `queryset` explicitly, or set `read_only=True`.

```python
class AlbumSerializer(serializers.ModelSerializer):
    tracks = serializers.SlugRelatedField(
        many=True,
        read_only=True,
        slug_field='title'
     )

    class Meta:
        model = Album
        fields = ['album_name', 'artist', 'tracks']
```

#### `HyperlinkedRelatedField`

```python
class AlbumSerializer(serializers.ModelSerializer):
    tracks = serializers.HyperlinkedRelatedField(
        many=True,
        read_only=True,
        view_name='track-detail'
    )

    class Meta:
        model = Album
        fields = ['album_name', 'artist', 'tracks']
```

### Requests
1. `.data`: returns the parsed content of the request body.


[1]: https://pypi.org/project/django-cors-headers/
[2]: https://docs.djangoproject.com/en/4.1/intro/tutorial04/#write-a-minimal-form
[3]: https://docs.djangoproject.com/en/4.1/topics/forms/#building-a-form-in-django
[4]: https://gist.github.com/ILoveBacteria/55b374670d65ceeb38e0e7c789bcf6af
[5]: https://gist.github.com/ILoveBacteria/b343c8ad8b9fa162db2f7554b1fa0fd1
{% endraw %}
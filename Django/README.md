# Django Cheatsheet

## Table Of Contents

- [Django Cheatsheet](#django-cheatsheet)
  - [Table Of Contents](#table-of-contents)
  - [Handwrite Notes](#handwrite-notes)
  - [Forms](#forms)
    - [How to write a minimal form in Django](#how-to-write-a-minimal-form-in-django)
    - [A simple form in Django](#a-simple-form-in-django)
    - [Bound and unbound form instances](#bound-and-unbound-form-instances)

## Handwrite Notes

1- `CORS`: When site *A* wants to access content from another site *B*, it is called a Cross-Origin request. As it is disabled for security reasons, *B* sends an `Access-Control-Allow-Origin` header in the response. By default, a domain is not allowed to access an API hosted on another domain. The `Origin` header should be in request.

[django-cors-headers][1] is a Django application for handling the server headers required for Cross-Origin Resource Sharing (CORS).

## Forms

### How to write a minimal form in Django

[Read the doc][2]

```html
<!-- polls/templates/polls/detail.html -->
{% raw %}
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
{% endraw %}
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
{% raw %}
<form action="/your-name/" method="post">
    {% csrf_token %}
    {{ form }}
    <input type="submit" value="Submit">
</form>
{% endraw %}
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


[1]: https://pypi.org/project/django-cors-headers/
[2]: https://docs.djangoproject.com/en/4.1/intro/tutorial04/#write-a-minimal-form
[3]: https://docs.djangoproject.com/en/4.1/topics/forms/#building-a-form-in-django
[4]: https://gist.github.com/ILoveBacteria/55b374670d65ceeb38e0e7c789bcf6af
[5]: https://gist.github.com/ILoveBacteria/b343c8ad8b9fa162db2f7554b1fa0fd1
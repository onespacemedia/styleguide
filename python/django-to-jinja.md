Originally located at onespacemedia/django2jinja

# Upgrading from Django to Jinja

You can learn quite a lot of the differences just by reading the pull requests.

* [Project template](https://github.com/onespacemedia/project-template/pull/47)
* [CMS](https://github.com/onespacemedia/cms/pull/111)

There you can compare the diff to see the Django way and then the Jinja way of how things are done.

## Notable changes

## `{%` => `{{`
One of the main changes from Django templates to Jinja templates are that all of the old template tags are now just global functions. There is no more `{% load %}` to bring in what templatetags you require, they are all just on `context`.

#### What this means.
The most notable difference this means is instead of calling templatetags we used to use before like `{% thumbnail %}` we will instead use `{{ thumbnail() }}`

#### Changes that were mode in the transition.
Since all of the templatetags now go into `context`, it means we can get a few name clashes, notably things like `meta_description`

Where we had `{% meta_description %}` before we ended up getting name clashes, to explain:

```python
    if description is None:
        description = context.get("meta_description")

    if description is None:
        request = context["request"]
        page = request.pages.current

        if page:
            description = page.meta_description

    return escape(description or "")
``` 

Here you can see the templatetag is looking in `context` for `meta_description` which with the way Jinja handles global methods is now this templatetag. So it would return the reference to this function instead of the `meta_description` we would normally get from our object like in Django templates.

So the workaround that was made for this was to use `get_` to prefix all templatetags that return a string, list, object, etc. to prevent nameclashes and give a bit more meaning to what the templatetag actually does.

So `meta_description` now becomes `get_meta_description`. The same goes for tags like `title` is now `get_title`.

## `()`
Jinja itself aims to be more Pythonic than the Django template engine is so by doing this it introduces a lot of the structures we expect from Python. For example:

`{{ object.item_set.all }}` now becomes `{{ object.item_set.all() }}`

All function references need the `()` unlike in Django templates where you couldn't really tell if something was a function or just a reference.

This also applies for filters. So when using filters like `slice` it would be: `{{ list|slice(1) }}` instead of `{{ list|slice:10 }}` 

## How to register template tags
[Here](http://niwinz.github.io/django-jinja/latest/#_registring_filters_in_a_django_way) is a very good guide to show the differences with registering filters / templatetags the "Django" way but with Jinja.

The main takeaways are:

---

#### Starting templatetags
**Old**
```python
from django import template
  		  
register = template.Library()
```

**New**
```python
from django_jinja import library
```

---

#### Access to context

**Old**
```python
# If using a filter
@register.filter(takes_context=True)
def filter(context):
    pass

# If using a function
@register.simple_tag(takes_context=True)
def function(context):
    pass
```

**New**
```python
import jinja2

# If using a filter
@jinja2.contextfilter
def filter(context):
    pass 

# If using a function
@jinja2.contextfunction
def function(context):
    pass
```

---

#### Register a filter

**Old**
```python
@register.filter
def filter():
    pass
```

**New**
```python
@library.filter
def filter():
    pass
```

---

#### Register a function

**Old**
```python
@register.simple_tag
def function():
    pass
```

**New**
```python
@library.global_function
def function():
    pass
```

---

#### Register a inclusion tag

**Old**
```python
@register.inclusion_tag('path/to/template.html')
def inclusion_tag(thing):
    return {
        'thing': thing
    }
```

**New**
```python
@library.global_function
@library.render_with('path/to/template.html')
def inclusion_tag(thing):
    return {
        'thing': thing
    }
```

---

#### Register a assignment_tags

**Old**
```python
@register.assignment_tag
def assignment_tag():
    pass
```

**New**
```python
@library.global_function
def assignment_tag():
    pass
```

You'll notice this is the exact same as "Register a function" that's because the way you set variables in Jinja templates is quite different to Django. You can see how it's done [Setting Variables](#setting-variables)

## General Jinja takeaways

#### Getting `x` number of items from a list
Same as how you would in Python! `{{ list[:10] }}`

#### Empty for loops
Same as how you would in Python!

```jinja
{% for item in list %}
    {{ item }}
{% else %}
    No items :(
{% endfor %}
```

#### For loop iteration
This is slightly different to Django templates. In Django you would use `{{ forloop.counter }}`. In Jinja it is `{{ loop.index }}`

You can see the changes more in depth [here](http://jinja.pocoo.org/docs/dev/templates/#for)

#### Block supers
In Django you use `{{ block.super }}`. Jinja is just `{{ super() }}`

#### Setting variables
In Django you tended to wrap things in `{% with %}` or use assignment_tags. In Jinja it's just `{% set item = 'string' %}`. You can also use the `global_function`'s with them as well `{% set thing = global_function() %}`

## Things i'd advise you to look at:
* [You can now use `{% block %}` inside conditionals](http://jinja.pocoo.org/docs/dev/templates/#block-nesting-and-scope)
* [You can use variables with `{% extends %}`](http://jinja.pocoo.org/docs/dev/templates/#template-objects)
* You can use string formatting! A nice example is:

    `{% include 'folder/{}/{}.svg'.format(folder, file) %}`.

    Tons better than
    
    `{% include 'folder/'|add:folder|add:'/'|add:file|add:'.svg' %}`
* [Macros are a great way to keep things modular](http://jinja.pocoo.org/docs/dev/templates/#macros)
* If using macros [Call](http://jinja.pocoo.org/docs/dev/templates/#call) can be amazing. They allow you to use something like `<slot>` in Vue. So you can create some base macros then include dynamic content when using them on the fly.
* [Import](http://jinja.pocoo.org/docs/dev/templates/#import) is to be advised when using macros for clean code
* [Builtin filters](http://jinja.pocoo.org/docs/dev/templates/#list-of-builtin-filters)
* [Builtin tests](http://jinja.pocoo.org/docs/dev/templates/#list-of-builtin-tests)
* [Builtin functions](http://jinja.pocoo.org/docs/dev/templates/#list-of-global-functions)
* [Builtin extensions](http://jinja.pocoo.org/docs/dev/templates/#extensions)

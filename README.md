# Introduction

As OSM moves forward and grows as a reputable design agency, it’s imperative to make sure as a team we are consistently producing work that makes us unique and stand out from the crowd. We have written this styleguide for your reference as a member of the development team to outline what is expected and how to achieve the highest possible standard as project size and frequency increase.

As the projects we start to work on become larger in scale and longer-running, it is very important that we work in a unified way and with a ever growing team with different specialities and abilities. It is necessary to keep the process streamlined in order to:

- Keep everything maintainable.
- Keep code transparent, sane, and readable.
- Keep everything scalable.

# Editing this style guide

This is a [docsify](https://docsify.js.org/#/quickstart) document. To start:
- Run `npm i docsify-cli -g`
- Clone the `styleguide` repository into your Workspace folder
- Run `docsify init ./styleguide`, then `docsify serve styleguide`

A web server will be spawned on port 3000. As you edit the README.md it will automatically refresh on the front-end.

# Consistency

To quote PEP8,

> A style guide is about consistency. Consistency with this style guide is important. Consistency within a project is more important. Consistency within one module or function is the most important.

As the current set of rules evolved over time, many older projects will not be conformant with them. If you find yourself revisiting an older project, it is more important to stay consistent with the rest of that project than it is to enforce these rules.

There is one exception to this: *Do not escalate CSS nesting specifity wars.* This harms maintainability in the future. If you possibly can, create a new, non-conflicting class for any new components that you add.

# CSS guidelines

Writing clean code is something that is good to look at, and although we use stylelist and now with the introduction of pre-commits we can rely on [Stylelint](https://stylelint.io/) to slap our wrists when we write something poorly or in the incorrect way, it is still good to understand as a team what the standard syntax should be when writing good sexy CSS, this includes:

- 80 character wide columns, to aid readability
- Multi-line CSS
- Meaningful use of whitespace
- Grouping properties

## General formatting rules

- Indent blocks with two spaces
- A space before the opening curly brace ({)
- Properties and values on the same line
- Always have a space after the colon (:)
- Opening brace on the same line as our last selector ({);
- Declarations on a new line including after the first opening curly brace ({);
- Closing curly brace on a new line (});
- Trailing semicolon at the end of each declaration (;).
- Always end a CSS file with an empty line.
- Properties should always be lowercase.
- Shorten colour values - use #fff rather than #ffffff, for example.

Example follows:

```css
  [selector] ,[related selector],
  [unrelated selector] {
    [property]: [value];
    [<--declaration-->]
  }

  .nsp-Component_ChildNode, .nsp-Component_ChildNode-childnode,
  .nsp-ComponentUnrelated_ChildNode {
    display: block;
  }
```

Exceptions:

- Rulesets that carry only one declaration each;
- Conform to the one-reason-to-change-per-line rule;
- Sharing enough similarities that they don’t need to be read as thoroughly as other rulesets - there is more benefit in being able to scan their selectors.

Example follow:

 ```css
 .nsp-Component_ChildNode {
   display: inline-block;
   width: 16px;
   height: 16px;

   background-image: url(/img/photo.jpg);
 }

 .nsp-Component_ChildNode-home     { background-position:   0     0  ; }
 .nsp-Component_ChildNode-person   { background-position: -16px   0  ; }
 .nsp-Component_ChildNode-files    { background-position:   0   -16px; }
 .nsp-Component_ChildNode-settings { background-position: -16px -16px; }
 ```

## Selectors

- Related selectors should be on the same line, unrelated selectors should be on a new line
- Never use IDs as selectors
- Do not use duplicate selectors.

## Stylelint and grouping properties

[Stylelint](https://stylelint.io/) is utilised to try and catch errors when code is pushed. Take a second to understand the [configuration](https://github.com/onespacemedia/project-template/blob/develop/%7B%7Bcookiecutter.repo_name%7D%7D/package.json#L119) we use.

Properties are grouped together so they’re easily readable with a empty line between, in this order:

 ```css
 .nsp-Component_ChildNode {
    content properties;

    position properties;

    flex properties;

    display margin and height properties;

    font and text properties;

    background border and color properties;

    animation and transition properties;
  }
 ```

We recommend read the documentation above about linting as we also use it for Javascript as well. If you are familiar with linting then take a moment to run through the order above.

## Commenting

With the increasing size of Onespacemedia projects it is increasingly important to comment your CSS. Trying to understand your own code is difficult enough. Someone inheriting the project will find it more difficult than that.

There should always be a heading comment at the top of a .css file that includes the component name and the namespace selector:

```css
/*
|--------------------------------------------------------------------------
| Component
|--------------------------------------------------------------------------
| @namespace: nsp-
|
*/
```

Then comments after should be grouped and order in the same way the HTML is ordered, keeping everything together in small sections in the file:

```css
/*
|--------------------------------------------------------------------------
| Header
|--------------------------------------------------------------------------
*/

/*
|--------------------------------------------------------------------------
| Body
|--------------------------------------------------------------------------
*/

/*
|--------------------------------------------------------------------------
| Footer
|--------------------------------------------------------------------------
*/
```

If you use "magic" numbers anywhere in your

# HTML

## General HTML rules

Tag names, attributes, and attribute values should be in lower-case where possible.

Attribute values should be quoted in "double quotes"

Where an element starts on a new line to a parent element, it should be indented by two spaces. Elements that are rendered as blocks (whether they are block-level elements in the specification or made to render that way in CSS) should usually start on a new line. Avoid doing this if it will cause an ungrammatical space in text, though.

For self-contained tags that do not have textual content, omit closing tags where the HTML5 specification permits it. Avoid the XML form. This is fine:

```html
<link rel="stylesheet" href="/static/css/style.css">
```

Elements that do have textual content wherein the specification permits omitting the end tag, should not have their end tag omitted. `<li>` and `<p>`, for example, may have their end tags omitted in the specification, but it's easier to spot the scope of an element with the explicit closing tag in place:

```html
<ul>
  <li>List item!</li>
</ul>

<p>A paragraph of text!</p>

<div>While this div, being a block-level element, would implicitly have closed the above P element if its end tag was omitted, the scope of the above element is more obvious with the explicit closing tag.</div>
```

An empty line between two HTML elements at the same indentation level is optional. Use your judgement.</p>

Use the short form for attributes like `checked` on `<input>` and `selected` on `<option>`:

```html
<select>
  <option selected value="1">I'm the default option</option>
  <option value="2">I'm not</option>
</select>
```

## Jinja and Django template styles

### Spacing

Always put a space between the delimiters of a 'print' or of a template tag, and its name. This makes it much more readable:

```
{# Prints should have spaces inside the curlies. #}
<h1>Hello, {{ name }}!</h1>

<ul>
  {# Tags and statements should have a space before and after the percent. #}
  {% for animal in animals %}
    <li>{{ animal }}</li>
  {% endfor %}
</ul>
```

### Indentation

Indentation should be with two spaces.

You should not have more than one consecutive empty line.

A statement block, which is to say any tag that has a corresponding `end` tag, should have its contents indented and on a new line if it contains any substantial amount of content.

It's fine to use a one liner to override a short string, as we quite often want to do with `{% block %}`:

```
{% extends 'base.html' %}

{% block title %}I'm a mere short title{% endblock %}

{% block body_class %}body-IAmAcceptableToo{% endblock %}

{% block content %}
  <p>As I contain a block-level HTML element and more than a trivial quantity of text, I start a new line and a new indentation level.</p>
{% endblock %}
````

There are times you should feel free to not do this to avoid unnecessary whitespace. For example, we can't put the `if` on its own line here because it would cause an ungrammatical space before the comma:

```
Filed under {% for category in categories %}{{ category }}{% if not forloop.last %}, {% endif %}{% endfor %}
```

Block-level tags, which is to say those with a corresponding `end` tag, should usually have an empty line between them and any other items at the same indentation level.

```
{% block content %}
  <h1>{{ person.name }}</h1>

  {% if person.nickname %}
    <p>{{ person.nickname }}</p>
  {% endif %}

  {% with person.pet_set.all() as animals %}
    {% if animals %} {# No empty line as it starts a new indentation level. #}
      <h2>Pets</h2>

      <ul>
        {% for animal in animals %}
          <li>{{ animal }}</li>
        {% endfor %}
      </ul>
    {% endif %}
  {% endwith %}
{% endblock %}
```

Blocks should not be padded with an empty line. Do this:

```
{% if some_condition %}
  <p>This is good!</p>
{% endif %}
```

Rather than this:

```
{% if some_condition %}

  <p>Don't do this!</p>

{% endif %}
```

### Jinja2 `set` and `with`

`{% set %}` and `{% with %}` assignments should have a space before and after the equals, just as one would in the generally-accepted Python style:

```
{% set articles = get_news_articles() %}
```

As well as being consistent with Python assignments, it means that constructs like this don't look weird:

```
{% set value1, value = get_some_tuple() %}
```

But as with generally accepted Python practice, keyword arguments passed to a function should not:

```
{% set articles = get_news_articles(count=5) %}
```

Function parameters should have a space after each comma, but never before:

```
{% set articles = get_news_articles(featured=True, count=5) %}
```

### Jinja macros

If your macro takes a lot of arguments, consider whether you can consolidate those which content into a single one.

An example might be a macro called like this, to render a card for a news article:

```
{{ cards.card(
  article.title,
  article.summary,
  article.image,
  article.get_absolute_url(),
  modifier_parameter=True
) }}
```

In this case, consider whether you could build an `as_card` method on your article model...

```
def as_card(self):
  return {
    'title': self.title,
    'summary': self.summary,
    'image': self.image,
    'url': self.get_absolute_url(),
  }
```

...and modify your `card` macro to take a single first parameter for the card's content:


```
{{ cards.card(article.as_card(), modifier_parameter) }}
```

This has a bonus of moving excessive logic out of templates into Python, where anything but the most simple logic is much cleaner.

## SVGs

Where possible, you should usually prefer inlining an SVG in HTML rather than including it with `<img>` or applying it with a `background-image`. This permits targeting SVG paths in CSS for things like hover effects an animations.

With a few rare exceptions, SVGs should contain only `path` elements, and you should be able to target the visible parts of the path with `fill` alone (rather than strokes). This gives a single target for CSS, and a single CSS property for styling it. Ask a designer to convert the file for you if you don't know how to convert it yourself.

When inlining SVGs, be careful with SVGs exported by Adobe Illustrator. Apart from being vastly bigger than they need to be, they almost invariably end up with SVGs with identical IDs, identical class names, and inline `<style>` blocks. This often causes an SVG to apply its styles to paths in a different SVG earlier in the document.

[SVGOMG](https://jakearchibald.github.io/svgomg/) is a useful tool for cleaning up SVG files. Alternatively, the manual cleanup guide is this:

1. Remove the `<?xml` declaration.
2. Remove all attributes from the root `svg` element except `viewBox`.
3. Remove the entire `<defs>` block if one is present.
4. Remove any explicit `fill=` attributes on paths.
5. Replace class names with ones conforming to our naming conventions (or for SVGs with a single colour, like most icons, remove class names altogether)
6. Use CSS (within the standard frontend build system, not in the SVG itself) to style the SVGs, if necessary.

# Python style guidelines

If in doubt, [PEP 8](https://www.python.org/dev/peps/pep-0008/).

## String quotes

Prefer 'single quotes' over "double quotes".

If a Python file consistently uses double quotes, then stay consistent with that file by using double quotes for new code within it.

If a file uses a mixture of single and double quotes, use single quotes for new code, but feel free to convert it to use single quotes consistently.

For quoting text that contains an apostrophe, it's fine to use either double quotes or single quotes with backslash escapes inside. This is fine:

```python
have_bugs = models.BooleanField(
    default=False,
    help_text="Don't check this!"
)
```

But this is too:

```python
explode = models.BooleanField(
    default=False,
    help_text='Don\'t check this either!'
)
```

## Line lengths

Try to keep lines to a maximum of 79 characters.

This is a suggestion, not a rule. If limiting the line length would result in uglier code, then ignore it.

## Multi-line constructs

The last closing parenthesis, bracket, or brace should line up with the indentation level of the first line of the construct. For example, this is good:

```python
ANIMAL_CHOICES = [
    (1, 'Cat'),
    (2, 'Dog'),
    (3, 'Horse'),
]
```

Don't do this:

```python
ANIMAL_CHOICES = [
    (1, 'Cat'),
    (2, 'Dog'),
    (3, 'Horse'),
    ]
```

Multi-line array, dictionary, and tuples should have a dangling comma on the last item. Multi-line function calls may optionally have them too, though this isn't always possible (it's a syntax error if your last argument is of the form `**kwargs`).

For function calls (including class instantiation), if your declaration goes across more than one line, then the first argument should be on a new line:

```python
objects = queryset.filter(
    is_online=True,
    page=self.request.pages.current,
)
```

## Import ordering

Use [isort's default ordering](http://timothycrosley.github.io/isort/).

## Django model style

This is the general form of model fields:

```python
class StyleGuide(models.Model):

    title = models.CharField(
        max_length=100,
        blank=True,
        null=True,
    )
```

There should be a new line before the first argument to a field's constructor, and each argument goes on a new line. This makes diffs easier to read when fields are altered.

There should be a dangling comma after the last argument. Again, this makes diffs easier to read (because adding an argument to the end does not affect neighbouring lines).

String literals that are displayed to users, such as the `help_text` for a model field, should not be broken across lines, regardless of their length. Don't do something like this:

```python
title = models.CharField(
    max_length=100,
    help_text=(
        'This is helpful text, but there is a problem because it spans more '
        'than one line'
    ),
)
```

This makes it harder to search for distinctive strings; a `grep` for "more than one line" will not return the file in which the above field lives. Do this instead:

```python
title = models.CharField(
    max_length=100,
    help_text='This is helpful text, and because it is all on one line you can grep for any part of this string.',
)
```

All models must define a `__str__` method in Python 3 projects, or `__unicode__` for Python 2.

### About various field types

Don't use a very large `max_length` for `CharField`. Generally, unless there are good technical reasons to do otherwise, don't supply a `max_length` of more than 300. If you need more characters than that, use a `TextField`.

A `ForeignKey`, `ManyToManyField`, etc, should usually use the string form for specifying the model:

```python
page = models.ForeignKey(
    'pages.Page',
)

articles = models.ManyToManyField(
    'news.Article',
)
```

This avoids problems with recursive imports across apps.

### Model ordering

Order model attributes thusly:

1. Static class variables other than database fields (e.g., the common `urlconf = 'projectname.apps.app_name.urls'` for CMS `ContentBase` derivatives)
2. Database fields
3. The `Meta` class
4. Double-underscore-prefixed special methods, e.g. `__unicode__`/`__str__`
5. Overrides of standard model methods (e.g. `save()`), in alphabetical order
6. All other model methods, in alphabetical order

### Model, field, and app naming

Always ensure that the `verbose_name_plural` for a model is grammatical. Django's default behaviour will pluralise 'Category' as 'Categorys', for example.

Where appropriate, supply an appropriate `verbose_name` to ensure that capitalisation is correct for both model names and field names, including appropriate capitalisation for brand names. A model called `Faq` should have a `verbose_name` of 'FAQ'. A 'linkedin_url' field should have a `verbose_name` of 'LinkedIn URL', to avoid the default admin rendering of 'Linkedin url'.

Similarly, where required, ensure that app names have an appropriate `AppConfig` to ensure that app names render grammatically in the admin.

## Django views

Always use [class-based views](https://docs.djangoproject.com/en/1.11/topics/class-based-views/). We have a nice [tutorial](http://www.onespacemedia.com/news/2014/feb/5/getting-started-generic-class-based-views-django/).

View classes should have their attributes in this order:

1. Static class attributes
2. Double-underscore methods (`__init__` and friends)
3. Methods inherited from Django generic views, in alphabetical order
4. Custom methods, in alphabetical order

# TODO:

## CSS
* [ ] Margin vs Padding on sections
* [ ] `& + &` vs all items having it and explicitly defining the `{first,last}-child`
* [ ] `Why --Grid_Container` can't take the `percentage(x, x)` as well
* [ ] How to do cards height (iOS issues with height: 100%)
* [ ] How to do overflow on body with iOS
* [ ] Enduring CSS - Understanding the problem
* [ ] Reusability - Naming
* [ ] Specifity
* [ ] Definition Properties
* [ ] Vertical Rythm

## HTML
* [ ] Sections don't all have to be a unique class
* [ ] Explain the `_Header` `_Body` style way of doing things and the benefits
* [ ] Explain the standard classes `_Title`, `_Text`, etc.

## Etc
* [ ] Browser testing
* [ ] Responsive development
* [ ] CMS / Admin
* [ ] User considerations
* [ ] SEO

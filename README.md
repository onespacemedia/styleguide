# Introduction

As OSM moves forward and grows as a reputable design agency, it’s imperative to make sure as a team we are consistently producing work that makes us unique and stand out from the crowd. This styleguide will help developers achieve consistency and the highest standard of work as project size and frequency increase.

As the projects we work on become larger in scale and longer-running, it is very important that we work in a unified way and with a ever growing team with different specialities and abilities. It is necessary to keep the process streamlined in order to:

- Keep everything maintainable.
- Keep code transparent, sane, and readable.
- Keep everything scalable.

# Editing this style guide

This is a [docsify](https://docsify.js.org/#/quickstart) document. To start:
- Run `npm i docsify-cli -g`
- Clone the `styleguide` repository into your Workspace folder
- Run `docsify init ./styleguide`, then `docsify serve styleguide`

A web server will be spawned on port 3000. As you edit the README.md it will automatically refresh on the front-end.

# Guidelines for any language

## Consistency

To quote [PEP 8](https://www.python.org/dev/peps/pep-0008/),

> A style guide is about consistency. Consistency with this style guide is important. Consistency within a project is more important. Consistency within one module or function is the most important.

Because our rules evolved over time, many older projects will not conform to them. If you find yourself revisiting an older project, it is more important to stay consistent with the rest of that project than it is to enforce these rules.

There is one exception to this: *Do not escalate CSS nesting specifity wars.* This harms maintainability in the future. If you possibly can, create a new, non-conflicting class for any new components.

## Commenting

Comment anything that someone fresh to the project might not understand immediately. Don't write comments for the sake of writing comments. There is no need for this:

```python
# Initialise the counter variable
counter = 0
# Loop over the items.
for item in some_list:
    # Add 1 to counter
    counter = counter + 1
```

The purpose of comments is not to describe what code does. They are to help other developers understand your code. This is an important distinction.

Anything particularly clever should have a comment (though you may want to see if you can write it in a less clever way). Anything done to work around strange bugs should be commented. Anything which would cause another developer to say "why did they do this instead of X", for values of X being some way that would have been the first, should be commented.

## Variable names

Keystrokes are abundant. While not enforced, your variable names should be at least a single English (or technical) word. A positive Python example of what you should do:

```
class ThingyListView(ListView):
    def get_queryset(self):
        queryset = super().get_queryset()
        queryset = queryset.filter(page__page=self.request.pages.current)
        return queryset

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context['categories'] = Category.objects.all()
        return context
```

It would be tempting to shorten `context` and `queryset` to `ctx` and `qs`. This would make your code harder to reason about, because someone has to remember what you meant by `ctx` and `qs` later on.

Variable names should be in accord with the most common community-accepted standard for whatever language you are using. For example, we use function_names_with_underscores in Python, because this is what the Python community has largely agreed to use. We use camelCase in Javascript, because this is what the language's standard library and the browser DOM uses.

# CSS guidelines

Most of these rules are enforced by [Stylelint](https://stylelint.io/), both in pre-commit hooks and in CI.

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

## Class naming conventions

We largely follow the [Enduring CSS](http://ecss.io/) convention, which is very similar in philosophy (differing mainly in syntactical details) to the [Block, Element, Modifier](http://getbem.com/naming/) convention. Specifically, *namespace, block, element, modifier*. Class names will take one of three forms.

1. `namespace-Block`
2. `namespace-Block-modifier`
2. `namespace-Block_Element`
3. `namespace-Block_Element-modifier`

Here, `namespace` is a two-to-four letter namespace. This could be a distinct set of types of different components, or it could be For example, for things related to the header, this can be namespaced under `hd-`. Different kinds of 'cards' would be namespaced `crd-`. Different kinds of modular sections would be `sec-`.

A `block` is a distinctive component with standalone meaning. Its first letter should always be capitalised. If you are struggling to find a name for it that doesn't fit in a single word, use `CondensedTitleCase` (though you're probably overthinking it).

An `element` is something which only ever exists within the context of a `block`.

A `modifier` describes a *minor* variation of a particular block or element.

An extremely simple example follows. Here is our HTML structure:


```css
.sec-Section {
  /* This is a standard section on a page, in cases where most or all sections
  on a page have similar layout and spacing rules. The `.sec` namespace is
  short for "sections", for pages that are built out of distinct, logically
  separate sections. */
  padding-top: 4vr;
  padding-bottom: @padding-top;

  background-color: #fff;
}

.sec-Section-grey {
  /* This follows all the same rules as `.sec-Section` above, with one tiny
  modification: its background colour is grey. */
  background-color: var(--Color_Grey-light);
}

.sec-Section_Inner {
  /* This is the inner part of a section. This doesn't have any applicability
  outside of a section. (Of course, containers are used elsewhere on a site,
  but we do not use utility classes.) */
  @include Grid_Container;
}

.sec-Section_Header {
  /* This defines the header for a section, with rules for how it will be
  spaced from the content. */
  margin-bottom: 2vr;

  text-align: center;
}

.sec-Section_Title {
  /* A section title which lives inside the above component. Its role is
  logically distinct, and separable, from being part of the section header -
  we define centre-aligned text and _Header above, not here. All this does is
  describe a standard font that will be used on all (or most) section titles.
  */
  @include Font_24-32;
}

.sec-Section_Body {
  /* This defines the 'body' of a section. Note that we don't actually have
  any assumptions about what that content might be! It might be a WYSIWYG
  block, or it might be a list of things, or it might be a picture, - don't
  make assumptions! More than likely whatever sits inside .sec-Section_Content
  will be namespaced differently. For example, if it was a list of news
  articles, we'd probably want to have something in the .lst- namespace. A
  list of news articles is a list of news articles that could be
  used in all sorts of ways, not just in something built with the section
  builder. */
}
```

## Selectors

Do not have duplicate selectors. You should only need to look in one block to see all the properties and states of any given component.

If your block has more than one selector, related selectors should be on the same line, and unrelated selectors should be on a new line.

### Specifity

As much as possible, you should always target a single class name, and as much as possible this should not be dependent on its context.

### Modifier classes

Modifier classes should not be nested if they target the same element. Prefer this:

```css
.crd-Card {
  background-color: #fff;
}

.crd-Card-transparent {
  background-color: transparent;
}
```

Rather than this:

```css
.crd-Card {
  background-color: #fff;

  &.crd-Card-transparent {
    background-color: transparent;
  }
}
```

This keeps the specifity to a single class name.

If you need to modify the attributes of a card based on the type or state of one of its ancestors, you should usually nest the parent selector inside the child element's definition block, rather than nesting the child element in the parent's definition block.


```css
.crd-Card_Body {
  color: var(--Color_Body);

  .crd-Card:hover & {
    color: var(--Color_Blue);
  }
}
```

In this case, it means you only need to look at one CSS block to see all the possible states for `.crd-Card`.

### !important

Do your best to avoid `!important`. There are rare exceptions, such as overriding other `!important` declarations added by third-party code, or inline styles added by the same, but they are rare.

### IDs as selectors

Don't target IDs in the form `#selector`, as it has [infinitely more specifity](https://css-tricks.com/specifics-on-css-specificity/) than class names.

If you are forced to target an ID (such as with elements injected by third-party code), target `*[id="foo"]` instead.


## Grouping properties

You should take a second to understand the [grouping configuration](https://github.com/onespacemedia/project-template/blob/develop/%7B%7Bcookiecutter.repo_name%7D%7D/package.json#L119) we use.

Properties are grouped for readability, with an empty line separating adjacent groups, in this order:

```css
 .nsp-Component_ChildNode {
    @include and @apply declarations;

    content properties;

    position properties;

    flex properties;

    display, width, height, margin and padding properties;

    font and text properties;

    background border and color properties;

    animation and transition properties;
  }
```

## Commenting

With the increasing complexity of Onespacemedia projects it is increasingly important to comment your CSS. Trying to understand your own code is difficult enough. Someone inheriting the project will find it more difficult than that.

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

If you use "magic" numbers in your CSS, add a comment above it explaining why you are using those numbers.

Comment any strange or clever CSS. Sometimes cross-browser fixes will require unusual rules that would not make any sense to a first-time observer, who might see it as a target for refactoring.

If you must add a `/* stylelint-disable */` comment to your CSS to bypass linting, it is often good form to write an explanatory comment about why it is disabled immediately above the line, to avoid the tendency to `/* stylelint-disable */` whenever a linting error is hit (for reasons both valid and invalid).

# HTML guidelines

In these examples, class names are omitted for readability.

## General formatting rules

Tag names, attributes, and attribute values should be in lowercase where possible.

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

An empty line between two HTML elements at the same indentation level is optional. Use your judgement.

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

### Jinja2 macros

If your macro takes many arguments, consider whether you can consolidate content-related ones into a single argument.

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

```python
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

## Using SVGs

Where possible, SVGs should be inline in HTML rather than included with `<img>` or applied with a `background-image`. This permits targeting SVG paths in CSS for things like hover effects and animations.

With a few rare exceptions, SVGs should contain only `path` elements, and you should be able to target the visible parts of the path with `fill` alone (rather than strokes). This gives a single target for CSS, and a single CSS property for styling it. Ask a designer to convert the file for you if you don't know how to convert it yourself.

When inlining SVGs, be careful with SVGs exported by Adobe Illustrator. Apart from being vastly bigger than they need to be, they almost invariably cause SVGs with identical IDs, identical class names, and inline `<style>` blocks. This often causes an SVG to apply its styles to paths in a different SVG earlier in the document.

[SVGOMG](https://jakearchibald.github.io/svgomg/) is a useful tool for cleaning up SVG files. Alternatively, the manual cleanup guide is this:

1. Remove the `<?xml` declaration.
2. Remove all attributes from the root `svg` element except `viewBox`.
3. Remove the entire `<defs>` block if one is present.
4. Remove any explicit `fill=` attributes on paths.
5. Replace class names with ones conforming to our naming conventions (or for SVGs with a single colour, like most icons, remove class names altogether)
6. Use CSS (within the standard frontend build system, not in the SVG itself) to style the SVGs, if necessary.

# JavaScript guidelines

We conform to the [Standard Style](https://standardjs.com/). In all semi-recent projects, this is enforced by the build system. To condense the Standard authors' summary:

- Two space for indentation
- Single quotes for strings, except to avoid escaping
- No unused variables
- No semicolons for statement termination
- Put a space after keywords and function names
- Use `===` instead of `==`
- Always prefix browser globals with `window`, except `document` and `navigator`

The one exception we've made to Standard's rules is that unused variables are a warning during development, not an error - unused variables should not make their way into production code.

Read the [complete set of rules](https://standardjs.com/rules.html) on the Standard site for more.

## ES6 features

These rules apply for scripts processed by our frontend build system, which are transpiled to universally-browser-friendly ES5. In the rare cases where inline JavaScript is used in a document, you should avoid ES6 features, as they are not universally and consistently implemented in all the browsers we support.

Always use `let` or `const`, never `var`. Always use `const` for locals which are not reassigned.

Use ES6 fat-arrow functions for anonymous functions whenever possible, which is almost always.

```javascript
window.addEventListener('DOMContentLoaded', () => {
  doDomContentLoadedStuff()
})
```

Use object-literal shorthand where you can:


```javascript
// In cases where our variable (or constant) names are the same as the names
// of our object keys, we can do this:
const animals = {
  cow,
  dog
}
```

Use ES6 string templates instead of ES5 concatenation:

```javascript
const animal = 'cat'
const animalPlural = `${animal}s`
```

Use ES6 `for...of` loops, rather than ES5 `for` when traversing arrays:

```javascript
for (const animal of animals) {
  animal.makeNoise()
}
```

# Python guidelines

If in doubt, follow [PEP 8](https://www.python.org/dev/peps/pep-0008/).

## String quotes

Prefer 'single quotes' over "double quotes".

If a Python file consistently uses double quotes, then stay consistent with that file by using double quotes for new code within it.

If a file uses a mixture of single and double quotes, use single quotes for new code, but feel free to convert it to use single quotes consistently.

For quoting text that contains an apostrophe, use double quotes. This example would permit grepping for the string "don't":

```python
have_bugs = models.BooleanField(
    default=False,
    help_text="Don't check this!"
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

Use [isort's default ordering](http://timothycrosley.github.io/isort/). Group imports thusly:

1. Imports from `__future__`
2. Python standard library
3. Third-party 'globally' installed packages (in our case, always installed in a virtual environment)
4. Current project
5. Explicit local imports (relative imports)

Use isort's "Grid" option for multi-line outputs.

Project-local imports should usually be relative imports. Imports from further up the directory structure should come first.

```
from ...utils.views import FilteredListView
from ..other_app.models import Taxonomy
from .models import Article, Category
```

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

There should be a new line before the first argument to a field's constructor, and each argument goes on a new line. This makes diffs easier to read when arguments are added or deleted.

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

All models should define an `ordering` attribute in their `Meta` class. Postgresql, our preferred database, will return rows in an order that is ["unspecified"](https://www.postgresql.org/docs/9.1/static/queries-order.html) if you do not. Don't rely on primary key ordering to give date-added-based ordering; this is not guaranteed. Implement a "date added" field and have explicit ordering on that column.

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

Always ensure that the `verbose_name_plural` for a model makes sense. Django's default behaviour will pluralise 'Category' as 'Categorys', for example.

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

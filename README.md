# TODO:

## CSS
* [ ] Margin vs Padding on sections
* [ ] `& + &` vs all items having it and explicitly defining the `{first,last}-child`
* [ ] `Why --Grid_Container` can't take the `percentage(x, x)` as well
* [ ] How to do cards height (iOS issues with height: 100%)
* [ ] How to do overflow on body with iOS

## HTML
* [ ] Why SVG's are best used inline instead of as a `src` or `bgi`
* [ ] Sections don't all have to be a unique class
* [ ] Explain the `_Header` `_Body` style way of doing things and the benefits
* [ ] Explain the standard classes `_Title`, `_Text`, etc.


## Python
* [ ] Django model attributes ordering and newslines
* [ ] Single quotes
* [ ] Consistency with older projects
* [ ] String literals on a single line where appropriate

## Templates
* [ ] Indentation
* [ ] Spaces inside curlies
* [ ] With/set in jinja2 - spacing

}

The end of the documentation are all the front-end sections I can think of that need to be covered, as I am not backend hopefully we can work together to create something definitive that we can all reference. Thanks!

-----------------------------------

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

## Enduring CSS

- Understanding the problem

## Reusability

 - Naming

## Specifity

## Definition Properties

## Vertical Rythm

## Browser testing

## Responsive development

## Models

## CMS / Admin

## User considerations

## SEO

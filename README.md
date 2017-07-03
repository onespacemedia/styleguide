This has been updated to a [docsify](https://docsify.js.org/#/quickstart) document, to start:
- Run 
```bash
npm i docsify-cli -g
```
- Clone the repo into your Workspace folder
- You don't need to CD that folder
- Run 
```bash
docsify init ./styleguide
```

Update the README.md file with markdown language and it will automaticaly refresh on the front-end

Old READ.md file below {

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

As OSM moves forward and grows as a reputable design agency, it’s imperative to make sure as a team we are consistently producing work that makes us unique and stand out from the crowd, this is why I have written a styleguide for your referral as a member of the dev team or a new member to outline what is expected and how to achieve the best possible standard so as we start to increase in project size and frequency.

As the projects we start to work on become larger in scale and long-running it is very important that we work in a unified way and with a ever growing team with different specialities and abilities it is necessary to keep the process streamline in order to:

- Keep everything maintainable;
- Keep code transparent, sane, and readable;
- Keep everything scalable

# Syntax & formatting

Writing clean code is something that is good to look at, and although we use stylelist and now with the introduction of pre-commits we can rely on [Stylelint](https://stylelint.io/) to slap our wrists when we write something poorly or in the incorrect way, it is still good to understand as a team what the standard syntax should be when writing good sexy CSS, this includes:

- Two space indent, (I am sure all your ide’s are set up to this but it’s good to specify);
- 80 character wide columns, to allow easy readability;
- Multi-line CSS;
- Meaningful use of whitespace;
- Grouping properties.

## Terminology and order

It’s good understand how to properly write CSS and know the terminology, the best way to write your CSS is as follows:

- Related selectors on the same line, unrelated selectors on a new line;
- A space before the opening curly brace ({)
- Properties and values on the same line;
- A space after our property, with a colon (:);
- Opening brace on the same line as our last selector ({);
- Declarations on a new line including after the first opening curly brace ({);
- Closing curly brace on a new line (});
- Trailing semicolon at the end of each declaration (;).

Example follows:

```css
  [selector] [related selector],
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
   
   background-image: url(/img/svg.svg);
 }
 
 .nsp-Component_ChildNode-home     { background-position:   0     0  ; }
 .nsp-Component_ChildNode-person   { background-position: -16px   0  ; }
 .nsp-Component_ChildNode-files    { background-position:   0   -16px; }
 .nsp-Component_ChildNode-settings { background-position: -16px -16px; }
 ```
## Stylelint and grouping properties

[Stylelint](https://stylelint.io/) is utilised to try and catch errors when code is push in order to make sure it is to a good quality, that doesn’t mean you should rely on it, take a second to understand the [configuration](https://github.com/onespacemedia/project-template/blob/develop/%7B%7Bcookiecutter.repo_name%7D%7D/package.json#L119) we use. Properties are grouped together so they’re easily readable with a empty line between, a quick breakdown is as follows (in order):

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

Other things to consider:
- All color and hex values must be lowercase and shortened;
- You must’nt use duplicate selectors;
- Properties should always be lowercase;
- .css files should always end with an empty blank space;
- Id shouldn’t be used as a selector.

A recommendation for new people joining the team would be to read the documentation above about linting as we also use it for javascript files as well, if you are familiar with linting then take a moment to run through the order above.

# Commenting

With the increase in size in projects it becomes more and more important to comment your css, trying to understand and remember your own code is difficult within itself let alone anyone inheriting the project.

There should always be a heading comment at the top of a .css file that includes the component name and the nsp selector, example:

```css
/*
|--------------------------------------------------------------------------
| Component
|--------------------------------------------------------------------------
| @namespace: nsp-
|
*/
```

Then comments after should be grouped and order in the same way the html is ordered keeping everything together in small sections in the file, example:

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

# Enduring CSS

- Understanding the problem

# Reusability
 
 - Naming

# Specifitiy

# Definition Properties

# Vertical Rythm

# Browser testing

# Responsive development

# Models

# CMS / Admin

# User considerations

# SEO

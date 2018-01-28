Originally located at onespacemedia/developer-guidelines-old

# Onespacemedia Developer Guidelines

This project represents a living document of the processes developers at Onespacemedia are expected to follow.  It covers code style, project setup and other facets of developer life.  Following the guidelines will ensure that code is consistent across all projects and will lower the barrier to entry of a developer joining a new project.  Contributions, comments and suggestions are welcomed.

### Table of Contents

* [Project essentials](#markdown-header-project-essentials)
* [Code style and automated tools](#markdown-header-code-style-and-automated-tools)
    * [Python](#markdown-header-python)
        * [PyLint and PEP8 exceptions](#markdown-header-pylint-and-pep8-exceptions)
        * [Useful libraries](#markdown-header-useful-libraries)
        * [Example code](#markdown-header-example-code)
    * [Javascript](#markdown-header-javascript)
        * [Example code](#markdown-header-example-code_1)
    * [SCSS / CSS](#markdown-header-scss-css)
        * [Example code](#markdown-header-example-code_2)
    * [HTML / Templating](#markdown-header-html-templating)
    * [Server management](#markdown-header-server-management)

---

## **Project essentials**
* All projects **MUST** use Virtual Environments, this cuts down on globally installed packages causing issues and also allows projects to have reproducable installation steps.  You must provide the project requirements in the root of each project, either in a file named `requirements.txt`, or in a folder named `requirements` with a file for each environment (i.e. requirements/local.txt, requirements/production.txt).
* Ensure a .gitignore file is present in the base of the project, one is included in the base CMS project template. Do not add local.py to the .gitignore file, it should be in the repository.
* Unless otherwise specified, projects should be stored on Bitbucket under the Onespacemedia team.  Clients may be given access to the repository after full payment has been made unless otherwise specified.
* When you have created a new repository, make sure you have added a POST hook to post commits to the #commits Slack channel.
* Code commits should be as small as possible and containing specific changes, described accurately in the commit message.  Do not use generic commit messages such as "Made changes" or "Text tweak". Be descriptive. If you have made multiple changes use the interactive commit system (`git commit -p`) and cherry-pick the feature-specific code. Do not group multiple changes together in the same commit.
* We use Digital Ocean for our hosting. The default configuration is the $10 package in London, with IPv6 and backups enabled. The operating system should be Ubuntu 14.04 x64 with all developer SSH keys added. Use the client's domain name as the hostname.
* Each project has a checklist which covers each of the steps in creating a website. You and your line manager will both need to sign off each section as it's completed.  This helps to ensure we do not miss crucial parts of the build process.

---

## **Code style and automated tools**

We have a number of automation and pre-configured tools to help speed up the setting up of new projects as well as general management, deployment etc.  The main ones you need to know about are as follows.

* [developer-automation](http://bitbucket.onespace.media/developer-automation). This project will take care of the initial setup of a project, from creating the repo, setting it up on your machine and configuring all of the external dependancies.
* [start\_cms\_project.py](https://github.com/onespacemedia/cms/blob/master/cms/bin/start_cms_project.py). This is run as part of the `developer-automation` package, but it's useful to know about.  When you `pip install onespacemedia-cms` you gain access to a `start_cms_project.py` command which will allow you to start a new project using our project layout.  Running the command will look something like this `start_cms_project.py testing .` (the `.` means to start it in the current directory, typically the root of your Git repo).
* [onespacemedia-server-management](https://github.com/onespacemedia/server-management/). This project provides a number of server management tools, such as pushing and pulling the remote database and media files, deploying a site for the first time, and updating sites when they are live. The commands are available from `./manage.py`.
* We use [Sentry](https://app.getsentry.com/onespacemedia/teams/onespacemedia/) to monitor our sites for errors. When errors occur on sites they get logged into Sentry and automatically posted to the [#commits channel in Slack](https://onespacemedia.slack.com/messages/commits/).  It's worth keeping an eye on this channel just in case any recent code pushes have started generating errors.  Generally speaking, when you see an error occur it's a good idea to have a quick look at it, if it's a quick fix then just do it and push the fix live.
* We use [Harvest](https://onespacemedia.harvestapp.com/time/week) to track our time spent on projects.  They have a nice [Mac OS X app](https://www.getharvest.com/mac) which makes logging time a lot easier.
* We use [Mailtrap](https://mailtrap.io/inboxes/37126/messages) for testing email sending during development.  This mailbox comes pre-configured in all CMS projects. When projects go-live, we use [Mandrill](https://mandrillapp.com/login/) as our SMTP server.  This gets auto-configured by the `developer-automation` project.  Ask your line manager for the credentials.  


### Python
* Indent with 4 spaces. 
* Written to conform to PyLint specifications and PEP8, with some exceptions (see below). It's strongly recommended that you set these up in your editor of choice.
* Ensure your migrations actually work. If someone pulls down the repository they should be able to just run `./manage.py migrate` without having to specific apps or fake certain migrations.
* Follow the [Django style guide](https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/coding-style/) when it comes to naming and function orders.
* Model field attributes should have their own line. Do not put them all on the same line.
* Put a trailing comma on all model attribute lines.
* Try to group your imports at the top of your files. Where possible, use alphabetical imports and use these as your groups:
    * Django imports.
    * Project imports.
    * 3rd party library imports.
    * Standard library imports.
* Where possible, use relative imports.
* Ensure your editor trims trailing whitespace on save.
* Ensure your editor adds a blank line at the end of the file on save.
* Always have a blank line as the first line in a class.
* Never use function based views.
* Class names should be in `UpperCamelCase`.
* Contants should be `CAPITALISED_WITH_UNDERSCORES`
* All other variables should be `lower_case_with_underscores`.

#### PyLint and PEP8 exceptions
**PyLint**

* Disable the following: W0403, W0232, E1101, C0111, R0904, E1002, R0912, C0103, R0801, R0914, C0301, C1001, E1120, E0202, W0613, W0212, W0142

**PEP8**

* E501 (80 character line length) is not a hard requirement, but try to keep to it where possible.

#### Useful libraries

* [Requests](http://docs.python-requests.org/en/latest/) - Makes HTTP requests very simple.
* [Twython](http://twython.readthedocs.org/en/latest/) - Twitter API

#### Example code

```
#!python

from django.conf import settings
from django.views.generic import DetailView

from .models import File, Image, Video

from cms.views import ExampleView

from datetime import date


class File(models.Model):

    file = models.FileField(
        blank=True,
        null=True,
    )

    def get_absolute_url(self):
        return reverse('file:detail', kwargs={
            'pk': self.pk,
        })


class FileDetail(DetailView):

    model = File

```

---

### Javascript
We use JSHint to validate our Javascript, ensure you meet it's requirements.  The main points are as follows:

* Indent with 2 spaces.
* If you have global variables (such as `$` or `document`) define them at the top of the document.
* Always use strict comparisons (`!==` instead of `!=`).
* Using `"use strict";` is not required, though not discouraged.
* Ensure you have semi-colons where required.


#### Example code

```
#!javascript

/*global $ */

var example,
    value;

example = function (param) {
    console.log(param);

    return {
        'key': 'value'
    };
};

$(function () {
    value = example();
});
```

---

### SCSS / CSS
CSS style is defined as follows:

* New projects use Gulp with node-sass. You can compile your SCSS by running `gulp`.
* Older projects will likely be using Compass, either version 0.13.alpha.4 or 1.0.1.
* Try to avoid using tag names in your selectors.
* When nesting SCSS rules, do not go deeper than 4 levels.
* If you have multiple parts to your selector, splitting it across multiple lines can be helpful.
* Use 4 spaces for indentation.
* Put a single space before the `{` in a rule declaration.
* Do not have any spaces between the property and the `:`.
* Have a single space between the property `:` and the value.
* Have a new line after the opening `{` and before the closing `}`.
* Each declaration should be on it's own line.
* Use hex colour codes `#FF0` unless using `rgba()`.
* Use `//` for comment blocks (instead of `/* */`).
* Don't specify units for zero values. (`margin: 0;` instead of `margin: 0px;`).
* Use standard CSS, don't worry about browser specific prefixes, autoprefixer takes care of that for us.
* Use `lower-case-with-hyphens` for class names and variables.


#### Example code

```
#!css

// Main navigation items
.menu-item {
    color: #F00;
    background-color: rgba(255, 100, 0, 0.2);
    font-size: 24px;
    padding-top: 0;

    a {
        color: inherit;
    }
}
```

---

### HTML / Templating

* Use HTML5 tags where suitable.
* Make use of HTML5 input types, but ensure validation for non-supported browsers is in place.
* Make use of fragment caching, but be careful not to cache user-specific data without the correct permission handling in place.
* Prefer Google Fonts for webfonts, with Typekit and Fontdeck as backups.
* Prefer Font Awesome for icons.
* Don't assume CDNs will always be available.  Bring external resources (JS / CSS) into the project where possible.
* If you need to re-use some HTML, consider splitting it out into an include. Reducing code duplication is almost always a good idea.

---

### Server management

We have several server management projects. For projects using Django <=1.6, use [Digital Ocean Management](https://bitbucket.org/onespacemedia/digital-ocean-management), for projects on Django 1.7+ use [onespacemedia-server-management](https://github.com/onespacemedia/server-management/), which is included by default in new projects.

These projects handle the standard tasks we need to perform with our servers:

* Deploying a site for the first time.
* Updating an existing application.
* Pushing your local database to the live server.
* Pushing your local media files to the live server.
* Pulling the live database to your local machine.
* Pulling the live media files to your local machine.

The Digital Ocean Management package also handles:

* Migrating a website from Webfaction to Digital Ocean
* General Digital Ocean server management.

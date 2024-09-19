# Jekyll [Step by Step Tutorial](https://jekyllrb.com/docs/step-by-step/01-setup/)

## 1. Setup
Welcome to Jekyll’s step-by-step tutorial. This tutorial takes you from having some front-end web development experience to building your first Jekyll site from scratch without relying on the default gem-based theme.

### Installation
Jekyll is a Ruby gem. First, install Ruby on your machine. Go to [Installation](https://jekyllrb.com/docs/installation/) and follow the instructions for your operating system.

With Ruby installed, install Jekyll from the terminal:

```
gem install jekyll bundler
```

Create a new `Gemfile` to list your project’s dependencies:

```
bundle init
```

Edit the `Gemfile` in a text editor and add jekyll as a dependency:

```
gem "jekyll"
```

Run `bundle` to install jekyll for your project.

You can now prefix all jekyll commands listed in this tutorial with `bundle exec` to make sure you use the jekyll version defined in your `Gemfile`.

### Create a site
It’s time to create a site! Create a new directory for your site and name it whatever you want. Through the rest of this tutorial we’ll refer to this directory as **root**.

You can also initialize a Git repository here.

One of the great things about Jekyll is there’s no database. All content and site structure are files that a Git repository can version. Using a repository is optional but is recommended. You can learn more about using Git by reading the [Git Handbook](https://guides.github.com/introduction/git-handbook/).

Let’s add your first file. Create `index.html` in **root** with the following content:

~~~html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Home</title>
  </head>
  <body>
    <h1>Hello World!</h1>
  </body>
</html>
~~~

### Build
Since Jekyll is a static site generator, it has to build the site before we can view it. Run either of the following commands to build your site:

- `jekyll build` - Builds the site and outputs a static site to a directory called `_site`.
- `jekyll serve` - Does `jekyll build` and runs it on a local web server at http://localhost:4000, rebuilding the site any time you make a change.

    :warning: When you’re developing a site, use `jekyll serve`. To force the browser to refresh with every change, use `jekyll serve --livereload`. If there’s a conflict or you’d like Jekyll to serve your development site at a different URL, use the `--host` and `--port` arguments, as described in the `[serve command options](https://jekyllrb.com/docs/configuration/options/#serve-command-options).

    :bulb: The version of the site that jekyll serve builds in `_site` is not suited for deployment. Links and asset URLs in sites created with `jekyll serve` will use `https://localhost:4000` or the value set with command-line configuration, instead of the values set in your [site’s configuration file](https://jekyllrb.com/docs/configuration/). To learn about how to build your site when it’s ready for deployment, read the [Deployment](https://jekyllrb.com/docs/step-by-step/10-deployment/) section of this tutorial.

Run `jekyll serve` and go to http://localhost:4000 in your browser. You should see “Hello World!”.

At this point, you might be thinking, “So what?”. The only thing that happened was that Jekyll copied an HTML file from one place to another.

Patience, young grasshopper, there’s still much to learn!

Next. you’ll learn about Liquid and templating.

## 2. Liquid
Liquid is where Jekyll starts to get more interesting. It is a templating language which has three main components:

- objects
- tags
- filters

### Objects
Objects tell Liquid to output predefined variables as content on a page. Use double curly braces for objects: `{{` and `}}`.

For example, `{{ page.title }}` displays the `page.title` variable.

### Tags
Tags define the logic and control flow for templates. Use curly braces and percent signs for tags: `{% and %}`.

For example:

~~~html
{% if page.show_sidebar %}
  <div class="sidebar">
    sidebar content
  </div>
{% endif %}
~~~

This displays the sidebar if the value of the `show_sidebar` page variable is true.

Learn more about the tags available in Jekyll `[here](https://jekyllrb.com/docs/liquid/tags/).

### Filters
Filters change the output of a Liquid object. They are used within an output and are separated by a `|`.

For example:

~~~html
{{ "hi" | capitalize }}
~~~

This displays `Hi` instead of `hi`.

[Learn more about the filters available](https://jekyllrb.com/docs/liquid/filters/).

### Use Liquid
Now, use Liquid to make your `Hello World!` text from [Setup](#1-setup) lowercase:

~~~html
...
<h1>{{ "Hello World!" | downcase }}</h1>
...
~~~

To make Jekyll process your changes, add front matter to the top of the page:

~~~html
---
# front matter tells Jekyll to process Liquid
---
~~~

Your HTML document should look like this:

~~~html
---
---

<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Home</title>
  </head>
  <body>
    <h1>{{ "Hello World!" | downcase }}</h1>
  </body>
</html>
~~~

When you reload your browser, you should see `hello world!`.

___

Much of Jekyll’s power comes from combining Liquid with other features. Add frontmatter to pages to make Jekyll process the Liquid on those pages.

## 3. Front Matter
Front matter is a snippet of YAML placed between two triple-dashed lines at the start of a file. You can use front matter to set variables for the page:

~~~
---
my_number: 5
---
~~~

You can call front matter variables in Liquid using the page variable. For example, to output the value of the `my_number` variable above:

~~~
{{ page.my_number }}
~~~

### Use front matter
Change the `<title>` on your site to use front matter:

~~~html
---
title: Home
---
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
  </head>
  <body>
    <h1>{{ "Hello World!" | downcase }}</h1>
  </body>
</html>
~~~

    :warning: You must include front matter on the page for Jekyll to process any Liquid tags on it. 

To make Jekyll process a page without defining variables in the front matter, use:

~~~
---
---
~~~

## 4. Layouts
Jekyll supports Markdown in addition to HTML when building pages. Markdown is a great choice for pages with a simple content structure (just paragraphs, headings and images), as it’s less verbose than raw HTML.

Create a new Markdown file named `about.md` in your site’s root folder.

You could copy the contents of `index` and modify it for the About page. However, this creates duplicate code that has to be customized for each new page you add to your site.

For example, adding a new stylesheet to your site would involve adding the link to the stylesheet to the `<head>` of each page. For sites with many pages, this is a waste of time.

### Creating a layout
Layouts are templates that can be used by any page in your site and wrap around page content. They are stored in a directory called `_layouts`.

Create the `_layouts` directory in your site’s root folder and create a new `default.html` file with the following content:

~~~html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
  </head>
  <body>
    {{ content }}
  </body>
</html>
~~~

This HTML is almost identical to `index.html` except there’s no front matter and the content of the page is replaced by a `content` variable.

`content` is a special variable that returns the rendered content of the page on which it’s called.

### Use layouts
To make `index.html` use your new layout, set the layout variable in the front matter. The file should look like this:

~~~html
---
layout: default
title: Home
---
<h1>{{ "Hello World!" | downcase }}</h1>
~~~

When you reload the site, the output remains the same.

Since the layout wraps around the content on the page, you can call front matter like page in the layout file. When you apply the layout to a page, it uses the front matter on that page.

### Build the About page
Add the following to `about.md` to use your new layout in the About page:

~~~
---
layout: default
title: About
---
# About page

This page tells you a little bit about me.
~~~

Open `http://localhost:4000/about.html` in your browser and view your new page.

Congratulations, you now have a two page website!

Next, you’ll learn about navigating from page to page in your site.

## 5. Includes
The site is coming together; however, there’s no way to navigate between pages. Let’s fix that.

Navigation should be on every page so adding it to your layout is the correct place to do this. Instead of adding it directly to the layout, let’s use this as an opportunity to learn about includes.

### Include tag
The include tag allows you to include content from another file stored in an _includes folder. Includes are useful for having a single source for source code that repeats around the site or for improving the readability.

Navigation source code can get complex, so sometimes it’s nice to move it into an include.

### Include usage
Create a file for the navigation at _includes/navigation.html with the following content:

~~~html
<nav>
  <a href="/">Home</a>
  <a href="/about.html">About</a>
</nav>
~~~

Try using the include tag to add the navigation to _layouts/default.html:

~~~html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
  </head>
  <body>
    {% include navigation.html %}
    {{ content }}
  </body>
</html>
~~~

Open http://localhost:4000 in your browser and try switching between the pages.

### Current page highlighting
Let’s take this a step further and highlight the current page in the navigation.

_includes/navigation.html needs to know the URL of the page it’s inserted into so it can add styling. Jekyll has useful variables available, one of which is page.url.

Using page.url you can check if each link is the current page and color it red if true:

~~~html
<nav>
  <a href="/" {% if page.url == "/" %}style="color: red;"{% endif %}>
    Home
  </a>
  <a href="/about.html" {% if page.url == "/about.html" %}style="color: red;"{% endif %}>
    About
  </a>
</nav>
~~~

Take a look at http://localhost:4000 and see your red link for the current page.

There’s still a lot of repetition here if you wanted to add a new item to the navigation or change the highlight color. In the next step we’ll address this.
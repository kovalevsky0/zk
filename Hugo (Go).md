---
title: Hugo (Go)
tags:
  - hugo
  - golang
public: true
type: note
date: 2021-06-22
---

# Hugo (Go)

## Links

* [gohugo.io](https://gohugo.io)
* [GitHub](https://github.com/gohugoio/hugo)

A static-site generator written in [Golang](Golang.md).

## Resources

* [byteski/hugo-blog-sample](https://github.com/maxoidIO/hugo-blog-sample)

## Installing

````
brew install hugo
````

````
hugo version
````

````
Hugo Static Site Generator v0.80.0/extended darwin/amd64 BuildDate: unknown
````

## Creating a new site

````
hugo new site <folder-name>
````

## Structure

````
ls -1

archetypes/
config.toml
content/
data/
layouts/
static/
themes/
````

* **archetypes/** - entities that are smth like markdown templates of content
* **config.toml** - main configuration file
* **content/** - folder that contains markdown content
* **data/** - folder that contains configuration files and data that used in generating your site
* **layout/** - folder that contains html templates for specific parts of website
* **static/** - folder that contains static files like images, fonts, etc.
* **themes/** - folder that can contain existing Hugo themes

## Starting website

````
hugo server
````

To running and see draft content:

````
hugo server -D
````

 > 
 > The empty white page in just created app is okay

On root route (/) Hugo try to render layout with name:

* **list.html** 
* or 
* **index.html** (**index.html** takes priority)

Types of pages:

* single (page of the specific post)
* list (page that contains list of posts)

## Configuration file

By default it is **config.toml**

It is possible to use YAML file. Just rename it to **config.yml**

Default settings:

**config.toml**

````toml
baseURL = "http://example.org/"
languageCode = "en-us"
title = "My New Hugo Site"
````

**config.yml**

````yml
baseURL: "http://example.org/"
languageCode: "en-us"
title: "My New Hugo Site"
````

## Templating

Create template of the main page in path **/layouts/list.html**

### Add website title

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ .Site.Title }}</title>
</head>
<body>
    
</body>
</html>
````

**.Site.Title**:

* **Site** - context
  * it is context of config.yml file
* **Title** - variable

### Add page title

Create file **/content/\_index.md**

````md
---
title: "Home"
---
````

* \_index.md  - file that contains content of home page (root route - '/')

Add title of home page in **/layouts/list.html**:

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ .Site.Title }}</title> <!-- Title of website -->
</head>
<body>
    <h1>{{ .Page.Title }}</h1> <!-- Title of the page -->
</body>
</html>
````

Because the layout is already in context of the page you can just change it:

````html
<h1>{{ .Page.Title }}</h1> <!-- Title of the page -->
````

to 

````html
<h1>{{ .Title }}</h1> <!-- Title of the page -->
````

## Render list of pages in loop

Create new pages **/content/about.md** and **/content/contact.md**

````md
---
title: "About"
---
````

To render list of these pages with their links just add this code in **layouts/list.html**:

````html
<ul>
	{{ range .Pages }}
		<li><a href="{{ .Permalink }}">{{ .Title }}</a></li>
	{{ end }}
</ul>
````

* **range** is a loop here. it iterates in **.Pages** which is a list of data about each page
* **.Permalink** - link to the specific page
* **.Title** - the title of the page which is declared in markdown metadata

## Default layouts

Create folder **/layouts/\_default** and move **layouts/list.html** there

Layouts from this folder **/layouts/\_default** will be used for all content by default

## Creating template for a single page

Create file **/layouts/\_default/single.html**:

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ .Title }} | {{ .Site.Title }}</title>
</head>
<body>
    <h1>{{ .Title }}</h1>
    <div>{{ .Content }}</div>
</body>
</html>
````

If you open page **/about** in the browser you'll see this html structure.

## Display list of pages with specific type

Change the loop in the layout to display list of resources that have type "page":

````html
<ul>
	{{ range where .Pages "Type" "page" }}
		<li><a href="{{ .Permalink }}">{{ .Title }}</a></li>
	{{ end }}
</ul>
````

By default markdown resources in **/content** has type "page".

If you don't want to show some specific resource as page you can specify type in metadata:

````md
---
title: "About"
type: "post"
---
````

## Display list of pages/posts of specific section

### What is Section?

More information you can find in [official doc of Hugo](https://gohugo.io/templates/section-templates/#readout)

I understand sections like sub folders in a folder **content**

Basically, you have a folder **content** that contains two folders - **blog** and **notes**.

In that case, **blog** and **notes** are **Sections** in Hugo.

### Display list of resources of section "blog"

````html
{{ range where .Site.RegularPages "Section" "blog" }}
	<li>
		<a href="{{ .Permalink }}">{{ .Title }}</a>
	</li>
{{ end }}
````

#### With pagination

````html
<div>
	{{ $paginator := .Paginate (where .Pages "Section" "blog") }}
	{{ range $paginator.Pages }}
	<article>
		<div>
			<a href="{{ .Permalink }}">{{ .Title }}</a>
		</div>
	</article>
	{{ end }}
	{{ template "_internal/pagination.html" . }}
</div>
````

## Splitting layouts

Let's say we have two layouts: for home page **layouts/\_default/list.html** and for single page **layouts/\_default/single.html**

They have pretty similar html structure:

**list.html**

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ .Site.Title }}</title> <!-- Title of website -->
</head>
<body>
    <h1>{{ .Title }}</h1>
    <div>
        <ul>
            {{ range where .Pages "Type" "page" }}
                <li><a href="{{ .Permalink }}">{{ .Title }}</a></li>
            {{ end }}
        </ul>
    </div>
</body>
</html>
````

**single.html**

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ .Title }} | {{ .Site.Title }}</title>
</head>
<body>
    <h1>{{ .Title }}</h1>
    <div>{{ .Content }}</div>
</body>
</html>
````

To avoid repeating code we can make base layout that will contain common html structure. 

Create the file **/layouts/\_default/baseof.html**

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ .Site.Title }}</title> <!-- Title of website -->
</head>
<body>
    <h1>{{ .Site.Title }}</h1>
    <div>
        <ul>
            {{ range where .Site.RegularPages "Type" "page" }}
                <li><a href="{{ .Permalink }}">{{ .Title }}</a></li>
            {{ end }}
        </ul>
    </div>
    {{ block "content" . }}{{ end }}
</body>
</html>
````

* `{{ block "content" . }}{{ end }}` renders layouts that have the same "name"

For home page let's change **/layouts/\_default/list.html**:

````html
{{ define "content" }}
    <h2>Index</h2>
{{ end }}
````

And now the **baseof** layout renders the sub layout **list.html** for home page

For single page let's change **/layouts/\_default/single.html**:

````html
{{ define "content"}}
	<h2>{{ .Title }}</h2>
	<div>{{ .Content }}</div>
{{ end }}
````

## Creating blog section

Create folder **/content/blog** and put there new file **/content/blog/\_index.md**

````md
---
title: "Blog"
---
````

And then create a few blank posts:

````md
---
title: "First Post"
---

Deserunt duis fugiat reprehenderit officia cillum qui cillum Lorem. Aliquip sint esse fugiat elit. In incididunt quis ullamco cupidatat tempor. Lorem ad non incididunt ad magna nostrud.
````

````md
---
title: "Second Post"
---

...
````

### Layout of posts list

To display list of posts let's create new layout. But first of all, we need to create new folder **/layouts/blog**.

Why?

* **/layouts/\_default** could used for general resources like pages
* **/layouts/blog** here are layouts for resources like blog posts

In **/layouts/blog** create file **list.html**:

````html
{{ define "content" }}
<div>
    <h2>{{ .Title }}</h2>
    <ul>
        {{ range where .Site.RegularPages "Type" "blog" }}
        <li>
            <a href="{{ .Permalink }}">{{ .Title }}</a>
        </li>
        {{ end }}
    </ul>
</div>
{{ end }}
````

It will display list of posts links

### Layout of specific post

Let's create a layout for page of post

In folder **/layouts/blog** create file **single.html**:

````html
{{ define "content" }}
<div>
    <h2>Post: {{ .Title }}</h2>
    <div>{{ .Content }}</div>
</div>
{{ end }}
````

This html structure will be used only for posts. It won't be used for all pages (for example about or contact).

## Using partials templates

Partials templates are specific layouts or files that can be used as parts of layout.

Let's say we have file **/layouts/\_default/baseof.html**:

````html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ .Site.Title }}</title> <!-- Title of website -->
</head>

<body>
<!-- something here -->
</body>

</html>
````

Here we have section **head** that we could make it as partial template.

Create folder **/layouts/partials**. It is a specific folder for all partials.

In that new folder let's create file **head.html** with head section of html structure:

````html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ .Site.Title }}</title> <!-- Title of website -->
</head>
````

To use this partial change the file **baseof.html**:

````html
<!DOCTYPE html>
<html lang="en">

<!-- using partial with section "head" -->
{{ partial "head" . }}

<body>
<!-- something here -->
</body>

</html>
````

## Shortcodes

### What Are Shortcodes?

More information [here (gohugo.io)](https://gohugo.io/content-management/shortcodes/#readout)

Basically, it's something similar to components in MDX but without React/JSX.

You can put a specific code in your [markdown](markdown.md) file and it will be rendered like a widget.

### Built-in Shortcodes

There are several built-in shortcodes in Hugo that can be used out of box.

For example, shortcode **youtube** that renders a video on the page:

````markdown
{{< youtube w7Ft2ymGmfc >}}
````

## Taxonomies

Basically, taxonomies are tags and categories or some new thing that (you can make it) has similar behavior to tags or categories.

I take it that way:

* Sections: One-to-many
  * one section (blog)
  * many resources
* Taxonomies: Many-to-many
  * many taxonomies (tags)
  * many resources

### Creating a new taxonomy

Let's call it **technologies**

First of all, we need to specify new taxonomy in **config file**:

````yml
baseURL: "http://example.org/"
languageCode: "en-us"
title: "My New Hugo Site"
taxonomies:
  category: categories
  tag: tags
  technology: technologies
````

 > 
 > notice that we need to specify built-in taxonomies too if you want to use them

Add this in some of your markdown resources:

````markdown
---
title: "Second Post"
technologies: ["typescript", "react", "node"]
---

...
````

To render the list of technologies in each post:

````html
<div>
	{{ $paginator := .Paginate (where .Pages "Section" "blog") }}
	{{ range $paginator.Pages }}
		<article>
		<!-- ... -->
		<div>
			{{ range (.GetTerms "technologies") }}
				<li><a href="{{ .Permalink }}">#{{ .LinkTitle }}</a></li>
			{{ end }}
		</div>
		<!-- ... -->
		</article>
	{{ end }}
	{{ template "_internal/pagination.html" . }}
</div>
````

To render the list of posts that has specific technology:

* file: **/layouts/taxonomy/technology.html**
* in the browser: /technologies/typescript

````html
{{ define "content" }}
<div>
    <h2>Technology: {{ .Title }}</h2>
    <ul>
        {{ range .Data.Pages }}
        <li>
          <a href="{{.RelPermalink}}">{{ .Title }}</a>
        </li>
        {{ end }}
      </ul>
</div>
{{ end }}
````

## Tags

Tags in Hugo are built-in taxonomies.

### Adding tags into the post

````markdown
<!-- /content/blog/First Post.md -->

---
title: "First Post"
draft: false
tags: ["tag1", "tag2"]
---

....
````

### Creating a page with all tags of all resources (posts)

* file: **/layouts/tag/list.html**
* in the browser: /tags

````html
{{ define "content" }}
<div>
  <h2>{{ .Title }}</h2>
  <ul>
    {{ range .Site.Taxonomies.tags }}
            <li><a href="{{ .Page.Permalink }}">{{ .Page.Title }}</a> {{ .Count }}</li>
    {{ end }}
  </ul>
</div>
{{ end }}
````

### Creating a page of specific tag with list of posts

* file: **/layouts/taxonomy/tag.html**
* in the browser: /tags/\<tag_name\>

````html
{{ define "content" }}
<div>
    <h2>#{{ .Title }}</h2>
    <ul>
        {{ range .Data.Pages }}
        <li>
          <a href="{{.RelPermalink}}">{{ .Title }}</a>
        </li>
        {{ end }}
      </ul>
</div>
{{ end }}
````

### Useful links

* [Extend markdown parser for Hugo](https://kaleo.blog/article/extend-markdown-parser-for-hugo/)

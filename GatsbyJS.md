---
title: Gatsby.js
tags:
  - gatsbyjs
  - javascript
public: true
date: 2020-12-13
---

# Gatsby.js

[gatsbyjs.com](https://www.gatsbyjs.com/)

**Gatsby.js** is a static site generator written in [JavaScript](JavaScript.md) for [React](React.md)

## Quick Start

### Gatsby CLI

To start making your application you should install [gatsby-cli](https://www.gatsbyjs.com/docs/gatsby-cli/) globally by these [npm](npm.md) command:

````
npm install -g gatsby-cli
````

To check that **gatsby-cli** was successfully installed just use this command that should return version of **gatsby-cli** on your computer:

````
gatsby --version
````

## Creating a new Gatsby application

To create a new **Gatsby** application use command:

`gatsby new <app_name>`

It will create a new folder with a name of your app and this folder will contain bootstrap of file structure based on **Gatsby**

### Start an application

After open the app's folder we can start application in **development** mode and locally. Command for running application:

`gatsby develop`

The app will be available on ***http://localhost:8000***

### TypeScript support

[How to set up GatsbyJS with TypeScript](How%20to%20set%20up%20GatsbyJS%20with%20TypeScript.md)

### File structure

* **/public** is generated (by Gatsby) *dist* folder that is root of your website. All compiled files are here.
* 
  * **index.html** is entry point of the app in production
* **/src** is a *source* folder with all files for developing the app
* 
  * **/component** is a folder with [React](React.md) components that are construction blocks of your app
* **gatsby-browser.ts**
* **gatsby-config.js** is a configuration file of your app. You can set meta data of the website here. Also you can set plugins here.
* **gatsby-node.ts** 
* **gatsby-ssr.ts** is a configuration file for [Server Side Rendering](Server%20Side%20Rendering.md)

## Creating a new page

* All pages are in ***/src/pages/*** folder as [React](React.md) components
* After build to production pages are in ***/public/page-data*** folder as **json** and others files

⚠️ **Naming of the page-component**

**Gatsby** uses [Reach Router](https://github.com/reach/router) under the hood

1. Name of the file of the new component as a page is the same as **url** to your page on website:

* About.tsx --> localhost:8000/About
* about.tsx --> localhost:8000/about

2. You can't use dash "-" in the name of your page-components

❓ Is it an [issue](https://github.com/gatsbyjs/gatsby/issues/12063) ?

## GraphQL

**Gatsby's** data layer works with [GraphQL](GraphQL.md)

See [Basic example of GraphQL in GatsbyJS](Basic%20example%20of%20GraphQL%20in%20GatsbyJS.md)

### Types of Queries

* **Static Query**:
  * could be used anywhere
  * does not support [Creating a post page in GatsbyJS > Context](Creating%20a%20post%20page%20in%20GatsbyJS.md#context)
  * does not working with [Basic example of GraphQL in GatsbyJS > Query with parameters](Basic%20example%20of%20GraphQL%20in%20GatsbyJS.md#query-with-parameters)
* **Page Query** ([Creating a post page in GatsbyJS > Page Query](Creating%20a%20post%20page%20in%20GatsbyJS.md#page-query)):
  * could be use only on pages
  * supports [Creating a post page in GatsbyJS > Context](Creating%20a%20post%20page%20in%20GatsbyJS.md#context)
  * is working with [Basic example of GraphQL in GatsbyJS > Query with parameters](Basic%20example%20of%20GraphQL%20in%20GatsbyJS.md#query-with-parameters)

## List of posts in the app

In **Gatsby** you can easily add functionality of posts using specific plugin and [GraphQL](GraphQL.md) 

[GatsbyJS - Adding the display of posts list in the app](GatsbyJS%20-%20Adding%20the%20display%20of%20posts%20list%20in%20the%20app.md)

## Markdown Metadata

You can set some parameters in [markdown](markdown.md) metadata. For example:

````
---
slug: "/blog/my-first-post"
date: "2019-05-04"
title: "My first blog post"
---
````

* **slug** is path to your post on the website

## Page of the post

Let's see [Creating a post page in GatsbyJS](Creating%20a%20post%20page%20in%20GatsbyJS.md)

## images

[Working with images in GatsbyJS](Working%20with%20images%20in%20GatsbyJS.md)

## Build and deploy

build the website:

````
npm run build
````

it generates all files for the website in folder **/public**

for running this generated site locally you can use:

````
npm run serve
````

### Netlify

All instructions you can find on the [website](https://www.netlify.com/)

Also check out [gatsby-plugin-netlify (gatsbyjs.com)](https://www.gatsbyjs.com/plugins/gatsby-plugin-netlify/)

## Generate sitemap for the website

Sitemap is needed file for SEO

Just use [gatsby-plugin-sitemap](https://www.gatsbyjs.com/plugins/gatsby-plugin-sitemap/)

installation:

````
npm install gatsby-plugin-sitemap
````

in **gatsby-config.js**

````
siteMetadata: {
  siteUrl: `https://www.example.com`,
},
plugins: [`gatsby-plugin-sitemap`]
````

For checking result just open \<your_domain\>/sitemap.xml - this file should exist

## Creating the Gatsby Theme (with yarn workspaces)

### Project structure

Create project's folder:

````
mkdir gatsby-theme-sample && cd gatsby-theme-sample
````

Initial project using [Yarn](Yarn.md):

````
yarn init
````

In **package.json**:

````json
{

	"private": true,
	"workspaces": [
		"packages/*",
		"sites/*"
	]

}
````

Create workspaces' folders:

````
mkdir packages sites
````

In **packages** folder create folder for your **gatsby theme** and initial [Yarn](Yarn.md) project there:

````
mkdir gatsby-theme-custom && cd gatsby-theme-custom
yarn init
````

 > 
 > private: true

Then, let's create folder for the website that will use our **gatsby theme** and also initial [Yarn](Yarn.md) project:

````
cd ../../sites
mkdir theme-custom-dev && cd theme-custom-dev
yarn init
````

### Installing dependencies

Let's see that [Yarn](Yarn.md) has information about our workspaces:

````
yarn workspaces info
````

Should return something like:

````json
{
  "gatsby-theme-custom": {
    "location": "packages/gatsby-theme-custom",
    "workspaceDependencies": [],
    "mismatchedWorkspaceDependencies": []
  },
  "theme-custom-dev": {
    "location": "sites/theme-custom-dev",
    "workspaceDependencies": [],
    "mismatchedWorkspaceDependencies": []
  }
}
````

Install dependencies for website **theme-custom-dev**:

````
yarn workspace theme-custom-dev add gatsby react react-dom
````

Now you have to install workspace **gatsby-theme-custom** as a dependency. If you try to install it as other dependencies like react, gatsby, etc you will get error 'cause [Yarn](Yarn.md) will try to find your dependency on *npmjs registry*

You should use this command for installing **gatsby-theme-custom** as dependency:

````
yarn workspace theme-custom-dev add "gatsby-theme-custom@*"
````

Create **gatsby-config.js** file in website's folder and put list of plugins that includes **gatsby-theme-custom**:

````js
module.exports = {
	plugins: ['gatsby-theme-custom'],
};
````

Add *scripts* section in *package.json* of website **theme-custom-dev**:

````
{
	"scripts": {
		"develop": "gatsby develop"
	}
}
````

Now you can run your website locally in development mode by this command:

````
yarn workspace theme-custom-dev develop
````

## Gatsby Starters

List of Gatsby Starters on [gatsbyjs.com/starters](https://www.gatsbyjs.com/starters)

### Create new app based on existing starter

````
gatsby new <app-name> https://github.com/<existing-starter-repo>
````

Example (gatsby-starter-blog):

````
gatsby new my-gatsby-project https://github.com/gatsbyjs/gatsby-starter-blog
````

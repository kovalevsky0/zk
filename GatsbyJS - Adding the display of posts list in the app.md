---
title: "GatsbyJS - Adding the display of post's list in the app"
tags:
  - gatsbyjs
  - javascript
public: true
date: 2020-12-16
---

# GatsbyJS - Adding the display of post's list in the app

## gatsby-source-filesystem (plugin)

This plugin provides the access to app's file-system. It adds specific **nodes** in [GraphQL](GraphQL.md) scheme of the app 

### Installation

Install [gatsby-source-filesystem](https://www.gatsbyjs.com/plugins/gatsby-source-filesystem) in your [GatsbyJS](GatsbyJS.md) app

1. `npm install gatsby-source-filesystem`
1. set up plugin in **gatsby-config.js**

````ts
// gatsby-config.js
{
...
	plugins: [
		...
		{
		  resolve: `gatsby-source-filesystem`,
		  options: {
			path: `${__dirname}/src/posts`,
			name: "posts",
		  },
		},
	]
}
````

### Create the post

Create [markdown](markdown.md) post in ***/src/posts***, for example **greetings.md**

### GraphQL query of the plugin

[gatsby-source-filesystem - How to query (gatsbyjs.com)](https://www.gatsbyjs.com/plugins/gatsby-source-filesystem#how-to-query)

Open the **GraphQL Editor** on *http://localhost:8000/\_\_graphql* 

Example

````graphql
{
  allFile {
    edges {
      node {
        extension
        name
      }
    }
  }
}
````

Response

````json
{
  "data": {
    "allFile": {
      "edges": [
        ...
        {
          "node": {
            "extension": "md",
            "name": "greetings"
          }
        }
      ]
    }
  },
  "extensions": {}
}
````

## gatsby-transformer-remark (plugin)

This plugin is a [markdown](markdown.md) parser

### Installation

npm:

````
npm install gatsby-transformer-remark
````

yarn:

````
yarn add gatsby-transformer-remark
````

In ***gatsby-config.js***:

````ts
{
	plugins: [
		...
		`gatsby-transformer-remark`,
	]
}
````

After re-run the app you can use new node **allMarkdownRemark** in [GraphQL](GraphQL.md) **editor** that is provided by the plugin

### GraphQL query of the plugin

***Query***

````graphql
{
  allMarkdownRemark {
    totalCount
    edges {
      node {
        html
        excerpt
		frontmatter {
          title
          slug
          date(formatString: "DD-MM-YYYY")
        }
      }
    }
  }
}
````

* **totalCount** is the count of [markdown](markdown.md) files in the app
* **edges**
  * **node** is each node (file)
    * **html** is html layout of the file
    * **except** the first text of the post's content with limit 
  * **frontmatter**
    * **title** is the parameter that you set in the [GatsbyJS > Markdown Metadata](GatsbyJS.md#markdown-metadata)

***Response***

````json
{
  "data": {
    "allMarkdownRemark": {
      "totalCount": 1,
      "edges": [
        {
          "node": {
            "html": "<h1>Greetings!</h1>",
            "excerpt": "Greetings!",
			"title": "My First Post",
            "slug": "/greetings-post",
            "date": "16-12-2020"
          }
        }
      ]
    }
  },
  "extensions": {}
}
````

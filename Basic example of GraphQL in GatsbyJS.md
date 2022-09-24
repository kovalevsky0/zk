---
title: Basic example of GraphQL in GatsbyJS
tags:
  - gatsbyjs
  - javascript
  - graphql
public: true
date: 2020-12-16
---

# Basic example of GraphQL in GatsbyJS

[GraphQL](GraphQL.md) **editor** is available in your local [GatsbyJS](GatsbyJS.md) app on *http://localhost:8000/\_\_graphql*

In the editor you can write a query and you will get information from API's that based on this written query

Example

````graphql
{
  site {
    
  }
}
````

**site** is like an object/schema that contains some information about your website. For example, it contains information about **siteMetadata** (that you configured in **gatsby-config.js** file)

**sideMetadata** is an object and has three fields **title**, **description**, **author**. 

So, if you wanna get information about **siteMetadata** (specifically - **title**) from API you should write query like this:

````graphql
{
  site {
    siteMetadata {
      title
    }
  }
}
````

And [GraphQL](GraphQL.md) **editor** should return response:

````json
{
  "data": {
    "site": {
      "siteMetadata": {
        "title": "Gatsby Blog Sample"
      }
    }
  },
  "extensions": {}
}
````

You can find the same code example in default component **Layout** of generated (by [GatsbyJS > Gatsby CLI](GatsbyJS.md#gatsby-cli)) app:

````ts
const Layout = ({ children }: { children: React.ReactNode }) => {
  const data = useStaticQuery(graphql`
    query SiteTitleQuery {
      site {
        siteMetadata {
          title
        }
      }
    }
  `)
  // ...
````

## Query with parameters

There is parameter **frontmatter** that includes **slug**

This query should return data of the post that has **slug** = "/greetings-post"

````graphql
query BlogPost {
  markdownRemark(frontmatter: {
    slug: {
      eq: "/greetings-post"
    }
  }) {
    html
    frontmatter {
      title
      date
    }
  }
}
````

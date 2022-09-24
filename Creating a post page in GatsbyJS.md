---
title: "Creating a post's page in GatsbyJS"
tags:
  - gatsbyjs
  - javascript
public: true
date: 2020-12-17
---

# Creating a post's page in GatsbyJS

## Post page component

Create a component for post page (temporary with basic jsx w/o any data)

````tsx
import React from "react"

import Layout from "./Layout"

const PostLayout = () => {
  return (
    <Layout>
      <h1>Some Post</h1>
    </Layout>
  )
}

export default PostLayout
````

## gatsby-node.ts

In **gatsby-node.ts** you should use **createPages** [GatsbyJS](GatsbyJS.md) method and set path to this component

````ts
import { GatsbyNode } from "gatsby"
import path from "path"
import { AllMarkdownRemark } from "./src/types"

const createPages: GatsbyNode["createPages"] = async ({ graphql, actions }) => {
  const { createPage } = actions

  const results = await graphql<
    AllMarkdownRemark<{
      frontmatter: {
        slug: string
      }
    }>
  >(`
    {
      allMarkdownRemark {
        edges {
          node {
            frontmatter {
              slug
            }
          }
        }
      }
    }
  `)

  results.data?.allMarkdownRemark.edges.forEach(
    ({
      node: {
        frontmatter: { slug },
      },
    }) => {
      createPage({
        path: `/posts${slug}`,
        component: path.resolve("./src/components/PostLayout.tsx"),
        context: {
          slug,
        },
      })
    }
  )
}

exports.createPages = createPages
````

## Context

In **gatsby-node.ts**:

````ts

...
	createPage({
        path: `/posts${slug}`,
        component: path.resolve("./src/components/PostLayout.tsx"),
        context: {
          slug, // here is context
        },
	})
	  ...
````

This field **slug** you can use in [Creating a post page in GatsbyJS > Page Query](Creating%20a%20post%20page%20in%20GatsbyJS.md#page-query) as the parameter of the query:

````ts
export const query = graphql`
  query BlogPostQuery($slug: String!) { // here is the field from context
    markdownRemark(frontmatter: { slug: { eq: $slug } }) {
      html
      frontmatter {
        title
        date
      }
    }
  }
`
````

Syntax:

**$slug: String!**

* $slug is the parameter name from context. it should be the same as in **gatsby-node.ts**
* String is type of value
* **!** it means that the parameter **is required**

 > 
 > This parameter is needed to dynamically display the content with $slug = slug (where slug is coming from url "localhost/posts/{slug}") in **PostLayout**

## Page Query

In the component **PostLayout** put [GraphQL](GraphQL.md) query:

````ts
export const query = graphql`
  query BlogPost {
    markdownRemark(frontmatter: { slug: { eq: "/greetings-post" } }) {
      html
      frontmatter {
        title
        date
      }
    }
  }
`
````

It should return data of the post with **slug** = "/greetings-post" and put it into ⚠️ **props of the component**:

````ts

type Props {
  data: {
    markdownRemark: {
      html: string;
      frontmatter: {
        title: string;
        date: string;
      }
    }
  }
}

const PostLayout = ({data}: Props) => {
  return (
    <Layout>
      <h1>Some Post</h1>
    </Layout>
  )
}
````

## Display html in the post component

Using **dangerouslySetInnerHTML**

 > 
 > 🚨  I don't like this method of rendering html. Maybe there is some other solution.

````ts
const PostLayout = ({ data: { markdownRemark } }: PostLayoutProps) => {
	...
      <div
        dangerouslySetInnerHTML={{
          __html: markdownRemark.html,
        }}
      ></div>
	...
}
````

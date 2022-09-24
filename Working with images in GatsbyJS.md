---
title: Working with images in GatsbyJS
tags:
  - gatsbyjs
public: true
date: 2020-12-22
---

# Working with images in [GatsbyJS](GatsbyJS.md)

install npm deps:

* [gatsby-image](https://www.gatsbyjs.com/plugins/gatsby-image/?=gatsby-image#gatsby-image)
* [gatsby-plugin-sharp](https://www.gatsbyjs.com/plugins/gatsby-plugin-sharp/)
* [gatsby-transformer-sharp](https://www.gatsbyjs.com/plugins/gatsby-transformer-sharp/?=sharp)

````sh
npm install --save gatsby-transformer-sharp gatsby-plugin-sharp gatsby-image
````

In **gatsby-config.js**

````js
  plugins: [
  	// ...
    `gatsby-plugin-sharp`,
    `gatsby-transformer-sharp`,
	// ...
	{
      resolve: `gatsby-source-filesystem`,
      options: {
        path: `${__dirname}/src/images`,
        name: "images",
      },
    },
	// ...
  ]
````

### Display photo

In **Layout.tsx**

````ts
import Img from "gatsby-image"
// ...

const Layout = ({ children }: { children: React.ReactNode }) => {
  // ...
  const data = useStaticQuery(graphql`
    query SiteTitleQuery {
      file(relativePath: { regex: "/bg-post/" }) {
        childImageSharp {
          fluid(maxWidth: 1000) {
            ...GatsbyImageSharpFluid
          }
        }
      }
    }
  `)
  
  return (
  	<>
	  <Img fluid={data.file.childImageSharp.fluid}></Img>
	</>
  )
````

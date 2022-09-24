---
date updated: "2021-01-07T05:14:20+03:00"
tags:
  - gatsbyjs
  - javascript
title: Creating GatsbyJS theme
public: true
date: 2021-01-07
---

# Creating GatsbyJS theme

Note about how to make theme for [GatsbyJS](GatsbyJS.md)

make folder

````
    mkdir gatsby-<name>-theme-workspace && cd <gatsby-your-new-theme>
````

initialize [npm](npm.md)/[Yarn](Yarn.md) new project

````
    yarn init
````

in *package.json*:

````json
{
	...
	"private": true,
	"workspaces": [
		"example", // example of the gatsby website that will use theme
		"gatsby-<name>-theme" // folder where is your theme is located
	]
}
````

setup theme folder

````
    cd gatsby-<name>-theme
    yarn init
````

setup example website folder

````
    cd ../example
    yarn init
````

 > 
 > private: true

check **yarn workspaces**:

````
    yarn workspaces info
````

should return smth like:

````json
{
  "example": {
    "location": "example",
    "workspaceDependencies": [],
    "mismatchedWorkspaceDependencies": []
  },
  "gatsby-<name>-theme": {
    "location": "gatsby-<name>-theme",
    "workspaceDependencies": [],
    "mismatchedWorkspaceDependencies": []
  }
}
````

add dependencies in **example**:

````
    yarn workspace example add gatsby
````

add package of our theme as a dependency:

````
    yarn workspace example add "gatsby-craeft-dev-theme@*"
````

add **scripts** section in **example/package.json**:

````json
{
	"scripts": {
		"develop": "gatsby develop",
		"build": "gatsby build"
	}
}
````

install *gatsby, react, react-dom* in our theme as a dev and [peer](https://classic.yarnpkg.com/en/docs/dependency-types#toc-peer-dependencies) dependency:

````
    yarn workspace gatsby-craeft-dev-theme add gatsby react react-dom -D
    yarn workspace gatsby-craeft-dev-theme add gatsby react react-dom -P
````

and add *react, react-dom* in **example** as dependencies:

````
    yarn workspace example add react react-dom
````

### mdx

* [MDX and Gatsby (mdxjs.com)](https://mdxjs.com/getting-started/gatsby)
* [Add components to content using MDX (gatsbyjs.com)](https://gatsbyjs.com/docs/how-to/routing/mdx/)

### create and set layout component

**layout** component is a component that wraps all your content (pages, posts)

you can create this component in **src/components/layout.tsx**

you need to set path to this component in **gatsby-config.js**:

````json
{
	resolve: \`gatsby-plugin-mdx\`,
	options: {
		defaultLayouts: {
			default: require.resolve("./src/components/layout.tsx"),
		},
	},
},
````

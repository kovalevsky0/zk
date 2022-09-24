---
title: How to set up GatsbyJS with TypeScript
tags:
  - javascript
  - gatsbyjs
  - typescript
public: true
date: 2020-12-17
---

# How to set up GatsbyJS with TypeScript

### First things

Install [TypeScript](TypeScript.md) as dependency:

* npm:

````
npm i typescript --save-dev
````

* yarn

````
yarn add typescript -D
````

After installing, you should initialize [TypeScript](TypeScript.md) project by generating **tsconfig.json** file

````
tsc --init
````

Because in [GatsbyJS](GatsbyJS.md) you are using [React](React.md) components that includes **JSX** you should set specific parameter in **tsconfig.json**:

````json
{
  	"compilerOptions": {
  		....
	    "jsx": "react",
	}
}
````

If you want to use [TypeScript](TypeScript.md) for your *src/* files you can easily just rename all files in *src/* from **.js** to **.ts** or **.tsx**. Gatsby support TypeScript out of the box.

### TypeScript for gatsby-\* files

If you want to write files *gatsby-browser.js*, *gatsby-node.js*, and others using [TypeScript](TypeScript.md) you should install and setup plugin [gatsby-plugin-ts-config](https://www.gatsbyjs.com/plugins/gatsby-plugin-ts-config)

**Steps:**

Installation:

* npm

`npm install -D gatsby-plugin-ts-config`

* yarn

````
yarn add gatsby-plugin-ts-config -D
````

Add *gatsby-plugin-ts-config* into *gatsby-config.js*

````js
module.exports = {
	plugins: [
	  `gatsby-plugin-ts-config`,
	]
}
````

### Using types in gatsby-node.ts

[Read more here (GitHub Gist)](https://gist.github.com/clarkdave/53cc050fa58d9a70418f8a76982dd6c8)

````ts
import { GatsbyNode } from "gatsby"

const createPages: GatsbyNode["createPages"] = ({ graphql, actions }) => {
	// something important
}

exports.createPages = createPages
````

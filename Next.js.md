---
title: Next.js
tags:
  - javascript
  - react
  - nextjs
public: true
type: note
date: 2021-06-27
---

# Next.js

Next.js is a [React](React.md) full stack framework. It provides an ability to develop applications with server-side rendering and also can be used as a [Static Site Generator](Static%20Site%20Generator.md).

## Creating a Next.js application (with TypeScript)

For creating a Next.js application you need install [Node](Node.md) and [npm](npm.md) on your machine. if you didn't do it before check out the post [How To Install or Update Node by Using nvm (Node Version Manager)](https://mkvl.me/blog/article/how-to-install-or-update-node-by-using-nvm).

**Next.js** provides a special tool called **create-next-app** that creates a boilerplate code and configuration for new app. It is similar to [create-react-app](create-react-app.md) but for **Next.js** applications.

To create a new app let's use [npx](npx.md):

```bash
npx create-next-app --ts
```

or [Yarn](Yarn.md):

```bash
yarn create next-app --typescript
```

The flags **--ts** and **--typescript** means that a new **Next.js** app will be created with [TypeScript](TypeScript.md) integration (all basic components and files will be created as **.ts** and **.tsx**). You can skip this flag if you don't use TypeScript but in this post series I'll use it for sure.

The tool will ask you about name of your new project. I called it "nextjs-blog-sample" but you can name it whatever you want. The name of the folder will be the same as the name of the project you called.

Open the folder of project:

```bash
cd nextjs-blog-sample
```

## File structure

If you look at the file structure of created project you should see something like this:

```
├── README.md
├── next-env.d.ts
├── next.config.js
├── package.json
├── pages
│   ├── _app.tsx
│   ├── api
│   │   └── hello.ts
│   └── index.tsx
├── public
│   ├── favicon.ico
│   └── vercel.svg
├── styles
│   ├── Home.module.css
│   └── globals.css
├── tsconfig.json
└── yarn.lock
```

You might notice that there is no file like _index.html_ in a public folder as you may have seen before in **React** (created by [create-react-app](create-react-app.md)) projects. This page is pre-rendered by **Next.js** on a server. So, basically, when server receives request it dynamically pre-renders the page.

A short intro to folder structure:

- next.config.js - **Next.js** configuration file
- pages/ - a folder that contains files of website's pages
- public/ - here you can put files like fonts or icons
- styles/ - folder with CSS files
- tsconfig.json - TypeScript configuration file

Folder **pages/** already has an index (or home) page of the website - **index.tsx**. If you run application you will see in the browser a content of that file (which is basically React component).

### Running application

To run application just execute the command:

```bash
npm run dev
```

or

```bash
yarn dev
```

It will run a dev server. While this dev server is running all files of source code (like in a folder /pages) compiles from **.tsx** (or **.jsx** if you don't use TypeScript) with some formatting and optimizations into static assets that used as output code of the app. To do that **Next.js** uses [Webpack](Webpack.md) under the hood.

Now, if you run the app, you should see in the browser something like this:

![](/images/grokking-nextjs-getting-started-first-look-app.png)

It is a content of home page that is available as a file **index.tsx** in a folder **/pages**.

### Creating pages

- Create file `/pages/index.tsx` with React component
  - it will be your home page on the website (/)
- Create folder `/pages/blog` and create file `/pages/blog/index.tsx` with React component
  - it will be page `/blog` on the website
  - there should probably be list of blog posts
- Create file `/pages/blog/[slug].tsx`
  - it's a page of specific blog post that has **slug**
  - so it will be available on `/blog/some-blog-post-slug`

An example of React component for page with specific type/interface:

```tsx
import { NextPage } from "next";
import React from "react";

const BlogPost: NextPage = () => {
  return <div>Blog Post</div>;
};

export default BlogPost;
```

### Display image in component

- Create folder `public/images` and put image there
- In the component you should use component `Image` of **Next.js**
  - It optimizes the image on the website

```tsx
import Image from "next/image";
// ...

export const Welcome: FC = () => {
  return (
    // ...
    <Image
      src="/images/avatar.jpeg"
      alt="This is an avatar"
      width={200}
      height={200}
    />
    // ...
  );
};
```

### Creating the main layout component

- Create component `components/MainLayout.tsx`
- Basic template (with **children**):

```tsx
export const MainLayout: FC = ({ children }) => {
  return (
    <>
      <Navigation />
      <main>{children}</main>
    </>
  );
};
```

- Implement this component in `pages/_app.tsx`:

```tsx
// ...
import { MainLayout } from "../components";

// ...

function MyApp({ Component, pageProps }: AppProps) {
  return (
    <MainLayout>
      <Component {...pageProps} />
    </MainLayout>
  );
}
```

### Rendering markdown

To render markdown inside React component use [react-markdown](https://github.com/remarkjs/react-markdown).

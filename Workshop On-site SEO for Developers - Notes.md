---
date: "2021-07-22 02:14"
tags:
  - seo
  - monicalent
  - blogging
  - bloggingfordevs
---

# Workshop On-site SEO for Developers - Notes

Parts of SEO described in this workshop:

- Technical SEO
- On-page SEO

### How Google Works

### Website Architecture

> important resource should be available after no more than 5 clicks

Googlebot:

- Home page
- - /blog
- - /article

### Tips to avoid problem with having more than 5 clicks

- increase page size of pagination
  - show 20-30 posts on one page

### SSR

- Server-side rendering is still needed
- You need one of these techniques:
  - pre-rendering
  - dynamic rendering
  - server-side rendering

### Canonical URLs

- Canonical urls should be on your website
- Need to add canonical url to post's page and **all pages like /blog**
  - `<link rel="canonical" href="https://mkvl.me/blog" data-react-helmet="true">`

### Performance in SEO

- Google renders mobile-first
  - it means that you should definitely optimize website's layout for mobile devices
  - [Google's mobile friendly test tool](https://search.google.com/test/mobile-friendly)

## Site SEO optimizations

- SEO-friendly url
  - of course, **https**
  - avoid subdomain (blog.mkvl.me)
  - subdirectory
  - slug
    - **short** and **sweet**
    - contains **keywords**
    - avoid numbers and dates
- title + h1 tag
  - \<title\>Your title\</title\>
  - \<h1\>Your headline\</h1/>
    - should be similar to each other
    - **powerful place for keywords**
- published date
  - `<time datetime="2020-08-07">August 7, 2020</time>`
- paragraphs
  - use \<p\> instead of \<div\>
  - make the p tag and h2 tag siblings if possible (???)
  - mention keywords
- images
  - it also good for SEO
  - use keywords in the file name
  - place well-made images under relevant subheadings
    - thumbnail for each h2 subtitle?
- subheadings
  - \<h2\>Your headline\<h2\>
    - should describe content of the paragraph below
    - great spot for keywords
    - adds context to internal and external links (??)
    - h2, h3 matter most
- outbound links
  - links to resource of another website
    - no need to "nofollow" unless it's a sponsored/affiliate link
    - open in a new tab
    - no competitor's links
- internal links
  - links to resource of your own website
- common tips
  - use keywords (think like a searcher)

### Google first page ranking

- figure out why these resources rank #1 (researching)
  - On-page optimization?
  - User signals? Search intent?
  - Keyword density?
  - Backlinks?
  - Internal link strategy?

## Free SEO Tools

- **Google Search Console**
  - to monitor clicks vs impressions
  - use it to
    - test improving the CTR of your titles
    - monitoring search performance
- **Ahrefs Webmaster Tools**
  - run Site Audit and pick up common issues like
    - maximum crawl depth
    - redirect chains
    - 404 pages
    - etc
- **Detailed Seo Extension**
  - quick access to web page's:
    - metadata
    - schema
    - page structure
    - internal links
    - external links

## Resources

- [A Basic (Yet Powerful) Technical SEO Audit for Beginners](https://www.youtube.com/watch?v=oJPGa0J6p5Q)
- [Martin Splitt — Technical SEO 101 for web developers](https://www.youtube.com/watch?v=XF08jiOKaiQ)
- [Gatsby SEO: Find and Fix Non-Obvious Mistakes](https://bloggingfordevs.com/gatsby-seo/)
- [Google Search Central](https://developers.google.com/search/)

## Tools for keyword research

- For beginners
  - [Keysearch](https://keysearch.co)
  - [Keywords Everywhere](https://keywordseverywhere.com/)
- For competitive niches
  - [Ahrefs](https://ahrefs.com/)

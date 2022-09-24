---
title: sed (utility)
date: "2021-09-04 02:08"
tags:
  - linux
public: true
---

# sed (utility)

**sed** is an Linux utility or command that can be used to replace string by another string.

## Examples

### Basic syntax

````bash
sed -i 's/old-word/new-word/g' *.md
````

## Troubleshooting

### sed: 1: "something...": invalid command code W

 > 
 > Try adding the `-e` argument explicitly and giving `''` as argument to `-i`:

````bash
sed -i '' -e 's/..\/src\/components/..\/components/g' *.mdx
````

### sed: content: in-place editing only works for regular files

````bash
sed -i '' -e "s/oldstr/newstr/g"  `find .* -type f -maxdepth 0 -print`
````

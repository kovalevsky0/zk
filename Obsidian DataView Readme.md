---
title: Obsidian DataView Readme
date: "2021-07-12 07:57"
tags:
  - obsidian
public: false
---

# Obsidian DataView Readme

Treat your obsidian vault as a database which you can query from. Provides a fully-fledged query language for filtering, sorting, and extracting data from your pages. See the Examples section below for some quick examples, or the full [reference](https://blacksmithgu.github.io/obsidian-dataview/).

## Examples

Show all games in the game folder, sorted by rating, with some metadata:

````
```dataview
table time-played, length, rating
from "games"
sort rating desc
```
````

![Game Example](docs/static/images/game.png)

---

List games which are MOBAs or CRPGs.

````
```dataview
list from #game/moba or #game/crpg
```
````

![Game List](docs/static/images/game-list.png)

---

List all tasks in un-completed projects:

````
```dataview
task from #projects/active
```
````

![Task List](docs/static/images/project-task.png)

---

List all of the files in the `books` folder, sorted by the last time you modifed the file:

````
```dataview
table mtime from "books"
sort mtime desc
```
````

---

List all files which have a date in their title (of the form `yyyy-mm-dd`), and list them by date order.

````
```dataview
list where file.day
sort file.day desc
```
````

# Usage

**Note**: See the full documentation [here](https://blacksmithgu.github.io/obsidian-dataview/).

Dataview allows you to write queries over markdown files which can be filtered by folder, tag, and markdown YAML fields; it can then display the resulting data in various formats. All dataviews are embedded code blocks with the general form:

````
```dataview
[list|table|task] field1, (field2 + field3) as myfield, ..., fieldN
from #tag or "folder" or [[link]] or outgoing([[link]])
where field [>|>=|<|<=|=|&|'|'] [field2|literal value] (and field2 ...) (or field3...)
sort field [ascending|descending|asc|desc] (ascending is implied if not provided)
```
````

The first word in a query is always the view type - currently, either:

* `list`, which just renders a list of files that pass the query filters.
* `table`, which renders files and any selected fields that pass the query filters.
* `task`, which renders all tasks from any files that pass the query filters.

You can query from either `#tags`, from `"folder"`, or from `[[link]]`. You can freely combine these filters into more complicated boolean expressions using `and` and `or`; if precedence is important, use parentheses.

Fields can be any YAML front-matter field (currently, strings, numbers, ISO dates and durations are supported, with support for ratings, links, and intervals forthcoming), any custom defined field (using the `field as field2` syntax). You can obtain fields inside of YAML objects using the `.` operator - i.e., `Dates.Birthday` for example. Fields can also be functions of other fields - for example, `rating + offset`, is a valid field.

#### Field Specifics

All files have the following implicit attributes:

* `file.name`: The file title.
* `file.path`: The full file path.
* `file.size`: The size (in bytes) of the file.
* `file.ctime`: The date that the file was created.
* `file.mtime`: The date that the file was last modified.

If the file has a date inside its title (of form `yyyy-mm-dd`), it also obtains the following attributes:

* `file.day`: The date contained in the file title.

Additionally, all of the fields defined in the YAML front-matter are available for querying. You can query inside nested objects using dot notation (so `dates.birthday` would get the `birthday` object inside the `dates` field). Fields can have the following types:

* `number`: A number like `0` or `18` or `19.37`.
* `date`: A date and time in ISO8601 format - `yyyy-mm-ddThh:mm:ss`. Everything after the year and month is optional, so
  you can just write `yyyy-mm` or `yyyy-mm-dd` or `yyyy-mm-ddThh`. If you want to use a date in a query, use
  `date(<date>)` where `<date>` is either a date, `today`, `tomorrow`, `eom` (end of month), or `eoy` (end of year).
  * You can access date fields like 'years' and so on via dot-notation (i.e., `date(today).year`).
* `duration`: A length of time - can be added/subtracted from dates. Has the format `<number> years/months/.../seconds`,
  where the unit can be years, months, weeks, days, hours, minutes, or seconds. If you want to use a duration in a
  query, use `dur(<duration>)`.
  * You can access duration fields like 'years' and so on via dot-notation (i.e., `dur(<duration>).years`).
* `link`: An obsidian link (in the same format); you can use dot-notation to get fields in the linked file. For example,
  `[[2020-09-20]].file.ctime` would get the creation time of the `2020-09-20` note.
* `array`: A list of elements. Automatically created by YAML lists in the frontmatter; can manually be created using
  `list(elem1, elem2, ...)`.
* `object`: A mapping of name -> value. Automatically created from YAML objects. You can access elements inside an
  object using dot-notation or array notation (`object.field` or `object["field"]`).
* `string`: Generic fallback; if a field is not a more specific type, it is a string, which is just text. To use a string in a query, use quotes - so `"string"`.

## Docs

* [Obsidian DataView Docs](Obsidian%20DataView%20Docs.md)

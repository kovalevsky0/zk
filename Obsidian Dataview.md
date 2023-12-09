---
tags: [obsidian, dataview]
---

------
- date: 2023-08-16 01:39:20
- tags: #obsidian #dataview 
-----

# Obsidian Dataview

## Basics

- DW - Dataview
- write query inside 
```
\```dataview
\```
```

- there are 4 types of DW queries
	- list

## Query syntax

- **LIST|TABLE|TASK|CALENDAR**
- **FROM** - the location of notes you need (folder?)
- **WHERE** - notes with this criteria
- **SORT** - with sorting (asc/desc)
- **GROUP BY** - to group notes (shows them under the subitem/subtitle)
- **LIMIT** - how many notes to show
## Types of query

### List

- to show the list of all notes from the vault
```
LIST
FROM ""
```
- to show the list of all notes from the vault which have a name "Obsidian Dataview"
```
LIST
FROM ""
WHERE file.name = "Obsidian Dataview"
```
```dataview
LIST
FROM ""
WHERE file.name = "Obsidian Dataview"
```

- a list can show only name of the note and one piece of data (for example - creation time):
	- file.ctime - creation time of the file
```
LIST file.ctime
FROM ""
WHERE file.name = "Obsidian Dataview"
```
```dataview
LIST file.ctime
FROM ""
WHERE file.name = "Obsidian Dataview"
```

### Table

^b874d7

- unlike the list, table can shows several pieces of data
- to show the table with "creation time" and "tags" of all notes which have a name "Obsidian Dataview"
```
TABLE file.ctime, tags
FROM ""
WHERE file.name = "Obsidian Dataview"
```

```dataview
TABLE file.ctime, tags
FROM ""
WHERE file.name = "Obsidian Dataview"
```
- to show the table of all notes with tags "obsidian" and "dataview"
```
TABLE
FROM #obsidian and #dataview 
```
```dataview
TABLE
FROM #obsidian and #dataview 
```
- to show the table of 4 notes with tags "obsidian" with sorting by "creation time"
```
TABLE
FROM #obsidian
SORT file.ctime DESC
LIMIT 4
```
```dataview
TABLE file.ctime
FROM #obsidian
SORT file.ctime DESC
LIMIT 4
```
### Task

^50b525
- to show the list of tasks (checkboxes) from the specific note (note "Obsidian Dataview")
```
TASK
FROM ""
WHERE file.name = "Obsidian Dataview"
```
```dataview
TASK
FROM ""
WHERE file.name = "Obsidian Dataview"
```
- to show the list of **uncompleted** tasks (checkboxes) from the specific note
```
TASK
FROM ""
WHERE file.name = "Obsidian Dataview" and !completed
```
```dataview
TASK
FROM ""
WHERE file.name = "Obsidian Dataview" and !completed
```
- to show the list of tasks (checkboxes) from the specific note **under the link to the note where they are from**
```
TASK
FROM ""
WHERE file.name = "Obsidian Dataview"
GROUP BY file.link
```
```dataview
TASK
FROM ""
WHERE file.name = "Obsidian Dataview"
GROUP BY file.link
```

### Calendar

- to show the calendar with notes that were modified (dates of then notes were modified)
```
CALENDAR file.mtime
FROM ""
```
```dataview
CALENDAR file.mtime
FROM ""
```

## DEMO

- All stuff below is needed to demonstrate Dataview functionality; Most of the DW queries get this note itself;
- Tasks here are needed to show tasks in [[#^50b525|the example with tasks]]

- [ ] Task 1
- [x] Task 2
- [ ] Task 3


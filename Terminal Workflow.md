# Terminal Workflow

## utilities

* imgcat
  * shows images in terminal

## fzf and vim

to search file and open it in vim:

````bash
vim -o `fzf`
````

### file preview (bat)

````bash
fzf --preview 'bat --style numbers,changes --color=always {} | head -500'
````

The command line is a user interface in the form of lines of text, that is navigated by typing commands instead of using a mouse.

# Basic Unix Commands

- `ls` - (List Files and Directories) show files & folders in current directory
    - can list a different directory by passing that path e.g. `ls /Users/local/`
    - `ls -a` - list all files including "hidden" files
    - `ls -l` - display more info / detailed
        - `ls -lh` - same as `-l` but human readable file sizes
        - `-S` - sort by size
        - `-t` - sort by last modified time
        - `-r` - reverse sort (can be combined with the others)
- `touch test.txt` - create file `test.txt`
    - can create more than one file at once 
        - e.g. `touch index.html script.js style.css`
- `pwd` - (Present Working Directory) prints name of current working directory
- `cd` - (Change Directories) navigate to path specified
    - `cd ..` - up a path
- `mkdir` - (Make Directory) create directory
- `cp` - (Copy) copy file or directory
    - e.g. `cp a.txt b.txt` - create new file `b.txt` copied from `a.txt`
    - multiple files can be copied `cp a.txt b.txt foo`
        - note the last argument must be the target directory to copy files to
    - copy directory can be done by using `-R` flag
        - e.g. `cp -R foo bar`
    - `-f` - force override
    - `-i` - force override confirm 
- `rm` - (Remove) Deleting files
- `mv` - (Move) Moving files - combination of `cp` and `rm`

- Input / Output (`|`, `>`)
    - Redirecting Output (`|`) to chain together a number of commands
        - e.g. `ls -a ~ | grep _` - output of `ls` "pipe"d to the input of the `grep` command
    - Writing to a file (`>`)
        - e.g. `ls -a ~ | grep _ > underscores.txt` writes the output of the `grep` command  to `underscores.txt`
    - Reading from a file (`<`)

# Tips

## Copying and Pasting

- Copy is `CTRL + SHIFT + C`
- Paste is `CTRL + SHIFT + V`

## Tab Completion

- hit `Tab` when writing out certain commands to autocomplete
    - e.g `cd folder/` then cycle tab to get next folder you want

## .

- `.` is a handy shortcut for opening eveything within a project directory
    - e.g. `git add .` is commonly used with `Git`
    - VSCode command `code .` can open up all project files in a project directory
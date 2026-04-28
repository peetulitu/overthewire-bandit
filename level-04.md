## Objective
The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed
up, try the “reset” command.

## Approach
This level was a bit trickier, here I needed to find a file that matched a series of parameters, to do so I used the `find` command
with the flags that it required and piped the results through `xargs` and `grep` helping narrow down the search results.

The command chain works like this:
- `find ./inhere -type f` -> Will find all files not directories under the provided path.
- `| xargs file` -> Takes each file from find and runs the file command on them identifying the file type (ASCII text, binary data...)
- `| grep text` -> Filters the output to lines that contain "text".

## Commands Used
```bash
find ./inhere -type f | xargs file | grep text
cat /home/bandit4/inhere/-file07
```

## Takeaway
In this level I learned how to use the `find` command specifying the directory I want to search and the type of file I was looking 
for. I also learned `xargs` and `grep`, two commands that proved to be very useful when looking for something very specific. 
The last thing I learned was how to pipe output through other commands kind of like a funnel narrowing down on the end goal.

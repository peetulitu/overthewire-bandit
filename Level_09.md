## Objective
The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ 
characters.

## Approach
This time I had to find a human readable string that is preceded by a certain character. Here I went back to `grep` and `strings`
this time I looked into the man pages of `grep` and `strings` in order to find a way to extract an unknown number of `=` from the file.

Command breakdown:
- `strings data.txt` -> Extracts all human readable strings.
- `| grep "^="` -> Filters for lines with `=` and `^` is a regex anchor which means that the line start here.

## Commands Used
```bash
strings data.txt | grep "^="
```

## Takeaway
This level was very useful for me to understand the capabilities of `grep` and how it is able to find data based on a hint, not the
full string I am looking for. More importantly I learned a new command in this level and I learned what regex anchors are.

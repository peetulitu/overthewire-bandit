## Objective
The password for the next level is stored in the file data.txt and is the only line of text that occurs only once.

## Approach
This time I need to find a line of text that is only shown once, to do this I used `sort` and `uniq`. Each one of these
commands helps narrow down all the extra lines that are in the file.

Command breakdown:
- `sort data.txt` -> Will organize the lines in the file.
- `uniq -u` -> Only shows the line that is unique(the one that appears only once)

## Commands Used
```bash
sort data.txt | uniq -u
```

## Takeaway
This level was useful to learn new commands, `sort` and `uniq`. I also learned the quirks of `uniq`, because it will only work
properly when the output is sorted I had to first use `sort` and then pipe that output to `uniq`. This is because `uniq` only
compares lines that are adjacent to one another, before I realized this I would receive output with repeated lines as in the 
'data.txt' file they were not adjacent.

## Objective
The password for the next level is stored in the file data.txt next to the word millionth.

## Approach
In this level I need to find a combination of characters I don't know somewhere in a large file, but I know that next to those
characters the word millionth is written. To find this I had to focus on finding the word millionth and as I learned before the
best way to do that is to use the `grep` command. `grep` lets me look for a specific pattern within a file.

## Commands Used
```bash
cat data.txt | grep "millionth"
```

## Takeaway
When I completed this level I thought that `grep` could only be used when piping results, if I was to do this again I would simply use 
`grep` as it would make the command simple and much easier to read:
```bash
grep "millionth" data.txt
```
I also learned that what I did to solve this has a name, 'Useless Use of Cat'(UUOC) it was pretty funny to learn that.

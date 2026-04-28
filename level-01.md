## Objective
The password for the next level is stored in a file called - located in the home directory.

## Approach
This second level was quite similar to the first one, I used `ls` to get a look at the home directory, then used `cat` to read the
file's contents. Because the file was named `-` I couldn't just `cat` the file directly, so I wrote the full path to the file.

## Commands Used
```bash
cat /home/bandit1/-
```

## Takeaway
This level taught me how to interact with different file names, because `-` is interpreted as `stdin` in many Unix commands, I need to find workarounds. Although my method worked, I could have prefixed the file name with `./` simplifying the command.
```bash
cat ./-
```

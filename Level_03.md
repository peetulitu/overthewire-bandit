## Objective
The password for the next level is stored in a hidden file in the inhere directory.

## Approach
In this level I encountered my first hidden file. To find this file I can't `ls` the mentioned directory as nothing would appear,
to get around this issue I will use the `-al` flag with the `ls` command, this flag will allow me to see all the files in a directory
including the hidden ones. Once I have identified the file I can use the `cat` command to read it.

## Commands Used
```bash
ls -al inhere
cat inhere/...Hiding-From-You
```

## Takeaway
This level taught me what flags are and how they make the commands I know more powerful and more useful, as well as introducing
hidden files, how any filename that starts with a `.` will be hidden. I also learned about combining flags, even though this level only needs the `-a` flag which shows all files, the `-l` flag shows the permissions, owners, size and date of each file in the directory. 

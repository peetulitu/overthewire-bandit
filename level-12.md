## Objective
The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this 
level, it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard-to-guess directory name. Or better, use 
the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!).

## Approach
This level was quite confusing the first couple of times I was doing it. As it says in the description, the file has been compressed many times, 
but it required many more rounds of decompression than I was expecting. For this level, I used 6 new commands: `xxd`, `cp`, `gunzip`, `mv`, 
`bunzip2`, and `tar`. I also used the `file` command that I learned in previous levels. To solve this level, I started by using `xxd -r` 
Because the file initially just looks like a hex dump, it is hard to see that it is compressed. I used `xxd` to convert it back to binary. 
Then I continuously repeated the same process: decompressed the file using the proper tool, ran the `file` command, and decompressed again 
According to the compression format that `file` reported.

## Commands Used
```bash
xxd -r data.txt > binary.txt
cp binary.txt output.gz
gunzip output.gz
file output.gz
mv output output.bz2 && bunzip2 output.bz2
mv output output.gz && gunzip output.gz
```
Repeat until `file` reports ASCII text.

## Takeaway
This level taught me about binary files, compressing and archiving files, as well as the different types of compression methods that exist.

## Objective
The password for the next level is stored in a file called --spaces in this filename-- located in the home directory.

## Approach
This challenge focused on a different issue from the previous one, here we need to read a filename that has spaces in it.
When there are spaces in the name we can use the \ to specify that the following space is part of the filename, not a separator. 
By default Unix interprets spaces as argument separators.

## Commands Used
```bash
cat /home/bandit2/--spaces\ in\ this\ filename--
```

## Takeaway
Understanding how to deal with different issues that Unix poses to be able to navigate and read files efficiently is very 
important.

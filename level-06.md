## Objective
The password for the next level is stored somewhere on the server and has all of the following properties:
- Owned by user bandit7
- Owned by group bandit6
- 33 bytes in size

## Approach
This level used the same format as the previous one, but here I needed to look at the files owners and its permissions, to do so I 
checked the `find` man page looking for flags that would help me find a files ownership and permissions.

## Commands Used
```bash
find / -type f -size 33c -group bandit6 -user bandit7 2>/dev/null
```

## Takeaway
This level was a great introduction to ownership, permissions, users and groups in Linux, understanding what their role is and how 
they interact with the filesystem. I also learned what `2>/dev/null` is and how to use it to receive a clean output by suppressing 
any errors.

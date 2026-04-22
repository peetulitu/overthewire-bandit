## Objective
The password for the next level is stored in a file called readme located in the home directory. Use this password to log into 
bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

## Approach
The approach to this level was quite straight forward, because we know the name of the file and the directory where it is located, 
I used the cat command to read the files contents.

## Command Used
```bash
cat ~/readme
```
  
## Takeaway
This level shows the basics of navigation and reading files directly from the terminal, learning to use the ls and cat commands 
learning that ~ is the home directory of the current user. And learning to connect via SSH for the first time.

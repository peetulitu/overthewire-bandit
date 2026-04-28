## Objective
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.
NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, 
try executing it to see the debug information it prints.

## Approach
I will start by doing the exact steps I started with in the previous level, looking at the `/etc/cron.d` directory, and running `cat` on the cronjob to understand its functionality and how I can
exploit it. Taking a look at the script.
```bash
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```
I can see that it first specifies the interpreter with the shebang `#!/bin/bash`, then it will save the output of `whoami` to a variable called `myname` in `myname=$(whoami)`, next it creates another
variable called `mytarget` and sets it to the `md5` hash of ` I am user $myname`, I don't know what the last part of this command is doing, `cut -d ' ' -f 1`, so I'll look into it before I continue,
so after doing some research, I found that what this does is remove specific sections from each line of the file. The `-d` flag will use a delimiter instead of TAB, and the `-f` flag will select only
the specified fields because `md5` will output the result with a space at the end and the filename. The command uses `cut` to separate the string right at the space and keep only the beginning, which
is the hash. The last part of the script will `cat` the contents of `/etc/bandit_pass/$myname` and output them in the file `/tmp/$mytarget`.
From reading this file, I understand that if I were to execute it as bandit22, it would pass my username through `md5`, then use that hash as the name for the file where it would copy my password to.
Since the file is executed every minute, like the one in the previous level,
```bash
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
```
I know that this file with bandit23's password has already been created, so to find it, I need to get the `md5` hash of the string bandit23 by running the same command that is run in the cronjob, 
substituting $myname with bandit23. Now that I have the `md5` hash, I can `cat` the file in the `tmp` directory and get the password.

## Commands Used
```bash
ls /etc/cron.d
cat /etc/cron.d/cronjob_bandit23
cat /usr/bin/cronjob_bandit23.sh
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```

## Takeaway
Being able to read and understand the scripts that I find is clearly an indispensable skill, and this level truly showed me how important it is. If I weren't able to identify that it was creating an
`md5` hash of the full string, not only the user, I would not be able to complete the level. It also serves as another reminder that being careful with who has permission to read your files is very 
important.

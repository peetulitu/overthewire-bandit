## Objective
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

## Approach
I will start by using `ls` to see all the files in the `cron.d` directory. Here, there is a cronjob called `cronjob_bandit22.sh`. I can `cat` this to see what it is doing and when it executes.
```bash
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```
This file first specifies the interpreter it will use via the shebang `#!/bin/bash` as the bandit22 user, then it allows all users to read the `tmp` file with 
`chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`, and finally it will cat the contents of `bandit22` but pass the output to the `tmp` file with the last command 
`cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`. If we look for the `tmp` file, we should be able to `cat` it and get the password to the following level. This works because 
although we do not have permission to execute this script, it is set as a cronjob, and looking at the results of `cat /etc/cron.d/cronjob_bandit22` we see that it is set to execute at every minute, 
always. This means that the necessary files will already be created and set in the `tmp` directory for us to find.

## Commands Used
```bash
ls /etc/cron.d
cat /etc/cron.d/cronjob_bandit22
cat /usr/bin/cronjob_bandit22.sh
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

## Takeaway
This challenge is helping lay down the foundations of what cronjobs are and how they are used to gain access to other accounts on the machine or even elevate privileges. It is important to properly
set the permissions in these scripts and be very careful with what they are doing. It also focuses on unnecessary permissions given to users that shouldn't have them. Nobody but `bandit22` should
have read permission to a file containing bandit22's password, so giving `644` permissions to that file is an important security flaw.

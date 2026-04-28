## Objective
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.
NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!
NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

## Approach
Ok, I will first take a look at the running cron job and try to understand what it is doing and how to exploit it.
The complete script:
```bash
#!/bin/bash
shopt -s nullglob
myname=$(whoami)
cd /var/spool/"$myname"/foo || exit 
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." ] && [ "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" "./$i")"
        if [ "${owner}" = "bandit23" ] && [ -f "$i" ]; then
            timeout -s 9 60 "./$i"
        fi
        rm -rf "./$i"
    fi
```
The first line: 
```bash
shopt -s nullglob
```
This is the first line of the script, it is modifying the bash options, the `-s` is enabling the option `nullglob`, this option will stop the binary from running a string that is meant to be a
wildcard as a literal string, for example, not treating `*.txt` as an actual file in the system. Next, we have:
```bash
myname=$(whoami)
cd /var/spool/"$myname"/foo || exit 
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
```
This section of the script will complete three actions: 1. Identify the user in the current session and set that user to the variable `myname`. 2. Travel to the `/var/spool/"$myname"/foo` 
directory and then if that command was to fail, the `|| exit` would terminate the rest of it. 3. Will echo the sentence `Executing and deleting all scripts in /var/spool/$myname/foo:` to 
the terminal. The final part of the script:
```bash
for i in * .*;
do
    if [ "$i" != "." ] && [ "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" "./$i")"
        if [ "${owner}" = "bandit23" ] && [ -f "$i" ]; then
            timeout -s 9 60 "./$i"
        fi
        rm -rf "./$i"
    fi
```
will start a loop where any files, regardless of their name or their type `* .*` will be executed. Breaking down this script further, `if [ "$i" != "." ] && [ "$i" != ".." ];` this is ensuring
that `.` and `..` are not executed to avoid changing directories. `echo "Handling $i"` This will simply let the user that executed the script know what file it is working on. 
`owner="$(stat --format "%U" "./$i")"` This section will use the `stat` command with the `%U` flag to identify the owner of the file and set it to the variable `owner`.
```bash
if [ "${owner}" = "bandit23" ] && [ -f "$i" ]; then
            timeout -s 9 60 "./$i"
```
Here it is checking if the owner is bandit23 and if the item is a regular file; if it is, it will then use the `timeout` command to send a `SIGKILL` 60 seconds after it runs the file.
And the very last line `rm -rf "./$i"` will delete the file.

----------------------------------------------------------------------------------------------------------------------------------------------------

My first has been to create a reverse shell with a `.sh` script and place that in the `/var/spool/bandit24/foo` directory, but it didn't work. I'll take a step back to explore the system and 
make a plan before attempting anything else. 
Examining the situation, I do believe I am on the right track. After checking the permissions for different directories and found the following: 1. I have both write and execute permissions 
to the `/var/spool/bandit24/foo` directory, but not read permissions. 2. There is no `/var/spool/bandit23` directory. Because of this, I believe that the method I need to follow is what I tried
before, creating a script in the `/bandit24/foo` directory and wait for it to be executed through the cron job as bandit24. What I need to figure out now is what script to execute. I know that
a reverse shell with a `nc -lvnp` listener on localhost doesn't work, so I need to figure out something else. I conducted a test to make sure that I can do what I think I can: I created 
`test.txt` in the `/var/spool/bandit24/foo` directory, and then, because I would need bandit24 to be able to execute it, I ran `chmod +x /var/spool/bandit24/foo/test.txt`. Although I cannot
check whether or not this file exists because I don't have read permissions on the directory, I will assume they both worked, as I didn't get any error messages.
I will now try to have the cron execute a script that copies the contents of `/etc/bandit_pass/bandit24` to a `.txt` file in the bandit23 home directory. This didn't work, I think it is because
I tried to create the `.txt` file in the bandit23 home directory, and I do not have permissions to write to this directory. I will try again, but I will create the file in `/tmp`.
Got it! That was the issue; once I copied the output to the `/tmp` directory, it worked.

Looking back at the lab, I see that my initial reverse shell probably failed because of the cron script itself, which is made to kill any scripts that are owned by the bandit23 user after 60 seconds
which means that any shells would shut down after that period of time.

## Commands Used
```bash
cat /etc/cron.d/cronjob_bandit24

ls -la /var/spool/bandit24/foo
ls -la /var/spool/bandit23

ls -la /home/bandit23

vim /var/spool/bandit24/foo/get.sh
chmod +x /var/spool/bandit24/foo/get.sh

cat /tmp/password.txt
```
### Script Used
```bash
#!/bin/bash

cp /etc/bandit_pass/bandit24 /tmp/password.txt
chmod +r /tmp/password.txt
```

## Takeaway
This level was the hardest one yet for me, but the most satisfying to complete. I really learned how to exploit the different permissions here, just because I can read a directory doesn't mean
I can't write or execute. This is a concept that I didn't even consider before this level. Moving forward, I know I will be paying very close attention to every permission on every single file
and directory I encounter. Having to decode a more complex script (the cron job) was very entertaining and a good way to learn about new commands like the `shopt -s nullglob` command, which I 
didn't know of before. It was also interesting to have to create my own script for this challenge. Although it turned out to be very simple, it was a challenge to figure out the commands that
would work to achieve the objective.

## Objective
To gain access to the next level, you should use the setuid binary in the home directory. Execute it without arguments to find out how to use it. The password for this level can be found in the 
usual place (/etc/bandit_pass), after you have used the setuid binary.
## Approach
As the title proposes, I will first run the setuid binary that's in the home directory. This binary is allowing me to execute commands as user bandit20, looking at its permission with `ls -al`, I
can see that it is owned by user `bandit20` and group `bandit19`. This is why I am able to execute it and run it as its owner `bandit20` when a file is owned by a certain user in the system that
file will execute with the owner's permission, if the `s` bit is also set in the permissions, meaning that a simple misconfiguration like this one, where the file is owned by one user but another 
user has the ability to execute it via the group ownership means the attacker is able to execute commands as another user through that file. I will use this binary to `cat` the `/etc/bandit_pass`
file and retrieve the password.
When I run the `ls -al` command on the file, I get this output:
`-rwsr-x---   1 bandit20 bandit19 14888 Apr  3 15:17 bandit20-do`
 From this, there are three parts that stand out the most: first, the `s` in the owner's execute position (SUID bit), the owner is bandit20, and group bandit19 has read+execute permissions.

## Commands Used
```bash
ls -al
./bandit20-do
./bandit20-do whoami
./bandit20-do cat /etc/bandit_pass/bandit20
```
## Takeaway
This lab teaches the very basics of file ownership, understanding how the user permissions interact with files, and, more importantly, how these can be leveraged to elevate privileges within the 
system.

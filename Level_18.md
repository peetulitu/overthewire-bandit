## Objective
The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone 
has modified .bashrc to log you out when you log in with SSH.

## Approach
This level was quite interesting as the `.bashrc` file was modified, I needed to use a different type of 
shell to connect to the machine, so I modified the `ssh` command to use a `sh` shell instead of `bash`, 
once connected, I just had to cat the `readme` file and get the password.

## Commands Used
```bash
ssh bandit18@bandit.labs.overthewire.org -p2220 /bin/sh
```

A few other approaches that would have worked:

```bash
# Run cat directly as the remote command — no shell needed
ssh bandit18@bandit.labs.overthewire.org -p2220 cat readme

# Use bash but skip the startup files
ssh bandit18@bandit.labs.overthewire.org -p2220 -t bash --norc
```

## TAKEAWAY
Every shell has its own set of startup files that it sources when it launches, and for bash, the main ones are 
`.bash_profile` (login shells) and `.bashrc` (interactive non-login shells). If one of those files is 
modified, you have a few options: use a different shell like `/bin/sh` that doesn't read `.bashrc`, tell bash
to skip its `rc` files with `--norc` or `--noprofile`, or avoid an interactive shell altogether by passing a 
command directly to `ssh` (anything after the host/port gets executed on the remote machine).

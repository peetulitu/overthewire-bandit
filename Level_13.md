## Objective
The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get 
the next password, but you get a private SSH key that can be used to log into the next level. Look at the commands that logged you into 
previous bandit levels, and find out how to use the key for this level.

## Approach
To complete this level, I copied the private `RSA` key from the bandit13 home directory. Then, from my own machine, I changed the `private.key` file
permissions to `600`, which only allows the owner of the file to read and write. This is enforced by `SSH` because the private key
is the credential that is used to connect to the server. Once this is set, I can `SSH` into bandit14 and retrieve the password. I use the `-i`
flag to specify that I am using the `private.key` as my credential when accessing the bandit14 user.

## Commands Used
```bash
chmod 600 private.key
ssh -i private.key bandit14@bandit.labs.overthewire.org -p 2220
```

## Takeaway
In this level, I learned that `RSA` keys can be used as credentials and the strict measures `SSH` uses to ensure that only the authorized 
people are logging in to accounts.

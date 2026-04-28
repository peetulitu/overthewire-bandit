## Objective
The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the 
range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak 
SSL/TLS and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you 
whatever you send to it. 
Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.

## Approach
Since I know the port range, I will begin by scanning that range, then I will look at the running services and figure out which 
port to use.
Now I will read the `nc` man page to find the flags I need to scan the machine's open ports. 
My first command using `-v` and `-z` for verbose output, and not sending data worked, but I received all the failed alerts as well, 
making it very difficult to find the open ports. I'm going to try another command and see if I can suppress the failed connections.
Removing the `-v` flag worked(I also scanned for `UDP` ports with `-u`, but none are open); I found 5 open `TCP` (31046, 31518, 
31691, 31790, 31960) ports, let's check the services they are running. I haven't been able to get the services running on each port
with `netcat`, so I will run an `nmap` scan with the flags `-sV` for the service version, `-sC` for the default script scan, and  
`-p` to specify the ports I want scanned. With this command, I found the services used by each port.
```bash
PORT      STATE SERVICE     VERSION
31046/tcp open  echo
31518/tcp open  ssl/echo
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=SnakeOil
| Not valid before: 2024-06-10T03:59:50
|_Not valid after:  2034-06-08T03:59:50
31691/tcp open  echo
31790/tcp open  ssl/unknown
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=SnakeOil
| Not valid before: 2024-06-10T03:59:50
|_Not valid after:  2034-06-08T03:59:50
| fingerprint-strings: 
|   FourOhFourRequest, GenericLines, GetRequest, HTTPOptions, Help, LPDString, RTSPRequest, SIPOptions: 
|_    Wrong! Please enter the correct current password.
31960/tcp open  echo
```
Looking at the second and fourth ports, I see that they are running `SSL`, so it has to be one of these. I noticed that the fourth
port(31790) has a fingerprint message that reads: 'Wrong! Please enter the correct current password.' and is the only one of the
two with an unknown service, while the second port also has the `echo` service running, so we now know that the right port is 31790.
I will try to connect to it via `openssl` and input the password to the current level. I was able to connect to the port, but the
password returned KEYUPDATE, I'll look at the `openssl s_client` man page and figure out this last part. Under 'CONNECTED COMMANDS'
I see that KEYUPDATE is triggered with the letter `k` at the beginning of a line, and because the password starts with a `k`, it is 
activating. To solve this, I will add the `-nocommands` flag, which ignores the interactive command letters.
This worked, I got the `RSA` private key.

## Commands Used
```bash
nc -z localhost 31000-32000
nmap -sV -sC localhost -p31046,31518,31691,31790,31960
openssl s_client -connect localhost:31790
openssl s_client -nocommands -connect localhost:31790
```

## Takeaway
This level brought together several new skills: port scanning with `nc -z` for quick range scans, service and version detection with 
`nmap -sV -sC`, and more use of `openssl s_client`, including its interactive commands and how to disable them. The most useful lesson came 
from the KEYUPDATE issue, it was a good lesson that "interactive" tools often have hidden behaviors triggered by specific input, and that 
reading the manpage when something unexpected happens is almost always the easiest and fastest solution, maybe Reddit too. This is also the 
first level where I used `nmap`, a tool I know I'll be using constantly from now on.

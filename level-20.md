## Objective
There is a setuid binary in the home directory that does the following: it makes a connection to localhost on the port you specify as a command-line argument. It then reads a line of text from the 
connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).
NOTE: Try connecting to your own network daemon to see if it works as you think

## Approach
First, what I will do is to use `netcat` to connect to port `5555` and give it the password. This didn't do anything because there is no service running on port 5555. I will look into what network daemons are and how to connect to them, so network
daemons are simply a service that is running on a port waiting for a connection to happen. To connect to it, I will have to set up a listener on a port, which can be done using `netcat` and then 
connect to that port with the binary in the home directory. First, I will have to open another tab with the `bandit20` user, then in the new session set up the `netcat` listener, from the first
session I will connect to it via the `suconnect` binary, and back on my listener I will input the password to this level. Great, this worked, and I received the password to the next level.

## Commands Used
```bash
# Session 2:
nc -l -p 5555
# Session 1:
./suconnect 5555
# Session 2:
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
# Session 2 output:
*PASSWORD*
```

## Takeaway
This level introduced the client/server model in a hands-on way. `suconnect` is a TCP client to interact with it; I had to play the role of the server using `nc -l`. This same pattern shows up across 
security work:
Reverse shells: an attacker runs `nc -lvnp 4444` on their own machine, the target executes a payload that calls back to that listener, giving the attacker an interactive shell. Service interaction 
during testing: sometimes you need to inspect what a client sends before responding, and standing up a `netcat` listener is the simplest way to do it. A TCP connection has two sides, and which side 
you control determines what kind of analysis or attack you can run. `nc` is endlessly useful precisely because it can play either role `nc <host> <port>` to connect, `nc -l -p <port>` to listen.

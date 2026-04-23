## Objective
The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL/TLS 
encryption. Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.

## Approach
This was a really fun level. The first command I tried was doing the same I did in the previous level, using `nc`, of course, that didn't work.
So I started to read about `openssl`, I knew what `SSL` and `TLS` were, but I didn't have a clue how it worked or how I had to use it in order 
to send a message. Once I understood what it did I read the man page for `openssl` and wrote the command that allowed me to connect and send
the password.

## Commands Used
```bash
openssl s_client -connect localhost:30001
```

## Takeaway
Directly seeing and experimenting with `TLS` was very interesting and exciting, once I finished the level I booted up two VMs and WireShark
to see how these interactions looked like in the packets themselves.

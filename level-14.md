## Objective
The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

## Approach
This level was particularly challenging as I didn't really understand what it was asking, so I tried to `SSH` to localhost using the user
bandit14 and 15, but that wasn't working. Then I decided to take a step back and read through all the documentation and commands that the 
level recommends. After looking at everything, I connected to localhost on port 30000 via `nc` (netcat), and once I was connected, I just 
submitted the password and finished the level.

## Commands Used
```bash
nc localhost 30000
```

## Takeaway
It was very good to work with `nc` hands-on instead of reading about the tool and looking at other people's examples. I learned what it is and 
how to use it. Seeing how services are listening on different ports and how these connections are made was fascinating.
Getting my first experience on such an amazing tool was eye-opening, one tool that can be used to connect to TCP/UDP services, listen for
connections, transfer files... although this level was a very basic introduction to it, getting to read about it and have an idea of what this
tool is actually capable of was really nice, I look forward to using it more in the future.

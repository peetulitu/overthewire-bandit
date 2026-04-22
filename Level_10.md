## Objective
The password for the next level is stored in the file data.txt, which contains base64 encoded data.

## Approach
This level was quite straightforward, I simply needed to use the `base64` command to be able to read the data in the file using 
the `-d` flag to specify that I wanted the data decoded.

## Commands Used
```bash
base64 -d data.txt
```

## Takeaway
It is a great level to be introduced to what `base64` is and how encoding works. It was very interesting to learn that `base64` is not 
secure, as it can be decoded by anyone as it is data encoding not encryption.

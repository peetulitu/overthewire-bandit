## Objective
The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated
by 13 positions.

## Approach
This level focused on a different kind of challenge, and it made the solution very simple. The information provided lets us know that the 
text has been passed through `ROT13`, so the next step is to undo the encoding. To do so, I used the `cat` command and piped the output
through `tr`. This works because the alphabet has 26 letters, so if it were initially rotated by 13 positions, we can rotate it 13 more to reverse back
to where we started.

Command breakdown:
- `cat data.txt` -> Reads the contents of data.txt
- `| tr 'A-Za-z' 'N-ZA-Mn-za-m'` -> Replaces the characters from set1 with the ones from set2. Since the characters in set2 are 13 characters
  away, therefore bringing the letters back to their original position.
  
## Commands Used
`cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'`

## Takeaway
During this level, I learned what `ROT13` is and how it works, using the newly discovered `tr` command and reading its manpage was very 
helpful in completing the level.

Category: General skills

Points: 50

Author: Yahaya Meddy
### Description
Our server seems to be leaking pieces of a secret flag in its logs. The parts are scattered and sometimes repeated. Can you reconstruct the original flag? Download the [logs](https://challenge-files.picoctf.net/c_saffron_estate/e5a9c3caaebf4ba0895d2c5623c99d1a1a563b90ade666d0bcfd8977da19a430/server.log) and figure out the full flag from the fragments.

---
### Hints
1: You can use `grep` to filter only matching lines from the log.

2: Some lines are duplicates; ignore extra occurrences.

---
### Solution
As the hint suggests, we can use `grep`. We could also open a random text editor and hit `Ctrl+F` , that would work. We'll use `grep`.

Checking the first ten lines of the file, we see that the file contains the flag parts in cleartext, each prefixed with `FLAGPART`.

![](attachments/Pasted%20image%2020251002113634.png)

So let's run`grep FLAGPART server.log`.

![](attachments/Pasted%20image%2020251002113828.png)

GG!
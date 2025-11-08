Category: Forensics

Points: 100

Author: Yahaya Meddy
### Description
This file seems broken... or is it? Maybe a couple of bytes could make all the difference. Can you figure out how to bring it back to life? Download the file [here](https://challenge-files.picoctf.net/c_amiable_citadel/8646393bf40c0026e51065e57963b604edf0a9a73371e01d1af2865c050d3e68/file).

---
### Hints
1: Try checking the file’s header.

2: JPEG

3:Tools like xxd or hexdump can help you inspect and edit file bytes.

---
### Solution
We start the challenge with a corrupt file. The `file` command gets us nowhere but the second clue suggests that the corrupted file is a `JPEG`. In this type of challenge, you just need to correct the header (magic bytes) of the file to have the flag.
To do this, we will first look for the **magic bytes** of `JPEG` on [wikipedia](https://en.wikipedia.org/wiki/List_of_file_signatures).

![](attachments/Capture%20d'écran%202025-10-02%20103759.png)

We will then use `hexeditor` to view and modify the file header and obtain the flag. The command:

```sh
hexeditor file
```

![](attachments/Pasted%20image%2020251002104334.png)

We see that it suffices to replace the bytes `5C 78` by `FF D8`. By rerunning the `file` command, the chall file is finally recognized as `JPEG`.

![](attachments/Pasted%20image%2020251002104710.png)

Let's open the file with `xdg-open file` and we will have the flag.

![](attachments/Pasted%20image%2020251002104957.png)

GG!
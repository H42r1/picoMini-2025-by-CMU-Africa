Category: Forensics

Points: 50

Author: Prince Niyonshuti N.
### Description
Hi, intrepid investigator! 📄🔍 You've stumbled upon a peculiar PDF filled with what seems like nothing more than garbled nonsense. But beware! Not everything is as it appears. Amidst the chaos lies a hidden treasure—an elusive flag waiting to be uncovered. Find the PDF file here [Hidden Confidential Document](https://challenge-files.picoctf.net/c_saffron_estate/890f24eea8682b3ed2572a336505e3b40a735dfb1776db57c404a1e6edbcc699/confidential.pdf) and uncover the flag within the metadata.

---
### Hints
1: Don't be fooled by the visible text; it’s just a decoy!

2: Look beyond the surface for hidden clues

---
### Solution
We start the challenge with a pdf file. Since the challenge is forensics, the first instinct to have is to check the file metadata. To do this, we will use the app`exiftool`.

![](attachments/Pasted%20image%2020251002093217.png)

We see that the value of `Author` is base64 encoded. By using online decoding tools like [dcode](https://www.dcode.fr/code-base-64) or [cyberchef](https://cyberchef.org), we obtain the flag: `picoCTF{REDACTED}`.

GG!
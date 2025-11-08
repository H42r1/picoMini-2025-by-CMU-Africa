Category: Forensics

Points: 100

Author: Prince Niyonshuti N.
### Description
The SOC team discovered a suspiciously large log file after a recent breach. When they opened it, they found an enormous block of encoded text instead of typical logs. Could there be something hidden within? Your mission is to inspect the resulting file and reveal the real purpose of it. The team is relying on your skills to uncover any concealed information within this unusual log. Download the encoded data here: [Logs Data](https://challenge-files.picoctf.net/c_amiable_citadel/563936741f1fcad4b6e9e1043d56fd127ef7de2227d293a8e478ae6887816d10/logs.txt). Be prepared—the file is large, and examining it thoroughly is crucial .

---
### Hint
Use `base64` to decode the data and generate the image file.

---
### Solution
The clue solves the challenge by itself lol. Let's use `base64` to decode the image.

![](attachments/Pasted%20image%2020251002105527.png)

We obtain an image on which there is an ASCII string.

![](attachments/Pasted%20image%2020251002105647.png)

By using [Image to text](https://www.imagetotext.info/), we can extract the text quite easily from the image.

`7069636F4354467B666F72656E736963735F616E616C797369735F69735F616D617A696E675F35646161346132667D`

The last step is decoding. We will use [dcode](https://www.dcode.fr). After analysis, we see that this string is ASCII.

![](attachments/Pasted%20image%2020251002110211.png)

Let's decode the string to obtain the flag:

![](attachments/Pasted%20image%2020251002110345.png)

GG!
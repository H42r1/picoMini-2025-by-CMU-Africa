Category: Forensics

Points: 100

Author: Yahaya Meddy
### Description
You’re given a seemingly ordinary JPG image. Something is tucked away out of sight inside the file. Your task is to discover the hidden payload and extract the flag. Download the jpg image [here](https://challenge-files.picoctf.net/c_saffron_estate/5037bce9fb8a1d1975211489cedcdcd2e374d9e1837d7ce76dc3355ba5d71952/img.jpg).

---
### Hint
Download the jpg image and read its metadata

---
### Solution
Nous allons faire ce que suggère l'indice en utilisant `exiftool`.

![](attachments/Pasted%20image%2020251002110705.png)

The value of `Comment` attracts our attention. This is a `base64` encoded string.

Decoded string: `steghide:cEF6endvcmQ=`. `cEF6endvcmQ=` is also in `base64`

Decoded string: `pAzzword`

Let's use `steghide` with the passphrase we obtained.

![](attachments/Pasted%20image%2020251002111251.png)

Let's read the contents of the extracted file:

![](attachments/Pasted%20image%2020251002111623.png)

GG!
Category : Forensics

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

La valeur de `Comment` attire notre attention. Il s'agit d'une chaîne encodée en `base64`.

Chaîne décodée : `steghide:cEF6endvcmQ=`. `cEF6endvcmQ=` est aussi en `base64`

Chaîne décodée : `pAzzword`

Dernière ligne droite. Utilisons `steghide` avec la passphrase que nous avons obtenu.

![](attachments/Pasted%20image%2020251002111251.png)

Lisons le contenu du fichier extrait :

![](attachments/Pasted%20image%2020251002111623.png)

GG!
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
On commence le chall avec un fichier corrompu. La commande `file` ne nous mène nulle part mais le deuxième indice suggère que le fichier corrompu est un `JPEG`. Dans ce genre de challenge, il suffit juste de corriger l'en-tête (magic bytes) du fichier pour avoir le flag.
Pour ce faire, nous allons d'abord chercher les **magic bytes** de `JPEG` sur [wikipedia](https://en.wikipedia.org/wiki/List_of_file_signatures).

![](attachments/Capture%20d'écran%202025-10-02%20103759.png)

Nous utiliserons ensuite `hexeditor` pour voir et modifier l'en-tête du fichier et obtenir le flag. La commande :

```sh
hexeditor file
```

![](attachments/Pasted%20image%2020251002104334.png)

On constate qu'il suffit de remplacer les octets `5C 78` par `FF D8`. En relançant la commande `file`, le fichier du chall est enfin reconnu comme `JPEG`.

![](attachments/Pasted%20image%2020251002104710.png)

Ouvrons le fichier avec `xdg-open file` et nous aurons le flag.

![](attachments/Pasted%20image%2020251002104957.png)

GG!
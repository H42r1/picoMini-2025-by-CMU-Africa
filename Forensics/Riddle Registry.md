Category : Forensics

Author: Prince Niyonshuti N.
##### Description
Hi, intrepid investigator! 📄🔍 You've stumbled upon a peculiar PDF filled with what seems like nothing more than garbled nonsense. But beware! Not everything is as it appears. Amidst the chaos lies a hidden treasure—an elusive flag waiting to be uncovered. Find the PDF file here [Hidden Confidential Document](https://challenge-files.picoctf.net/c_saffron_estate/890f24eea8682b3ed2572a336505e3b40a735dfb1776db57c404a1e6edbcc699/confidential.pdf) and uncover the flag within the metadata.

---
##### Hints
1: Don't be fooled by the visible text; it’s just a decoy!

2: Look beyond the surface for hidden clues

---
##### Solution
On commence le challenge avec un fichier pdf. Étant donné que le challenge est de type forensics, le premier réflexe à avoir est de vérifier les métadonnées du fichier. Pour ce faire, nous utiliserons l'app`exiftool`.

![](attachments/Pasted%20image%2020251002093217.png)

On constate que la valeur de `Author` est encodée en base64. En utilisant les outils de décodage en ligne comme [dcode](https://www.dcode.fr/code-base-64) ou [cyberchef](https://cyberchef.org), on obtient le flag en clair: `picoCTF{REDACTED}`.
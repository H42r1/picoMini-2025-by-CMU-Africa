Category: Reverse engineering 

Points: 400

Author: Prince Niyonshuti N.
### Description
In a bustling city where innovation meets finance, Pico Bank has emerged as a beacon of cutting-edge security. Promising state-of-the-art protection for your assets, the bank claims its mobile application is impervious to all forms of cyber threats. Pico Bank’s tagline, "Security Beyond the Limits," echoes through its high-tech marketing campaigns, assuring users of their utmost safety. As a cybersecurity enthusiast, your mission is to test these bold claims. You’ve been hired by a secretive organization to put Pico Bank’s mobile app through a rigorous security assessment. The flag might be in one or more locations, and additional information reveals that a Pico Bank user’s credentials were leaked in an unusual way. Your task is to crack the username and password based on the following profile information: His name is Alex Johnson with the email johnson@picobank.com, Date of Birth: March 14, 1990, Last Transaction Amount: $345.67, Pet name: tricky, and Favorite Color: Blue. To perform this challenge, you can use any Android emulator. Some examples include [Genymotion Android Emulator](https://www.genymotion.com/product-desktop/download/) or [Android Studio](https://developer.android.com/studio).

Additional details will be available after launching your challenge instance.

---
### Hints
1: Use tools like JadxGUI or apktool to inspect the APK.

2: Look at the app's network requests, especially for login and OTP.

3: The flag has two parts.

4: Check the server’s response after entering the correct OTP.

5: Investigate the transaction history for unusual data.

---
### Solution
Il y a beaucoup d'informations. C'est mon chall préféré de la compétition.
Nous avons accès à une app web à partir de laquelle nous pouvons télécharger un fichier APK.

![](attachments/Pasted%20image%2020251002122758.png)

Nous téléchargeons le fichier apk et l'exécutons avec [Android Studio](https://developer.android.com/studio).
Pour apprendre à installer et rooter l'émulateur, nous pouvons suivre la vidéo du Youtuber [UnderSecured](https://youtu.be/QzsNn3GhYYk?si=lLbQxnRHPemhOsnx).
Après avoir installé le fichier apk: 

![](attachments/Pasted%20image%2020251002123038.png)

Après avoir lancer l'app:

![](attachments/Pasted%20image%2020251002123113.png)

Nous somme confrontés à un formulaire où il n'y a pas la possibilité de créer un compte. Nous allons devoir chercher les identifiants dans le code source de l'app.

Pour ce faire, nous pouvons utiliser `jadx` ou `apktool` pour désassembler l'apk et rechercher la variable les identifiants avec grep et les infos trouvées dans la description du chall. La différence entre `jadx` et `apktool` :

- **`jadx`** → décompile en **Java lisible** (idéal pour lire la logique rapidement).
    
- **`apktool`** → décompile en **smali + ressources** (idéal pour patcher/reconstruire, et pour voir des détails bas-niveau).


Nous utiliserons `jadx`. 

![](attachments/Pasted%20image%2020251002123636.png)

Syntaxe: `jadx -d $HOME/extracted $HOME/pico-bank.apk

- `-d $HOME/extracted`  
    `-d` = _directory_ de sortie. Ici on dit « écris la sortie dans `~/extracted` ». `$HOME` est la variable d’environnement qui pointe vers notre dossier personnel (ex : `/home/kali).
    
- `$HOME/pico-bank.apk  
    Le chemin vers l’APK à analyser (ici `~/pico-bank.apk`). Jadx lit ce fichier en entrée.

La prochaine étape est de rechercher les identifiants de `Alex Johnson` dans le code source avec grep.

![](attachments/Pasted%20image%2020251002124350.png)

Bingo! Les identifiants se trouvent dans le fichier `Login.java` (Héhé... Logique). 

Après avoir soumis les identifiants dans le formulaire de connexion, on se retrouve face à une page où il faut entrer un `OTP`.

![](attachments/Pasted%20image%2020251002124716.png)

Cherchons l'OTP dans le code source avec `find` cette fois-ci parce que les identifiants ont eux été trouvés dans un fichier au nom assez explicite XD.

Syntaxe: `find <Dossier_d'extraction> -name <motif> 2>/dev/null`

- `<Dossier_d'extraction>`  
    Point d’entrée où `find` commence la recherche (ex : `~/extracted` ou `./apktool-out`).
    
- `-name <motif>`  
    Filtre sur le **nom** du fichier (ou du répertoire). `<motif>` utilise des **globs** shell (wildcards), pas des regex.  
    Exemples de motifs :
    
    - `*otp*` → tous les fichiers contenant le motif `otp`
    
- `2>/dev/null`  
    Redirige la **sortie d’erreur** (stderr, descripteur `2`) vers `/dev/null` (la poubelle).  
    Ça supprime les messages du type `Permission denied` ou autres erreurs d’accès — pratique quand tu fouilles des arbores larges sans vouloir spam d’erreurs.


![](attachments/Pasted%20image%2020251002130348.png)

L'OTP doit sûrement se trouver dans le fichier `OTP.java` (jpp).
Après analyse du code `OTP.java`, on constate que la valeur de l'OTP est stockée sous la clé `otp_value` codée en dur dans `res/values/strings.xml` et doit être soumise dans une requête POST vers `/verify-otp` pour obtenir le flag. 

![](attachments/Pasted%20image%2020251002131408.png)

En réalité, l'application ne communique pas directement avec un serveur. On va devoir faire la requête vers le serveur à la main. Mais une question persiste. Quel serveur ?

Réponse: Nous utiliserons le serveur de l'application fourni par le challenge.

Avec `curl`, on soumet l'OTP avec une requête `POST` au serveur du l'app où nous avons télécharger l'apk.

```sh
curl -X POST http://<SNIP>/verify-otp -H "Content-Type: application/json" -d '{"otp":"<OTP_VALUE>"}'
```


![](attachments/Pasted%20image%2020251002132303.png)

Nous avons obtenu une partie du flag. Le hint nous fait comprendre que l'autre partie est au niveau de l'application. Après avoir mis l'OTP au niveau de l'application, on arrive sur un dashboard.

![](attachments/Pasted%20image%2020251002132852.png)

Ici, on comprends vite que l'autre partie du flag est encodé au niveau des prix des transactions bancaires.
Nous irons une dernière fois dans le code source pour accéder plus facilement aux valeurs des transactions financières de `Johnson`.

![](attachments/Pasted%20image%2020251002133145.png)

Elles se trouvent donc dans le fichier: `extracted/sources/com/example/picobank/MainActivity.java`

![](attachments/Pasted%20image%2020251002133318.png)

C'est de l'ASCII évidement mais c'est à partir de la transaction : `Car Maintenance` que ça se complique parce que sa valeur à moins de bits que les autres. Pour combler ça, `il suffit d'ajouter un zéro devant`.

Exemple: 

**110001 devient 0110001**

On fait la même chose pour toutes les transactions à `6 bits` et on est bon. Nous pouvons aussi nous aider de scripts pour automatiser tout ça (Ce que j'ai fait).

GG!
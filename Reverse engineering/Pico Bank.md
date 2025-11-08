Category: Reverse engineering 

Points: 400

Author: Prince Niyonshuti N.
### Description
In a bustling city where innovation meets finance, Pico Bank has emerged as a beacon of cutting-edge security. Promising state-of-the-art protection for your assets, the bank claims its mobile application is impervious to all forms of cyber threats. Pico Bank’s tagline, "Security Beyond the Limits," echoes through its high-tech marketing campaigns, assuring users of their utmost safety. As a cybersecurity enthusiast, your mission is to test these bold claims. You’ve been hired by a secretive organization to put Pico Bank’s mobile app through a rigorous security assessment. The flag might be in one or more locations, and additional information reveals that a Pico Bank user’s credentials were leaked in an unusual way. Your task is to crack the username and password based on the following profile information: His name is Alex Johnson with the email `johnson@picobank.com`, Date of Birth: March 14, 1990, Last Transaction Amount: $345.67, Pet name: tricky, and Favorite Color: Blue. To perform this challenge, you can use any Android emulator. Some examples include [Genymotion Android Emulator](https://www.genymotion.com/product-desktop/download/) or [Android Studio](https://developer.android.com/studio).

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
There is a lot of information. This is my favorite challenge from the competition.
We have access to a web app from which we can download an APK file.

![](attachments/Pasted%20image%2020251002122758.png)

We download the apk file and run it with [Android Studio](https://developer.android.com/studio).
To learn how to install and root the emulator, we can follow the Youtuber's video[UnderSecured](https://youtu.be/QzsNn3GhYYk?si=lLbQxnRHPemhOsnx).
After installing the apk file:

![](attachments/Pasted%20image%2020251002123038.png)

After launching the app:

![](attachments/Pasted%20image%2020251002123113.png)

We are faced with a form where there is no possibility of creating an account. We will have to look for the identifiers in the source code of the app.

To do this we can use `jadx` or `apktool` to disassemble the apk and search for the variable identifiers with grep and the info found in the chall description. The difference between `jadx` and `apktool` :

- **`jadx`** → decompiles to **readable Java** (ideal for reading logic quickly).
    
- **`apktool`** → decompiles to **smali + resources** (ideal for patching/rebuilding, and for seeing low-level details).


We will use `jadx`. 

![](attachments/Pasted%20image%2020251002123636.png)

Syntax: `jadx -d $HOME/extracted $HOME/pico-bank.apk

The next step is to search the source code for `Alex Johnson` creds with grep.

![](attachments/Pasted%20image%2020251002124350.png)

Bingo! The identifiers are found in the `Login.java` file (Hehe... Logical).

After submitting the credentials in the connection form, we find ourselves faced with a page where we must enter an `OTP`.

![](attachments/Pasted%20image%2020251002124716.png)

Let's look for the OTP in the source code with `find` this time because the identifiers were found in a file with a fairly explicit name XD.

Syntax: `find <Dossier_d'extraction> -name <motif> 2>/dev/null`


![](attachments/Pasted%20image%2020251002130348.png)

The OTP must surely be in the `OTP.java` (jpp) file.
After analyzing the `OTP.java` code, we see that the OTP value is stored under the hardcoded `otp_value` key in `res/values/strings.xml` and must be submitted in a POST request to `/verify-otp` so that we can obtain the flag.

![](attachments/Pasted%20image%2020251002131408.png)

In reality, the application does not communicate directly with a server. We will have to make the request to the server by hand. But a question remains. Which server?

Answer: We will use the application server provided by the challenge.

With `curl`, we submit the OTP with a `POST` request to the app server where we downloaded the apk.

```sh
curl -X POST http://<SNIP>/verify-otp -H "Content-Type: application/json" -d '{"otp":"<OTP_VALUE>"}'
```


![](attachments/Pasted%20image%2020251002132303.png)

We got part of the flag. The hint makes us understand that the other party is at the mobile application level. After entering the OTP, you arrive on a dashboard.

![](attachments/Pasted%20image%2020251002132852.png)

Here, we quickly understand that the other part of the flag is encoded at the level of the values ​​of financial transactions.
We will go one last time into the source code to more easily access the values ​​of `Johnson`'s financial transactions.

![](attachments/Pasted%20image%2020251002133145.png)

They are therefore in the file:
`extracted/sources/com/example/picobank/MainActivity.java`

![](attachments/Pasted%20image%2020251002133318.png)

It's obviously ASCII but it's from the transaction: `Car Maintenance` that it gets complicated because its value has fewer bits than the others. To fill that, `just add a zero in front`.

Exemple: 

**110001 devient 0110001**

We do the same thing for all `6-bit` transactions and we are good. We can also use scripts to automate all of this (which I did).

GG!
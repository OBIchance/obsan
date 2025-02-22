+++
title = 'JavaScript Deobfuscation'
date = 2025-02-18T19:47:08-06:00
draft = false
author = "Obsan Muzemil"
categories = ["Red"]
updated = 2025-02-18
+++

# **JavaScript Security Analysis and Exploitation**
![AI](/images/java/d2428cd5-7d42-407c-af7e-d561345821b6.webp)

This lab involved analyzing a website’s JavaScript code to uncover hidden information, reverse obfuscation, and extract a secret key. The key was then decoded and sent as a POST request to obtain the final flag.
(This lab is found on hack the box JavaScript Deobfuscation module  )
## **Step 1: Identifying the JavaScript File**
I started by inspecting the HTML source code of the webpage that was given to me. The script tag in the HTML revealed the JavaScript file:
![HTML](/images/java/HTML_CODE.png)
```html
<script src="api.min.js"></script>
```

This file contained obfuscated JavaScript.

## **Step 2: Running the JavaScript Code**
After extracting the JavaScript file, I checked its contents. Running the script in a browser console produced unreadable output, indicating obfuscation.
![JS](/images/java/JS_flag.png)
## **Step 3: Deobfuscating the JavaScript Code**
I used an **online JavaScript unpacker** and manual formatting techniques to make the script readable. The deobfuscated script contained an API request:
![unpacker](/images/java/unpacker.png)

when I unpacked the code I could run it in the console so i had to fix it and run it. 
![obf](/images/java/java_obf.png)
```javascript
function apiKeys() {
    var flag = 'HTB{n3v3r_run_0_bfu5c473d_c0d3!}';
    var xhr = new XMLHttpRequest();
    var _0x437f8b = '/keys.php';

    xhr.open('POST', _0x437f8b, true);
    xhr.send(null);
}

console.log('HTB{j4v45c_r1p7_3num3r4710n_15_k3y}');
```

From this, I know that `keys.php` was the endpoint handling API key validation. 
![keyphp](/images/java/keys.png) 

## **Step 4: Extracting and Decoding the Key**


I sent a POST request to `keys.php` to retrieve the encoded key:

```sh
curl -s http://94.237.56.156:34126/keys.php -X POST -d "pram1=sample"
```
![key](/images/java/curl_keys.png)
This returned an encoded string:

```
4150495f70336e5f37333537316e365f31355f66756e
```

Using the command below, I decoded the hex string:

```sh
echo 4150495f70336e5f37333537316e365f31355f66756e | xxd -p -r
```

The result:

```
API_p3n_73571n6_15_fun
```
![api](/images/java/decode.png)

## **Step 5: Submitting the Key for the Final Flag**
Now that I had the correct key, I sent another POST request:

```sh
curl -s http://94.237.56.156:34126/keys.php -X POST -d "key=API_p3n_73571n6_15_fun"
```

The response contained the final flag:

```
HTB{r34dy_70_h4ck_my_w4y_1n_2_HTB}
```
![HTB](/images/java/HTB.png)


This was a  hands-on exercise in **JavaScript Deobfuscation**.


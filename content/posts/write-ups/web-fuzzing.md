+++
title = 'Web Fuzzing'
date = 2025-02-16T19:47:08-06:00
draft = true
author = "Obsan Muzemil"
updated = 2025-02-16
+++

Web Fuzzing

## **Introduction**
In this assessment, I was given the IP address of an online academy but had no prior information about its website structure. My goal was to conduct a penetration test to find all the subdomains and pages linked to the given IP. This involved various types of fuzzing to discover hidden resources and potential vulnerabilities.



## **Step 1: Finding Subdomains**
To start, I ran a **subdomain/vhost fuzzing scan** on `*.academy.htb` using a tool like `ffuf` or `gobuster`.For this lab I will be using ffuf and  This process involved sending a list of potential subdomain names to the target and checking for responses that indicated active domains.
![subdomains](/images/ffuf/subdomains.png)





## **Step 2: Identifying File Extensions**
Before running a **page fuzzing scan**, I first needed to check what file extensions the domains accept. This helps in targeting the correct file types and avoiding unnecessary requests.

![extensions](/images/ffuf/extn.png)

I used **extension fuzzing** and found that the site accepts two 



## **Step 3: Locating Restricted Pages**
This part took a lot of time for me to figure out since I needed to know what wordlists to use and how can approach it. after I went to module to revise I used a different wordlists and  I finally cracked it. 
using this: 
```
ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://faculty.academy.htb:50628/FUZZ -H "Host: faculty.academy.htb" -recursion -recursion-depth=1 -mc 200,301 -fc 403,404 -e .php,.php7,.phps -v -fs 287 
```

![c](/images/ffuf/courses.ext.png)

Then after got the first domain I later fuzzed for page and found and when I curled it gave this response 

> **"You don't have access!"**

![ls](/images/ffuf/ls.ext.png)
This response indicates that the page exists but requires special permissions or authentication.



## **Step 4: Finding Accepted Parameters**
On the restricted page, I examined the URL and tested different parameters to see what inputs the page accepts.
![parmeter](images/ffuf/postParameter.png)



These parameters can be used to manipulate the website’s behavior, potentially revealing sensitive information.


## **Step 5: Fuzzing Parameters for a Flag**
Once I had the parameters, I **fuzzed their values** to see if any special inputs triggered an unexpected response. One of them led to a **flag** being displayed.

![users](/images/ffuf/hair.png)



Finding a flag means we successfully bypassed a restriction or triggered a specific condition that the website wasn’t supposed to reveal.
![value](/images/ffuf/valueflag.png)


## **Conclusion**
By  scanning for subdomains, file extensions, pages, and parameters, I was able to map out the website's structure and uncover hidden information. This lab demonstrates the importance of **fuzzing** in web security assessments and how it can be used to find vulnerabilities in real-world applications.

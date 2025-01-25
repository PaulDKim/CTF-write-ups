# Natas13 (Level 12 -> 13)

  * username: `natas13`  
  * password: `trbs5pCjCrkuSknBBKHhaBxq6Wm1j3LC`  
  * url: `http://natas13.natas.labs.overthewire.org`  
  * flag: ``  
  * vulnerability: `File Upload Vulnerability`  

## Proof of Concept
1. From looking at the home page of the application, it looks like a `file upload vulnerability` lab. I start by
checking the source code to look for clues and to see if the web application has any `client-side` filters.   
![descript](images/natas13-source.png)
2. It looks like the web application doesn't have any client-side filters but it does specify that they only 
accept `image` files and have a file size limit. To start off the lab, I need to figure out what kind of language
the web application is running on- so in the url, I type `/index.php`. I got returned the same page, so I know
that it's likely that the web application is running on `PHP`. 
3. Next, I can try employing a variety of techniques such as:
  - double extensions 
  - reverse double extensions
  - MIME-type spoofing 
  - Magic Byte spoofing  
I first want to see how the web application normally acts. I downloaded a simple google logo png as my first test.  
![descript](images/natas13-normal-1.png)  
![descript](images/natas13-normal-2.png)  
So the web application sends my image file to an `/uploads` folder, and even provides with a clickable link to the file. I also now know `.png` files are permitted. 
4. **double extensions**: First, I want to test if `.png.php` works. This will work if the web app only looks for `.png` and does not filter out any unwanted file types. Let's fire up Burp Suite and intercept the request.  
![descript](images/natas13-original-request.png)  
This is what the normal request looks like. I will change the file name to `google-logo_11zon.png.php` and also `delete` the original data body and replace it with php code: `<?php passthru($_REQUEST['cmd']); ?>`
> My PHP code is a simple web shell! 

![descript](images/natas13-altered-request.png)  
![descript](images/natas13-altered-request-response.png)  
It looks like this didn't bypass the validation check on the backend. Let's move on to `reverse double extension`.  
5. **reverse double extension**: This technique relies mostly on system misconfigurations (keep that in mind!). I will use the same exact methology as stated in `step 4` but change the file name to `google-logo_11zon.php.png`. It looks like I got the same error. Let's move on to `MIME-type spoofing`  
6. **MIME-type Spoofing**: In this step, I realized I was changing the wrong file name when I was altering my requests. I must move up one row and change the file extension of the file with the `randomized name`. I figured this out from re-reading the source code and studying how the web application acts. Every upload I tried I noticed that the name would change. And on legimitate/successful POST requests, the link would show the same randomized name of the file I upload with a `.jpg` extension. I could redo the previous steps, but I found `MIME type spoofing` works.  
![descript](images/natas13-mime-type.png)  
7. The red arrows are the indicators as to which parts I changed, but for the most part the steps I took are very similar to that of `step 4` on this write up. However, despite the file upload being successful, it seems that I cannot pass a web shell for this challenge of the lab. But because I know the path to the password, I can change my php script to be: `<?php passthru("cat /etc/natas_webpass/natas14"); ?>`. This will solve the lab!

## Notes
* `MIME-type Spoofing` is tricking a system into believing a file is a different type than it actually is by faking its `MIME` type. (e.g., `making a malicious script appear as an image`)
  * A `MIME type` is a label that tells a system what kind of file it is

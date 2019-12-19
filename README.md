# MIME Confusion Attack on Midori Browser

This PoC describes a MIME confusion attack on: 

* Midori Browser 0.5.11 on Windows 10

* Internet Explorer 11.535.18362.0 on Windows 10

* Safari 10.1.2 (12603.3.8) on MacOS Sierra


## About the MIME Confusion Attack

Scanning the content of a file allows web browsers to detect the format of a file regardless of the specified Content-Type by the web server. For example, if browser requests script from a web server and that web server sends that script using a Content-Type of “image/jpg” browser will successfully detect the actual format and will execute the script. This technique, colloquially known as “MIME sniffing”, compensates for incorrect, or even complete absence of metadata browsers need to interpret the contents of a page resource. The browser uses contextual clues (the HTML element that triggered the fetch) or also inspects the initial bytes of media type loads to determine the correct content type. While MIME sniffing increases the web experience for the majority of users, it also opens up an attack vector known as MIME confusion attack.

Consider a web application which allows users to upload image files but does not verify that the user actually uploaded a valid image, e.g., the web application just checks for a valid file extension. This lack of verification allows an attacker to craft and upload an image which contains scripting content. The browser then renders the content as HTML opening the possibility for a Cross-Site Scripting attack (XSS). Even worse, some files can even be polyglots, which means their content satisfies two content types. E.g., a GIF can be crafted in a way to be valid image and also valid JavaScript and the correct interpretation of the file solely depends on the context.

## About Midori Browser

Download: https://www.midori-browser.org/download/


## Step by Step Proof of Concept

As PoC i used a polyglot GIF with a built-in JavaScript code (available at: http://portswigger-labs.net/polyglot/jpeg/xss_within_header_compressed_small_logo.jpg):

```
 */=alert("Burp rocks.")/*
```

And the following HTML / JS code:

```
  <html>
     <img src="2x.jpg">
	   <script src="https://poc.sejalivre.org/wp-content/uploads/2019/12/2x.jpg"></script>
 </html>
```

I hosted at http://joomla.sejalivre.org

Simply accessed the page mentioned above on browsers.

As shown, we find that the browsers has no implementation against MIME confusion attacks

## Midori

![alt](https://ciber.sejalivre.org/WP/MD-1.png)
![alt](https://ciber.sejalivre.org/WP/MD-2.png)

## Internet Explorer

![alt](https://ciber.sejalivre.org/WP/IE-1.png)
![alt](https://ciber.sejalivre.org/WP/IE-2.png)

## Safari

![alt](https://ciber.sejalivre.org/WP/SF-1.png)
![alt](https://ciber.sejalivre.org/WP/SF-2.png)

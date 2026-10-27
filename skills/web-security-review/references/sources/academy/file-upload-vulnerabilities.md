# File uploads

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `4df76e9d09e2357f1bbb28bdad557a9f2b41ebd8df6beb1609d5dd7c14f0dda4`.
> Conversion: Local English OCR, all 8 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


File upload vulnerabilities

File upload vulnerabilities you how to bypass common defense mechanisms in order to upload a web shell, enabling you to take full control of a vulnerable Given how common file upload functions are, knowing how to test them properly is essential knowledge.

Server POST [images HTTP/I .1 HTTP/I.1 200 0K exploit-php File uploaded: exploit php &lt;?php echo system('id); ?&gt;

HTTP/I.1 200 0K

```text
GET /images/exploit.php HTTP/1.1
```

uid=0(root) gid=O(root) groups=O(root)

If you're already familiar with the basic concepts behind file upload vulnerabilities and just want to get practicing, you can acce labs in this topic from the link below. View all file upload labs

## What are file upload vulnerabilities?

File upload vulnerabilities are when a web server allows users to upload files to its filesystem without sufficiently validating things name, type, contents, or size. Failing to properly enforce restrictions on these could mean that even a basic image upload functior to upload arbitrary and potentially dangerous files instead. This could even include server-side script files that enable remote code In some cases, the act of uploading the file is in itself enough to cause damage. Other attacks may involve a follow-up HTTP requ typically to trigger its execution by the server.

## What is the impact of file upload vulnerabilities?

The impact of file upload vulnerabilities generally depends on two key factors:

- Which aspect of the file the website fails to validate properly, whether that be its size, type, contents, and so on.

- What restrictions are imposed on the file once it has been successfully uploaded.

---

. j sp ) to be executed as code. In this case, an attacker could potentially upload a server-side code file that functions as a web s effectively granting them full control over the server. If the filename isn't validated properly, this could allow an attacker to overwrite critical files simply by uploading a file with the sarru server is also vulnerable to directory traversal, this could mean attackers are even able to upload files to unanticipated locations. Failing to make sure that the size of the file falls within expected thresholds could also enable a form of denial-of-service (DoS) at the attacker fills the available disk space.

## How do file upload vulnerabilities arise?

Given the fairly obvious dangers, it's rare for websites in the wild to have no restrictions whatsoever on which files users are allow More commonly, developers implement what they believe to be robust validation that is either inherently flawed or can be easily b For example, they may attempt to blacklist dangerous file types, but fail to account for parsing discrepancies when checking the fi As with any blacklist, it's also easy to accidentally omit more obscure file types that may still be dangerous. In other cases, the website may attempt to check the file type by verifying properties that can be easily manipulated by an attacke like Burp Proxy or Repeater. Ultimately, even robust validation measures may be applied inconsistently across the network of hosts and directories that form th resulting in discrepancies that can be exploited.

## How do web servers handle requests for static files?

Before we look at how to exploit file upload vulnerabilities, it's important that you have a basic understanding of how servers hand static files. Historically, websites consisted almost entirely of static files that would be served to users when requested. As a result, the path o could be mapped 1:1 with the hierarchy of directories and files on the server's filesystem. Nowadays, websites are increasingly dy the path of a request often has no direct relationship to the filesystem at all. Nevertheless, web servers still deal with requests for files, including stylesheets, images, and so on. The process for handling these static files is still largely the same. At some point, the server parses the path in the request to iden extension. It then uses this to determine the type of the file being requested, typically by comparing it to a list of preconfigured ma between extensions and MIME types. What happens next depends on the file type and the server's configuration. If this file type is non-executable, such as an image or a static HTML page, the server may just send the file's contents to the HTTP response.

- If the file type is executable, such as a PHP file, and the server is configured to execute files of this type, it will assign variabl

the headers and parameters in the HTTP request before running the script. The resulting output may then be sent to the cliel response. If the file type is executable, but the server is not configured to execute files of this type, it will generally respond with an errc some cases, the contents of the file may still be served to the client as plain text. Such misconfigurations can occasionally be leak source code and other sensitive information. You can see an example of this in our information disclosure learning mate

Tip

Now that you're familiar with the key concepts, let's look at how you can potentially exploit these kinds of vulnerabilities.

## Exploiting unrestricted file uploads to deploy a web shell

From a security perspective, the worst possible scenario is when a website allows you to upload server-side scripts, such as PHP Python files, and is also configured to execute them as code. This makes it trivial to create your own web shell on the server.

Web shell

---

HTTP requests to the right endpoint.

If you're able to successfully upload a web shell, you effectively have full control over the server. This means you can read and wr files, exfiltrate sensitive data, even use the server to pivot attacks against both internal infrastructure and other servers outside the example, the following PHP one-liner could be used to read arbitrary files from the server's filesystem: < ? php echo file get_contents ( / path/ to/ target/ file') ;

Once uploaded, sending a request for this malicious file will return the target file's contents in the response.

Remote code execution via web shell upload

A more versatile web shell may look something like this: < ? php echo system ($ GET [ command'] ) ;

This script enables you to pass an arbitrary system command via a query parameter as follows:

```text
GET / example/ HTTP/ I . I
```

## Exploiting flawed validation of file uploads

In the wild, it's unlikely that you'll find a website that has no protection against file upload attacks like we saw in the previous lab. E because defenses are in place, that doesn't mean that they're robust. You can sometimes still exploit flaws in these mechanisms t web shell for remote code execution.

Flawed file type validation When submitting HTML forms, the browser typically sends the provided data in a POST request with the content type applicat form-urlencoded . This is fine for sending simple text like your name or address. However, it isn't suitable for sending large arr binary data, such as an entire image file or a PDF document. In this case, the content type multipart/ form-data is preferret Consider a form containing fields for uploading an image, providing a description of it, and entering your username. Submitting su might result in a request that looks something like this:

---

```text
Host: normal—website . com
```

Content -Length: 12345 Content -Type: multipart/ form—data; boundary= --012345678901234567

------------------012345678901234567890123456 Content -Disposition: form—data; name—'l image " ; filename= " example . jpg" Content —Type: image/ jpeg

. binary content of example .jpg. ]

----012345678901234567890123456 Content -Disposition: form data; "

This is an interesting description of my image .

---------012345678901234567890123456 Content—Di sposition : form—data; name= " username "

wiener -012345678901234567890123456--

As you can see, the message body is split into separate parts for each of the form's inputs. Each part contains a Content-Disp header, which provides some basic information about the input field it relates to. These individual parts may also contain their owr Type header, which tells the server the MIME type of the data that was submitted using this input. One way that websites may attempt to validate file uploads is to check that this input-specific Content-Type header matches e MIME type. If the server is only expecting image files, for example, it may only allow types like image/ jpeg and image/png arise when the value of this header is implicitly trusted by the server. If no further validation is performed to check whether the cor file actually match the supposed MIME type, this defense can be easily bypassed using tools like Burp Repeater.

Web shell upload via Content-Type restriction bypass

Preventing file execution in user-accessible directories While it's clearly better to prevent dangerous file types being uploaded in the first place, the second line of defense is to stop the s executing any scripts that do slip through the net. As a precaution, servers generally only run scripts whose MIME type they have been explicitly configured to execute. Otherwise, I return some kind of error message or, in some cases, serve the contents of the file as plain text instead:

```text
GET / static/ exploit . HTTP/ I . I
```

```text
Host: normal—website . com
```

HTTP/ 1.1 200 0K Content -Type: text/ plain Content —Length: 39

<?php echo system ($ GET [ ' command'] ) ;

This behavior is potentially interesting in its own right, as it may provide a way to leak source code, but it nullifies any attempt to c shell. This kind of configuration often differs between directories. A directory to which user-supplied files are uploaded will likel Auve-m controls than other locations on the filesystem that are assumed to be out of reach for end users. If you can find a way t upload different directory that's not supposed to contain user-supplied files, the server may execute your script after all.

---

Web shell upload via path traversal

You should also note that even though you may send all of your requests to the same domain name, this often points to a reverse of some kind, such as a load balancer. Your requests will often be handled by additional servers behind the scenes, which may al: configured differently.

Insufficient blacklisting of dangerous file types One of the more obvious ways of preventing users from uploading malicious scripts is to blacklist potentially dangerous file extens . php . The practice of blacklisting is inherently flawed as it's difficult to explicitly block every possible file extension that could be execute code. Such blacklists can sometimes be bypassed by using lesser known, alternative file extensions that may still be exe as .php5 , . shtml , and so on.

Overriding the server configuration As we discussed in the previous section, servers typically won't execute files unless they have been configured to do so. For exar an Apache server will execute PHP files requested by a client, developers might have to add the following directives to their / etc/ apache2/apache2 . conf file: LoadModuIe php module /usr/ Iib/apache2 / modules/ Iibphp . so AddType application/ x—httpd—php . php

Many servers also allow developers to create special configuration files within individual directories in order to override or add to c the global settings. Apache servers, for example, will load a directory-specific configuration from a file called .htaccess if one i Similarly, developers can make directory-specific configuration on IIS servers using a web . config file. This might include direct the following, which in this case allows JSON files to be served to users:

```text
<staticContent>
```

```text
<mimeMap fileExtension=" . j son" mimeType=" application/ j son" / >
```

&lt; / staticContent&gt;

Web servers use these kinds of configuration files when present, but you're not normally allowed to access them using HTTP req However, you may occasionally find servers that fail to stop you from uploading your own malicious configuration file. In this case, file extension you need is blacklisted, you may be able to trick the server into mapping an arbitrary, custom file extension to an ex MIME type.

Web shell upload via extension blacklist bypass

Obfuscating file extensions Even the most exhaustive blacklists can potentially be bypassed using classic obfuscation techniques. Let's say the validation coc sensitive and fails to recognize that exploit .pHp is in fact a . php file. If the code that subsequently maps the file extension ti is not case sensitive, this discrepancy allows you to sneak malicious PHP files past validation that may eventually be executed b) You can also achieve similar results using the following techniques:

Provide multiple extensions. Depending on the algorithm used to parse the filename, the following file may be interpreted as file or JPG image: exploit . php . jpg

- Add trailing characters. Some components will strip or ignore trailing whitespaces, dots, and suchlike: exploit . php.

- Try using the URL encoding (or double URL encoding) for dots, forward slashes, and backward slashes. If the value isn't dec

validating the file extension, but is later decoded server-side, this can also allow you to upload malicious files that would othe blocked: exploit%2Ephp

---

Java, but the server processes the file using lower-level functions in C/C++, for example, this can cause discrepancies in wh. as the end of the filename: exploit. asp; . jpg or exploit. asp%OO .jpg Try using multibyte unicode characters, which may be converted to null bytes and dots after unicode conversion or normalize Sequences like xco x2E , xC4 XAE or xco XAE may be translated to x2E if the filename parsed as a UTF-8 string, bu converted to ASCII characters before being used in a path.

Other defenses involve stripping or replacing dangerous extensions to prevent the file from being executed. If this transformation recursively, you can position the prohibited string in such a way that removing it still leaves behind a valid file extension. For exam what happens if you strip . php from the following filename: exploit .p . phphp

This is just a small selection of the many ways it's possible to obfuscate file extensions.

Web shell upload via obfuscated file extension

Flawed validation of the file's contents Instead of implicitly trusting the Content-Type specified in a request, more secure servers try to verify that the contents of the match what is expected. In the case of an image upload function, the server might try to verify certain intrinsic properties of an image, such as its dimensio uploading a PHP script, for example, it won't have any dimensions at all. Therefore, the server can deduce that it can't possibly be and reject the upload accordingly. Similarly, certain file types may always contain a specific sequence of bytes in their header or footer. These can be used like a fin! signature to determine whether the contents match the expected type. For example, JPEG files always begin with the bytes FF 1 This is a much more robust way of validating the file type, but even this isn't foolproof. Using special tools, such as ExifTool, it can create a polyglot JPEG file containing malicious code within its metadata.

Remote code execution via polyglot web shell upload

Exploiting file upload race conditions Modern frameworks are more battle-hardened against these kinds of attacks. They generally don't upload files directly to their inte destination on the filesystem. Instead, they take precautions like uploading to a temporary, sandboxed directory first and randomiz to avoid overwriting existing files. They then perform validation on this temporary file and only transfer it to its destination once it is to do so. That said, developers sometimes implement their own processing of file uploads independently of any framework. Not only is this to do well, it can also introduce dangerous race conditions that enable an attacker to completely bypass even the most robust vali For example, some websites upload the file directly to the main filesystem and then remove it again if it doesn't pass validation. Tl behavior is typical in websites that rely on anti-virus software and the like to check for malware. This may only take a few millisecc the short time that the file exists on the server, the attacker can potentially still execute it. These vulnerabilities are often extremely subtle, making them difficult to detect during blackbox testing unless you can find a way relevant source code.

Web shell upload via race condition

Race conditions in URL-based file uploads

---

the internet and create a local copy before it can perform any validation. As the file is loaded using HTTP, developers are unable to use their framework's built-in mechanisms for securely validating files. may manually create their own processes for temporarily storing and validating the file, which may not be quite as secure. For example, if the file is loaded into a temporary directory with a randomized name, in theory, it should be impossible for an attac any race conditions. If they don't know the name of the directory, they will be unable to request the file in order to trigger its execu other hand, if the randomized directory name is generated using pseudo-random functions like PHP's uniqid ( ) , it can potentia forced. To make attacks like this easier, you can try to extend the amount of time taken to process the file, thereby lengthening the wind0\ forcing the directory name. One way of doing this is by uploading a larger file. If it is processed in chunks, you can potentially take this by creating a malicious file with the payload at the start, followed by a large number of arbitrary padding bytes.

## Exploiting file upload vulnerabilities without remote code execution

In the examples we've looked at so far, we've been able to upload server-side scripts for remote code execution. This is the most consequence of an insecure file upload function, but these vulnerabilities can still be exploited in other ways.

Uploading malicious client-side scripts Although you might not be able to execute scripts on the server, you may still be able to upload scripts for client-side attacks. For you can upload HTML files or SVG images, you can potentially use tags to create stored XSS payloads. If the uploaded file then appears on a page that is visited by other users, their browser will execute the script when it tries to rende Note that due to same-origin policy restrictions, these kinds of attacks will only work if the uploaded file is served from the same o you upload it.

Exploiting vulnerabilities in the parsing of uploaded files If the uploaded file seems to be both stored and served securely, the last resort is to try exploiting vulnerabilities specific to the pa processing of different file formats. For example, you know that the server parses XML-based files, such as Microsoft Office . doc files, this may be a potential vector for XXE injection attacks.

## Uploading files using PUT

It's worth noting that some web servers may be configured to support PUT requests. If appropriate defenses aren't in place, this alternative means of uploading malicious files, even when an upload function isn't available via the web interface.

```text
PUT / images/exploit.php HTTP/ 1.1
```

```text
Host: vulnerable—website . com
```

Content—Type: application/x-httpd—php Content —Length: 49

<?php echo file get contents (' / path/ to/ file');

Tip

You can try sending OPTIONS requests to different endpoints to test for any that advertise support for the PUT method

## How to prevent file upload vulnerabilities

Allowing users to upload files is commonplace and doesn't have to be dangerous as long as you take the right precautions. In ger effective way to protect your own websites from these vulnerabilities is to implement all of the following practices:

- Check the file extension against a whitelist of permitted extensions rather than a blacklist of prohibited ones. It's mu h easier

which extensions you might want to allow than it is to guess which ones an attacker might try to upload.

---

- Rename uploaded files to avoid collisions that may cause existing files to be overwritten.

- Do not upload files to the server's permanent filesystem until they have been fully validated.

- As much as possible, use an established framework for preprocessing file uploads rather than attempting to write your own v

mechanisms.

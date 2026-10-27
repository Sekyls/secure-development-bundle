# What is XXE (XML external entity) injec...rial & Examples

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `47d65a048729d53c27125967cd2864dfde8066fe607ea761ef3b643209bde1af`.
> Conversion: Local English OCR, all 6 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


XXE injection

## XML external entity (XXE) injection

## and exploit various kinds of XXE injection, and summarize how to prevent XXE injection attacks.

## What is XML external entity injection?

## XML external entity injection (also known as XXE) is a web security vulnerability that allows an attacker to interfere with an

## application's processing of XML data. It often allows an attacker to view files on the application server filesystem, and to

## interact with any back-end or external systems that the application itself can access.

## In some situations, an attacker can escalate an XXE attack to compromise the underlying server or other back-end

## infrastructure, by leveraging the XXE vulnerability to perform server-side request forgery (SSRF) attacks.

&lt; ! DOCTYPE stockCheck [ &lt; ! ENTITY xxe SYSTEM "file: /// etc/ ] &gt; &lt; stockCheck&gt;<productId &xxe ; < / / s tockCheck

root : x: O : O : root : / root : /bin/bash daemon : x : 1 : 1 : daemon : /usr/sbin : /bin/ sh bin : x: 2 : 2 : bin : / bin : /bin/sh sys : x: : sys : /dev : / bin/ sh sync : x: 4 : 65534 : sync: /bin : /bin/ sync games ; x ; 5 : 60 : games /usr/games : /bin/ sh man : x: 6 : 12 man : /var/cache/man /bin/ sh Ip : x : 7 : 7 : Ip : /var/spool/lpd: /bin/sh mail : x: 8 : 8 : mail : /var/mail : /bin/sh news : x: 9 : 9 : news : Ivar/ spool/news : /bin/sh uucp : x: IO : IO : uucp : Ivar/ spool/uucp : /bin/sh * sensitive data proxy: x: 13 : 13 : proxy: / bin : / bin/ sh www—data : x : 33 : 33 : www—data: / var/www:/bin/sh backup : x : 34 : 34 : backup : / vac / backups : /bin/ sh

## If you're already familiar with the basic concepts behind XXE vulnerabilities and just want to practice exploiting them

## on some realistic, deliberately vulnerable targets, you can access all of the labs in this topic from the link below.

- View all XXE labs

## How do XXE vulnerabilities arise?

## Some applications use the XML format to transmit data between the browser and the server. Applications that do this

## virtually always use a standard library or platform API to process the XML data on the server. XXE vulnerabilities arise

---

features even if they are not normally used by the application.

- Learn about the XML format, DTDs, and external entities

XML external entities are a type of custom XML entity whose defined values are loaded from outside of the DTD in which they are declared. External entities are particularly interesting from a security perspective because they allow an entity to be defined based on the contents of a file path or URL.

## What are the types of XXE attacks?

There are various types of XXE attacks:

- Exploiting XXE to retrieve files, where an external entity is defined containing the contents of a file, and returned in

the application's response. Exploiting XXE to perform SSRF attacks, where an external entity is defined based on a URL to a back-end system. Exploiting blind XXE exfiltrate data out-of-band, where sensitive data is transmitted from the application server to a system that the attacker controls. Exploiting blind XXE to retrieve data via error messages, where the attacker can trigger a parsing error message containing sensitive data.

## Exploiting XXE to retrieve files

To perform an XXE injection attack that retrieves an arbitrary file from the server's filesystem, you need to modify the submitted XML in two ways:

- Introduce (or edit) a DOCTYPE element that defines an external entity containing the path to the file.

Edit a data value in the XML that is returned in the application's response, to make use of the defined external entity.

For example, suppose a shopping application checks for the stock level of a product by submitting the following XML to the server: &lt; ? xml ver s 1 O" encoding "UTF 8"? &gt; &lt; stockCheck&gt;

The application performs no particular defenses against XXE attacks, so you can exploit the XXE vulnerability to retrieve the / etc/ passwd file by submitting the following XXE payload: &lt; ? xml O" &lt; ! DOCTYPE foo &lt; ! ENTITY xxe SYSTEM ] &gt; t &xxe ; &lt; /product / stockCheck&gt;

This XXE payload defines an external entity &xxe; whose value is the contents of the /etc/passwd file and uses the entity within the product ld value. This causes the application's response to include the contents of the file: Invalid product I D: root: / root: /bin/bash daemon : x: 1 : 1 : daemon : /usr/ sbin : /usr/ sbin/nologin /bin: /usr/sbin/nologin

Note With real-world XXE vulnerabilities, there will often be a large number of data values within the submitted XML, any one of which might be used within the application's response. To test systematically for XXE vulnerabilities, you will

---

whether it appears within the response.

Exploiting XXE using external entities to retrieve files

## Exploiting XXE to perform SSRF attacks

Aside from retrieval of sensitive data, the other main impact of XXE attacks is that they can be used to perform server-side request forgery (SSRF). This is a potentially serious vulnerability in which the server-side application can be induced to make HTTP requests to any URL that the server can access. To exploit an XXE vulnerability to perform an SSRF attack, you need to define an external XML entity using the URL that you want to target, and use the defined entity within a data value. If you can use the defined entity within a data value that is returned in the application's response, then you will be able to view the response from the URL within the application's response, and so gain two-way interaction with the back-end system. If not, then you will only be able to perform blind SSRF attacks (which can still have critical consequences). In the following XXE example, the external entity will cause the server to make a back-end HTTP request to an internal system within the organization's infrastructure: DOCTYPE foo [ &lt; ! ENTITY xxe SYSTEM "http://internal . ] &gt;

Exploiting XXE to perform SSRF attacks

## Blind XXE vulnerabilities

Many instances of XXE vulnerabilities are blind. This means that the application does not return the values of any defined external entities in its responses, and so direct retrieval of server-side files is not possible. Blind XXE vulnerabilities can still be detected and exploited, but more advanced techniques are required. You can sometimes use out-of-band techniques to find vulnerabilities and exploit them to exfiltrate data. And you can sometimes trigger XML parsing errors that lead to disclosure of sensitive data within error messages.

- Finding and exploiting blind XXE vulnerabilities

## Finding hidden attack surface for XXE injection

Attack surface for XXE injection vulnerabilities is obvious in many cases, because the application's normal HTTP traffic includes requests that contain data in XML format. In other cases, the attack surface is less visible. However, if you look in the right places, you will find XXE attack surface in requests that do not contain any XML.

Xlnclude attacks Some applications receive client-submitted data, embed it on the server-side into an XML document, and then parse the document. An example of this occurs when client-submitted data is placed into a back-end SOAP request, which is then processed by the backend SOAP service. In this situation, you cannot carry out a classic XXE attack, because you don't control the entire XML document and so cannot define or modify a DOCTYPE element. However, you might be able to use Xlnclude instead. X Include is a part of the XML specification that allows an XML document to be built from sub-documents. You can place an X Includ

---

single item of data that is placed into a server-side XML document. To perform an X Include attack, you need to reference the X Include namespace and provide the path to the file that you wish to include. For example: &lt; foo xmlns :xi="http://www.w3 &lt; xi : include /&gt;&lt;/f00&gt;

Exploiting Xlnclude to retrieve files

XXE attacks via file upload Some applications allow users to upload files which are then processed server-side. Some common file formats use XML or contain XML subcomponents. Examples of XML-based formats are office document formats like DOCX and image formats like SVG. For example, an application might allow users to upload images, and process or validate these on the server after they are uploaded. Even if the application expects to receive a format like PNG or JPEG, the image processing library that is being used might support SVG images. Since the SVG format uses XML, an attacker can submit a malicious SVG image and so reach hidden attack surface for XXE vulnerabilities.

Exploiting XXE via image file upload

XXE attacks via modified content type Most POST requests use a default content type that is generated by HTML forms, such as application/x-www form ur 1 encoded . Some web sites expect to receive requests in this format but will tolerate other content types, including XML. For example, if a normal request contains the following:

```text
POST / action HTTP/ 1.0
```

Content—Type: application/x—www—form—urlencoded Content —Length : 7

foo=bar

Then you might be able submit the following request, with the same result:

```text
POST /action HTTP/ 1.0
```

Content —Type: text/ xml Content Length: 52

< ? xml O"

If the application tolerates requests containing XML in the message body, and parses the body content as XML, then you can reach the hidden XXE attack surface simply by reformatting requests to use the XML format.

## How to find and test for XXE vulnerabilities

The vast majority of XXE vulnerabilities can be found quickly and reliably using Burp Suite's web vulnerability scanner. Manually testing for XXE vulnerabilities generally involves:

- Testing for file retrieval by defining an external entity based on a well-known operating system file and using that

entity in data that is returned in the application's response.

---

monitoring for interactions with that system. Burp Collaborator is perfect for this purpose.

- Testing for vulnerable inclusion of user-supplied non-XML data within a server-side XML document by using an

Xlnclude attack to try to retrieve a well-known operating system file.

Note Keep in mind that XML is just a data transfer format. Make sure you also test any XML-based functionality for other vulnerabilities like XSS and SQL injection. You may need to encode your payload using XML escape sequences to avoid breaking the syntax, but you may also be able to use this to obfuscate your attack in order to bypass weak defences.

## How to prevent XXE vulnerabilities

Virtually all XXE vulnerabilities arise because the application's XML parsing library supports potentially dangerous XML features that the application does not need or intend to use. The easiest and most effective way to prevent XXE attacks is to disable those features. Generally, it is sufficient to disable resolution of external entities and disable support for x Include . This can usually be done via configuration options or by programmatically overriding default behavior. Consult the documentation for your XML parsing library or API for details about how to disable unnecessary capabilities.

- Find XXE vulnerabilities using Burp Suite's web vulnerability scanner

---

Find XSS vulnerabilities using

Burp Su Customers Insights

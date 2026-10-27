# Examining the database in SQL injection attacks

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `184528e41e669719e53e45b8fdfe928c6bc49eeab1ae8c78de3d9c5775788f38`.
> Conversion: Local English OCR, all 3 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


Examining the database

SQL injection

The type and version of the database software. The tables and columns that the database contains.

## Querying the database type and version

You can potentially identify both the database type and version by injecting provider-specific queries to see if one works The following are some queries to determine the database version for some popular database types: Database type Query Microsoft, MySQL SELECT @@version Oracle

```text
SELECT * FROM v$version
```

PostgreSQL

```text
SELECT version ( )
```

For example, you could use a UNION attack with the following input:

```text
UNION SELECT
```

This might return the following output. In this case, you can confirm that the database is Microsoft SQL Server and see the versior Microsoft SQL Server 2016 (SP2) (KB4052908) 13.0. 5026.0 (X64) Mar 18 2018 Copyright (c) Mucrosoft Corporation Standard Edition (64—bit) on Windows Server 2016 Standard 10.0 &lt;X64&gt; (Build 14393: (Hypervi

SQL injection attack, querying the database type and version on Oracle

SQL injection attack, querying the database type and version on MySQL and Microsoft

## Listing the contents of the database

Most database types (except Oracle) have a set of views called the information schema. This provides information about the data For example, you can query information schema. tables to list the tables in the database:

```text
SELECT * FROM information schema . tables
```

This returns output like the following:

---

MyDatabase dbo BASE TABLE MyDatabase dbo Users BASE TABLE MyDatabase dbo Feedback BASE TABLE

This output indicates that there are three tables, called Products , Users , and Feedback You can then query information schema. columns to list the columns in individual tables:

```text
SELECT * FROM information schema . columns WHERE table name —
```

'Users'

This returns output like the following: TABLE CATALOG TABLE SCHEMA TABLE NAME COLUMN NAME DATA TYPE

My Database dbo Userld int MyDatabase dbo Users Usern ame varchar MyDatabase dbo Users Password varchar

This output shows the columns in the specified table and the data type of each column.

SQL injection attack, listing the database contents on non-Oracle databases

Listing the contents of an Oracle database On Oracle, you can find the same information as follows:

You can list tables by querying all tables :

```text
SELECT * FROM all tables
```

You can list columns by querying all tab columns

```text
SELECT * FROM all tab columns WHERE table name
```

'USERS '

SQL injection attack, listing the database contents on Oracle

---



## What is SQL Injection (SQLi)?

**SQL Injection (SQLi)** is a type of attack where an attacker exploits vulnerabilities in an application’s software by injecting malicious SQL code into input fields or parameters. This allows the attacker to manipulate the SQL queries executed by the database, leading to unauthorized data access, modification, or even complete control over the database. SQL Injection is one of the most common and dangerous web application vulnerabilities, often resulting in significant data breaches and security incidents.

## Types of SQL Injection

SQL Injection attacks can be categorized into several types based on the techniques used and the application's response:

### 1. Error-based SQL Injection
**Error-based SQL Injection** is a technique that relies on triggering database errors to retrieve information about the structure of the database. By deliberately causing errors, attackers can extract sensitive data from the database by analyzing the error messages returned.

### 2. Double Query Injection
**Double Query Injection** involves injecting two or more SQL queries in a single execution context. This technique can be used to execute multiple commands in one go, often to manipulate the database in unexpected ways or to retrieve hidden data.

### 3. Blind SQL Injection
**Blind SQL Injection** occurs when an attacker is able to send malicious SQL queries to the database, but the results of the query are not directly visible to them. The attacker deduces information by observing the behavior of the application, such as changes in response times or content.

#### 3.1 Boolean-based Blind SQL Injection
**Boolean-based Blind SQL Injection** is a type of Blind SQL Injection where the attacker sends a SQL query to the database and determines the result based on whether the query returns a true or false response. This method is used to infer information from the database by checking how the application reacts to true or false conditions.

#### 3.2 Time-based Blind SQL Injection
**Time-based Blind SQL Injection** is a technique where the attacker uses SQL queries that cause a delay in the database's response. By measuring the time it takes for the application to respond, the attacker can infer whether the query was true or false, allowing them to extract information without direct feedback from the database.

### 4. SQL Injection Authentication Bypass
**SQL Injection Authentication Bypass** is a technique where an attacker uses SQL Injection to bypass the authentication mechanism of an application. By injecting specially crafted SQL queries, attackers can gain unauthorized access to the system without needing valid credentials.

### 5. Data Dumping via SQL Injection
**Data Dumping via SQL Injection** is a process where an attacker uses SQL Injection to retrieve large amounts of data from the database. The attacker crafts SQL queries to systematically extract data, which can then be analyzed or exploited.

### 6. SQL Injection + Local File Inclusion (LFI) = Remote Code Execution (RCE)
**SQL Injection combined with Local File Inclusion (LFI) to achieve Remote Code Execution (RCE)** involves using SQL Injection to exploit the database and then combining it with LFI vulnerabilities to execute arbitrary code on the server. This can lead to full control of the server.

### 7. SQL Injection in Insert Statements
**SQL Injection in Insert Statements** involves injecting malicious SQL code into the `INSERT` statements of a database. This technique can be used to manipulate the data being inserted, such as inserting additional rows, modifying existing data, or causing other unintended database behavior.

### 8. SQL Injection in Update Statements
**SQL Injection in Update Statements** occurs when an attacker injects malicious SQL code into `UPDATE` statements. This can be used to change existing data in the database, often with the goal of altering information to gain unauthorized access or corrupt data.

### 9. SQL Injection in Delete Statements
**SQL Injection in Delete Statements** involves injecting malicious SQL code into `DELETE` statements. This can be used to remove data from the database, potentially causing data loss, denial of service, or other destructive actions.

### 10. Second Order SQL Injection
**Second Order SQL Injection** is a type of SQL Injection where the injected payload is stored in the database and is executed later when the stored data is processed by the application. Unlike traditional SQL Injection, where the attack is immediate, Second Order SQL Injection requires a different context to trigger the payload.

### 11. Out-of-band SQL Injection (OOB SQLi)
**Out-of-band SQL Injection (OOB SQLi)** is a technique where the attacker uses a different communication channel to receive the results of the injected SQL query. Instead of relying on the application's response, OOB SQLi can send data to a different server controlled by the attacker, allowing them to extract data without directly interacting with the application.


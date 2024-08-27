

### SQL Injection in DELETE

SQL Injection vulnerabilities can also impact `DELETE` statements. Such vulnerabilities occur when user inputs are used to formulate `DELETE` queries without proper sanitization or validation, allowing attackers to delete unauthorized data or potentially affect the integrity of the database.

#### Concept

The `DELETE` statement is used to remove records from a database. If user input is directly included in this statement without proper validation or parameterization, an attacker can inject malicious SQL code to alter the intended operation.

#### Example Vulnerability

Consider a scenario where a user can delete their profile from an application. The application might use a URL like:

```
http://example.com/delete_profile.php?id=1
```

The corresponding SQL query might be:

```sql
DELETE FROM users WHERE id = 1;
```

If the `id` parameter is vulnerable to SQL Injection, an attacker can manipulate this parameter to execute unintended queries.

#### Exploiting the Vulnerability

1. **Basic Injection:**

   An attacker might inject SQL code into the `id` parameter to delete multiple records or perform other unauthorized operations. For example:

   ```sql
   1; DELETE FROM users -- 
   ```

   This results in:

   ```sql
   DELETE FROM users WHERE id = 1; DELETE FROM users -- ;
   ```

   This query deletes all records from the `users` table, not just the one with `id = 1`.

2. **Advanced Injection:**

   An attacker could use more sophisticated payloads to manipulate the `DELETE` operation. For example:

   ```sql
   1' OR '1'='1' -- 
   ```

   This modifies the query to:

   ```sql
   DELETE FROM users WHERE id = 1' OR '1'='1' -- ;
   ```

   Here, the condition `'1'='1'` is always true, resulting in the deletion of all records from the `users` table.

3. **Blind Injection:**

   In cases where the output is not visible, attackers might use blind SQL injection techniques to infer the structure of the database or other information. For example:

   ```sql
   1' AND (SELECT IF(1=1, SLEEP(5), 0)) -- 
   ```

   If the application pauses for 5 seconds, it indicates that the injection was successful.

#### Prevention

To protect against SQL Injection in `DELETE` statements:

1. **Use Prepared Statements:**
   Prepared statements with parameterized queries prevent user inputs from being treated as executable code.
   
   Example in PHP with PDO:
   ```php
   $stmt = $pdo->prepare('DELETE FROM users WHERE id = :id');
   $stmt->execute(['id' => $id]);
   ```

2. **Input Validation and Sanitization:**
   Ensure that all user inputs are validated against expected formats and sanitized to prevent malicious input from affecting SQL queries.

3. **Least Privilege Principle:**
   Limit the database user account permissions to only those required for specific operations. Avoid using high-privilege accounts for regular application operations.

4. **Use ORM (Object-Relational Mapping):**
   ORMs provide an abstraction layer that helps prevent direct SQL execution, reducing the risk of SQL Injection.

5. **Escape Inputs:**
   Properly escape any user inputs included in dynamic SQL queries. However, prepared statements are preferred for their enhanced security.

#### Conclusion

SQL Injection in `DELETE` statements represents a significant risk to database integrity and security. By understanding how these vulnerabilities can be exploited and implementing best practices for prevention, you can safeguard your applications against unauthorized data manipulation and maintain the security of your database.


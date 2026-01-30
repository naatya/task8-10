# task8-10
SQL Injection Practical Exploitation

**Vulnerability Assessment Report: SQL Injection**

**1. Executive Summary**

This report documents the identification and exploitation of a SQL Injection (SQLi) vulnerability found on the target web application. The assessment was conducted in a controlled environment (TryHackMe) to understand the security impact and provide remediation steps.

**2. Attack Flow Documentation**

**Step 1**: Identification of Injectable Parameters

**Target URL:** http://<Target_IP>/vulnerable.php?id=1 (Replace with your actual IP)

**Method:** Manual testing by appending a single quote (') to the parameter.

**Observation:** The application returned a SQL syntax error, confirming that the id parameter is not properly sanitized.

**Step 2**: Automated Exploitation (SQLMap)

I used SQLMap on Kali Linux to automate the extraction of data.

**Command used to list databases:**

sqlmap -u "http://<Target_IP>/page.php?id=1" --dbs --batch

**Step 3:** Data Extraction
After identifying the database staff_db, I proceeded to extract tables and sensitive user records.

**Command to dump user data:**

sqlmap -u "http://<Target_IP>/page.php?id=1" -D staff_db -T users --dump --batch

**3. Impact Analysis**

The impact of this vulnerability is CRITICAL. An attacker can:

**Confidentiality Breach**: Access sensitive user information, including hashed passwords and personal details.

**Authentication Bypass:** Gain unauthorized access to administrative accounts.

**Data Integrity**: Potentially modify or delete records within the database.

**4. Suggested Fixes (Remediation)**

To secure the application against SQL Injection, the following measures are recommended:

**A. Use Prepared Statements (Parameterized Queries)**

Instead of building queries with string concatenation, use prepared statements. This ensures the database treats user input as data, not as executable code.

**Example (PHP):**

PHP
// Secure way using PDO
$stmt = $pdo->prepare('SELECT * FROM users WHERE id = :id');
$stmt->execute(['id' => $_GET['id']]);
$user = $stmt->fetch();

**B. Input Validation & Sanitization**

Implement strict allow-lists for user input.

Ensure that numerical parameters (like id) only accept integer values.

**C. Principle of Least Privilege**

Configure the database user with minimum necessary permissions. The web application's database user should not have DROP or FILE privileges unless absolutely required.

**5. Conclusion**

The SQL Injection vulnerability was successfully exploited, demonstrating a significant risk to the application's data security. Implementing Prepared Statements is the most effective fix to prevent such attacks in the future.

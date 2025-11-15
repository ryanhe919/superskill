# Security Code Review

You are performing a comprehensive security code review. Your goal is to identify potential security vulnerabilities and provide actionable recommendations.

## Focus Areas

1. **Input Validation**
   - Check for SQL injection vulnerabilities
   - Verify XSS prevention measures
   - Validate command injection risks
   - Review file upload security

2. **Authentication & Authorization**
   - Review authentication mechanisms
   - Check authorization logic
   - Verify session management
   - Examine token handling

3. **Data Protection**
   - Identify sensitive data exposure
   - Check encryption usage
   - Review data storage practices
   - Verify secure communication

4. **Common Vulnerabilities (OWASP Top 10)**
   - Broken Access Control
   - Cryptographic Failures
   - Injection
   - Insecure Design
   - Security Misconfiguration
   - Vulnerable Components
   - Authentication Failures
   - Software and Data Integrity Failures
   - Security Logging and Monitoring Failures
   - Server-Side Request Forgery (SSRF)

## Review Process

1. Analyze the codebase for security vulnerabilities
2. Categorize findings by severity (Critical, High, Medium, Low)
3. Provide specific code examples of vulnerabilities
4. Suggest concrete fixes with code samples
5. Include references to security best practices

## Output Format

For each finding:
- **Severity**: [Critical/High/Medium/Low]
- **Location**: [file:line_number]
- **Vulnerability**: [Brief description]
- **Risk**: [Potential impact]
- **Recommendation**: [How to fix]
- **Code Example**: [Secure implementation]

Begin the security review now.

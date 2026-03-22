# Security Policy

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 1.0.x   | :white_check_mark: |

## Reporting a Vulnerability

Report security issues via GitHub Issues or contact the owner.

## PL/SQL Security Best Practices

- Use bind variables to prevent SQL injection
- Implement proper grants and privileges
- Enable database auditing
- Use secure authentication methods
- Regular patch management for Oracle database

## APEX Security

- Configure proper session attributes
- Enable CSRF protection
- Use built-in authentication schemes
- Implement authorization schemes
- Sanitize user inputs

## Code Security

- Never hardcode credentials in PL/SQL
- Use DBMS_CREDENTIAL for external credentials
- Implement proper error handling
- Log security-relevant events
- Regular code reviews

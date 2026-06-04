# Security

> **The top priority is the security and privacy of your data, it sits above ALL other requirements.**

We adhere to the latest, industry-leading privacy and security standards, drawing guidance from organizations including the Open Web Application Security Project (OWASP) and the Electronic Frontier Foundation (EFF).

## Table of Contents
- [Security Best Practices](#security-best-practices)
- [Authentication & Authorization Overview](#authentication--authorization-overview)
- [Security Features](#security-features)
- [Vulnerability Disclosure](#vulnerability-disclosure)
- [Security Audits](#security-audits)

## Security Best Practices
- **No External Access:** While every effort has been made, we recommend you **do not** allow external Internet access to Athena at any time
- **Turn Off Sign Ups:** While logged in as an Administrator, navigate to Settings -> Admin Panel -> Application -> Settings -> General and **turn off** "Allow new sign ups"
- **Enable MFA:** Multi Factor Authentication adds a secondary layer of security to your login and data, we recommend enabling it upon sign up
- **Enable HTTPS:** By default, HTTPS and TLS v1.3 is enabled via the Nginx proxy service and randomly generated SSL certificates. Although the data transferred between your computer and the Athena Docker container should only be done over an internal LAN, these can still have untrusted devices (looking at you IoT!) that can snoop. Ensure HTTPS is always enabled for full end-to-end encryption of all data
- **Keep Athena Updated:** Please ensure you update the Docker images promptly to ensure all security bugs are squashed

## Authentication & Authorization Overview
### Server
- **Signup & Signin:** Returns `accessToken` in the JSON response body and `refreshToken` in a secure cookie
- **Refresh:** Expects the `refreshToken` in the request cookie and returns a new `accessToken` and `refreshToken`
- **Signout:** Revokes the `refreshToken` cookie on the server side and client side
- **Auth Middleware:** Validates the `accessToken` provided in the Authorization: Bearer header for protected routes

### Client
- **Access Token:** Stored in memory only (not local storage!) and used for any authenticated API requests
- **Refresh Token:** Stored in a HttpOnly, Secure cookie to protect against XSS attacks
- **Automatic Refresh:** Refreshes the `accessToken` when it expires every 15 minutes using the `refreshToken`

## Security Features

### User Privacy
- Private and anonymous by default. No personally identifiable information required. Ever
- No logging of any events (except authentication and the currently logged in sessions for enhanced security features), to enable additional privacy protection and anonymity
- Failed sign in attempts are logged and rate limiting is applied to stop brute force attacks
- Can be Self Hosted via a freely downloadable Docker image for full data and privacy sovereignty
- All user data can be viewed, downloaded and deleted easily via Settings -> Data & privacy giving you instant, complete control
- See all of your active sessions, when they expire and remotely sign out of any you don’t want

### Authentication
- Password management fully enforced to NIST, NCSC and OWASP best practices
- Usernames are case insensitive and unique
- Password minimum length requirements set to 15+ characters by default
- Password maximum length supported up to 255 characters with added protection against long password DoS attacks
- No password complexity rules, no regular password resets and no security questions
- Password fields fully accept “copy and paste” enabling users to easily input long, randomly generated passwords
- Included password best practices tips to help users create strong passwords upon registration
- Password verification uses dependency free, inbuilt `password_verify` function
- Sign in, as well as all other pages, are only accessible via HTTPS for secure username and password communication
- App based Multi Factor Authentication (MFA) support with no SMS support allowed
- Protection against automated or brute force attacks by limiting sign in attempts and account lockouts
- Sessions are invalidated on signout, and refresh tokens are rotated to prevent session hijacking

### Password Storage
- Password storage uses state of the art Argon2id hashing to secure against database breaches
- Automatic salting of all hashed passwords as part of the Argon2id standard

### Authorization
- Authorization logic is built to be “deny by default” to ensure even when unexpected events occur resources are still secure
- Authorization to any resource is always tested on every request made
- Authorization checks are built into both client side (for an improved user experience) and verified on the server side too
- Authorization logic is centralized to ensure application compliance across all parts of the application
- JWTs are signature protected and built to the latest standards for the best user experience and security
- Secret key used to encode all JWT’s is long, user configurable and generated using a secure source of randomness
- JWT access token is stored in memory only (not cookies or local storage) to ensure protection against CSRF & XSS attacks
- JWT access and refresh tokens are refreshed every 15 minutes for enhanced security
- JWT’s are sent via standard Authorization Bearer header to ensure they are always encrypted
- Encoding and decoding of JWT’s uses standardized, dependency free functions
- Standard JWT claims such as iss, aud, exp and nbf are built into every token and validated on every request via the centralized JwtManager

### Input Validation
- All client side forms have strict and thorough data validation to prevent incorrect data being sent to the server
- Data is validated immediately as it’s read into the system to prevent attacks as early as possible
- Input validation code is standardized to ensure application compliance across all parts of the application
- Data is validated on both syntactical (e.g., a date field contains a date) and semantic (e.g., start date is before end date) levels
- Data is validated to ensure it adheres to logical length and value types

### Database
- The MariaDB database is hardened according to their given hardening guidelines
- The MariaDB application itself is not run under the systems root account and uses the dedicated “mysql” user instead
- The root database account is protected with a strong and unique password
- The database account used by the API is not a root, sa or SYS level account and does not have administrative rights over the database
- The database account used by the API is only allowed access to the specific database
- As per the Principle of Least Privilege recommended by OWASP and NIST, the database account is only allowed SELECT, INSERT, UPDATE and DELETE permissions 
- Database authentication details are separated out into their own, private configuration file enabling enhanced security
- Private configuration file containing database authentication details is stored outside the web root
- Default databases and accounts have been removed as part of using the MariaDB Docker container
- All database queries use prepared statements with variable binding to protect against SQL injection attacks
- Database query code built using PHP PDO to enable support for a wide range of database back ends

### Transport Layer
- Where required, only the most up to date and secure TLS version 1.3 is supported
- Where required, only the most up to date and secure GCM ciphers are supported
- HTTPS using TLS is enabled site wide and on all pages, to enable enhanced security
- Supports HTTP Strict Transport Security (HSTS), ensuring HTTPS is fully enforced application wide
- All cookies are marked with the “Secure” attribute, ensuring they are only sent over encrypted HTTPS connections
- Caching of sensitive data is prevented by setting cache headers to not cache any of the data transferred

### NodeJS
- Third party dependencies are minimized and kept up to date to ensure they are patched for the latest security updates
- Commonly known dangerous functions are avoided to ensure they can’t be used in attacks
- Regular expressions are used sparingly and fully reviewed to ensure mitigation against ReDoS attacks
- All code is developed to strict ESlint linting standards for increased security and code legibility
- All code is developed using strict type checking to enhance code quality and reduce errors
- All code is developed using ECMAScript version ES2022, ensuring unsafe and dangerous legacy features aren’t used
- Dependencies are regularly scanned for vulnerabilities using tools like `npm audit` and updated promptly

### RESTful API
- API only provides HTTPS endpoints for secure data transfer
- Each API endpoint allows only the HTTP request methods that are required
- Full suite of security headers passed with every API response matching OWASP recommendation, verified to receive A+ security rating
- Delivers appropriate and restricted Cross-Origin Resource Sharing (CORS) headers
- Passwords, security tokens, and API keys are never sent through the URL to ensure no sensitive information is logged or leaked
- Strict adherence to the standardized HTTP return status codes when replying to all requests
- Standardized API error responses that follow [RFC 7807](https://www.rfc-editor.org/rfc/rfc7807)
- Strict type checking (strict mode) is applied to all code files to improve type safety and reduce runtime bugs
- Strict Content Security Policy (CSP) to mitigate XSS attacks by restricting script and resource sources
- Rate limiting to prevent abuse and brute force attempts with both the rate limit and lock out periods being admin user configurable. Default is 10 requests/minute/IP for public and password related endpoints and 1,000 requests/minute/user for all other authenticated endpoints
- Comprehensive server logging to help catch and debug issues as well as detect improper access
- Automatic deletion of old logs via user configurable retention days and crond

### HTML5
- Location permissions are never asked for as no personally identifiable information is required. Ever.
- To increase security and prevent Tabnabbing, rel=”noopener noreferrer” tags are used on all external links
- All iframes use the sandbox attribute to ensure restricted permissions
- All sign in / sign up fields have autocomplete turned off to ensure credentials are not auto filled in by unauthorized users

## Vulnerability Disclosure
If you discover a security vulnerability, please report it by opening up a pull request.  We will do our best to promptly review and fix any security issues found.

## Security Audits
Currently, we do not perform any security audits of the code base and have not performed one. We of course welcome any help or suggestions to further increase the security of Athena.
